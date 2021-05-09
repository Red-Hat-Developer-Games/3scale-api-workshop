# Lab 203

## Jenkins CICD

### Create new OpenAPI specification file

* Duration: 30 mins
* Audience: Developers, Architects, Devops

## Overview

This sections reminds us, how to create an API definition based on OpenAPI 3.0.2, based on this specification file we are going to deploy a NodeJS app.

### Skipping The Lab

We know sometimes we don't have enough time to go over step by step on the labs.

If you are planning to skip this lab and follow the next one, we have already added the definition file in the here is a [link](https://raw.githubusercontent.com/misanche/3scaleworkshop-todo-app/main/src/openapi/openapi.yaml) to the specification generated in this lab.

### Environment

**URLs:**

Check with your instruction the *GUID* number of your current workshop environment. Replace the actual number on all the URLs where you find **GUID**.

Example in case of *GUID* = **1234**:

```bash
https://master.GUID.open.redhat.com
```

becomes =>

```bash
https://master.1234.open.redhat.com
```

**Credentials:**

Your username is your asigned user number and @openshiftworkshop.com. For example, if you are assigned user number **1**, your username is:

```bash
user1@openshiftworkshop.com
```

The password to login is always the same:

```bash
openshift
```

## Lab Instructions

### Step 1: Creating APIs with Apicurio Studio

1. Open a browser window and navigate to:

    ```bash
    http://apicurio-studio.apps.GUID.open.redhat.com/
    ```

2. Accept the self-signed certificate if you haven't: 

    1. If using Google Chrome click the **ADVANCED** link.

      ![selfsigned-cert](images/00-selfsigned-cert.png "Self-Signed Cert")

    2. Then click the **Proceed to..** link to accept the certificate and add the exception.

      ![00-selfsigned-cert-accept](images/00-selfsigned-cert-accept.png  "Self-Signed Cert Proceed")

3. Log in using your designated [user and password](#environment).

    ![design-login](images/design-01.png "Login")

4. Click on **APIs** in the left side navigation menu from the Dashboard page.

    ![design-apis](images/design-02.png "APIs")

5. Click on **Create New API**.

    ![design-new-api](images/design-03.png "Create New API")

6. Create a brand new Access Token with the following information:

    * Name: **Todo List**
    * Description: **Todo List Spec **
    * Type: **OpenAPI 3.0.2**
    * Template: **Blank API**

    ![todo-01](images/todo-01.png "Create API")

7.  On the next screen, Click on **Edit API**.

    ![todo-02](images/todo-02.png "Edit API")

8.  Now, we are going to create a new Data Type, click on **Add a data type**.

    ![todo-03](images/todo-03.png "Add data type")

9.   Fill the sections with the following data:

     * Name: **Item**
     * Description: **Item type**
     * Enter JSON example:

        ```json
            {
                "id": "3423432432",
                "name": "Bananas",
                "description": "Buy bananas at Carrefour"
            }
        ```

     * Choose to create a REST Resource with the Data Type: **No Resource**
    ![todo-08](images/todo-08.png "Data type")

10. Click **Save** button.
11. Click on **Add a path** button:
   ![todo-04](images/todo-04.png "Add a path")
12. Enter a valid path:
   * Path: **/items**
   ![todo-05](images/todo-05.png "Add items path")
13. Click **Add** button.
14. Under the operations sections, click on **GET** button and click **Add Operation** button, then fill the **Operation ID** with **getItems**.
   ![todo-06](images/todo-06.png "Add get operation")
   ![todo-13](images/todo-13.png "Add Operation ID")
15. The next step is add a new response for this GET operation, to do that, just click **Add a response** button.
   ![todo-07](images/todo-07.png "Add a response")
16. In the popup select **200 OK** Response Status Code and click **Add** button.
    ![todo-09](images/todo-09.png "200 status")
17. To remove all the warnings appeared in the 200 Status code Response, we need to fill the Description field, click **edit** button:
    Description: **valid response**
18. Click **Add a media type ** button in the same section.
    ![todo-10](images/todo-10.png "Add a media type")
19. Select **application/json** option and click **Add** button:
    ![todo-11](images/todo-11.png "Add")
20. Follow the steps as they appear in the image:
     * Type: **Array**
     * of: **Item**
21. Lastly, Add an example:
    * Name: **items**
    * Example:
        ```json
                [
            {
                "id": "123123",
                "name": "Buy Bananas",
                "description": "Buy Bananas"
            },
            {
                "id": "345345",
                "name": "Buy Chocolate",
                "description": "85% chocolate"
            }
        ]
        ```
### Step 2: Create POST method

We have already setup the GET `/items` endpoint, let's increase it and add a second endpoint using th POST method. This method will allow us to create new Items.

1.  Under the operations sections, click on **POST** button click **Add Operation** button then fill the **Operation ID** with **createItem** then .
2.  
   ![todo-20](images/todo-20.png "Add get operation")

   ![todo-21](images/todo-21.png "Add Operation ID")

2. With POST Operations, we need to send a body with the Item we want to create, to do that, click on **Add a request body**

   ![todo-22](images/todo-22.png "Add Request Body")

3. Now click on **Add a media type** button, choose on **Type** the **application/json**

   ![todo-23](images/todo-23.png "add application/json")

4. fill the form following the steps:

   * Required
   * Type: **Item**

   ![todo-24](images/todo-24.png "type steps")

5. Click on example and then **Add an example** button.

   ![todo-25](images/todo-25.png "add example")

6. Fill the popup with the data:

   * Name: **item**
   * Example:

   ```json
        {
            "id": "123123",
            "name": "Buy Bananas",
            "description": "Buy Bananas"
        }
    ```

    ![todo-26](images/todo-26.png "example item")

7. The next step is add a new response for this POST operation, to do that, just click **Add a response** button.

   ![todo-27](images/todo-27.png "Add a response")

8. In the popup select **201 OK** Response Status Code and click **Add** button.

    ![todo-28](images/todo-28.png "201 status")

17. To remove all the warnings appeared in the 200 Status code Response, we need to fill the Description field
    * Description: **Created description**

### Step 4: Secure the api
1. In order to secure the api with 3scale API Gateway, we are going to define a new security scheme, based on api-key, click **Add a security scheme** button under security schemes section:
![todo-34](images/todo-34.png "add scheme")
2. Fill the form with the following data:
    * Name: **api-key**
    * Description: **Use a 3scale API Key**
    * Type: **API Key**
    * Key Location: **HTTP header**
    * Name: **api-key**

![todo-33](images/todo-33.png "scheme data")
### Step 3: Save the Definition file

1. Finally, we have created the both endpoints needed for our application, to save the definition file, click **Todo List** breadcrumb.
    ![todo-30](images/todo-30.png "save")
2. Click **3 dots** and then **Download** button
    ![todo-31](images/todo-31.png "download")
3. Select the options as they appear, `yaml` format and without references:
    ![todo-32](images/todo-32.png "save-options")


## Steps Beyond

So, you want more? Have you tried to familiarize with 3scale-toolbox, try different commands.

## Summary

In this lab you have learned how to generate access tokens and how to get the `.3scalerc.yml`.

You can now proceed to [Lab 3](../lab203/#lab-3)

## Notes and Further Reading

* 3Scale API Gateway
  * [Tokens](https://access.redhat.com/documentation/en-us/red_hat_3scale_api_management/2.4/html/accounts/tokens)