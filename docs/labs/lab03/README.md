# Lab 3

## API Management

### Take control of your APIs

* Duration: 15 mins
* Audience: API Owners, Product Managers, Developers, Architects

## Overview

Once you have APIs deployed in your environment, it becomes critically important to manage who may use them and for what purpose. You also need to begin to track usage of these different users to know who is/is not succeeding in their usage. For this reason in this lab you will be adding management capabilites to the API to give you control and visibility of it's usage.

### Why Red Hat?

Red Hat provides one the leading API Management tools which provide management services. The 3scale API Management solution enables you to quickly and easy protect and manage your APIs.

### Skipping The Lab

If you are planning to follow to the next lab, there is an already running API proxy for the Location API Service in this endpoint:

```bash
https://location-service-api.amp.apps.GUID.opentlc.com
```

### Environment

**URLs:**

Check with your instruction the *GUID* number of your current workshop environment. Replace the actual number on all the URLs where you find **GUID**.

Example in case of *GUID* = **cluster-lhm8v.lhm8v.sandbox430**:

```bash
https://console-openshift-console.apps.GUID.opentlc.com
```

becomes =>

```bash
https://console-openshift-console.apps.cluster-lhm8v.lhm8v.sandbox430.opentlc.com
```

**Credentials:**

Your username is your asigned user number. For example, if you are assigned user number **1**, your username is: 

```bash
user1
```

The password to login is always the same:

```bash
openshift
```

## Lab Instructions

### Step 1: Define your API Proxy

Your 3scale Admin Portal provides access to a number of configuration features.

1. Open a browser window and navigate to:

    ```bash
    https://userX-admin.apps.GUID.opentlc.com/
    ```

    *Remember to replace the GUID with your [environment](#environment) value and your user number.*

1. Accept the self-signed certificate if you haven't.

    ![selfsigned-cert](images/00-selfsigned-cert.png "Self-Signed Cert")

1. Log into 3scale using your designated [user and password](#environment). Click on **Sign In**.

    ![01-login.png](images/01-login.png)

1. The first page you will land is the *API Management Dashboard*. Click on the **Dashboard** menu link and then click the **API** menu link.

    ![01a-dashboard.png](images/01a-dashboard.png)

1. This is the *API Overview* page. Here you can take an overview of all your products. Click on the **Integration** left menu link and then **Settings** link.

    ![03-edit-settings.png](images/03-edit-settings.png)

2. Keep select the **APIcast 3scale managed** deployment option in the *Deployment* section, under the Authentication section keep the **API Key (user_key) The application is identified & authenticated via a single string**.

    ![04-apicast.png](images/04-apicast-user-key.png)

3. Scroll down and click on **Update Product** button.

    ![05-authentication.png](images/05-update-product.png)

4. Click on **Mapping Rules** section under **Integration** menu to define the allowed methods on our exposed API.

    *The default mapping is the root ("/") of our API resources, something that we might want to avoid*.

    ![07b-mapping-rules.png](images/07b-mapping-rules.png)

5. Click Trash icon for the verb **GET** and pattern **/**, if a popup appears, confirm.
   
    ![07b-delete-mapping-rule.png](images/07b-delete-mapping-rule.png)

6. Click on **Method & Metrics** section under **Integration** menu.

    ![07b-mapping-rules-define.png](images/07b-mapping-rules-define.png)

7. Click on the **New Method** link in the *Methods* section.

    ![07b-new-method.png](images/07b-new-method.png)

8.  Fill in the information for your Fuse Method.

    * Friendly name: **Get Locations**

    * System name: **locations_all**

    * Description: **Method to return all locations**

    ![07b-new-method-data.png](images/07b-new-method-data.png)

9.  Click on **Create Method**.

10. Click on the **Add mapping rule** link

    ![07b-add-mapping-rule.png](images/07b-add-mapping-rule.png)

11. Set the following properties: 

    * Verb: **GET**
    * Pattern: **/locations**
    * Metric or Method to increment: **Get Locations**
    * Increment by: **1**
    * Friendly name: **Get Locations**
    * Position: **1**

    ![07b-create-mapping-rule.png](images/07b-create-mapping-rule.png)

12. Click on **Create Mapping Rule**
    ![07b-mapping-end.png](images/07b-mapping-end.png)

### Step 2: Define your API Policies

Red Hat 3scale API Management provides units of functionality that modify the behavior of the API Gateway without the need to implement code. These management components are know in 3scale as policies.

The order in which the policies are executed, known as the “policy chain”, can be configured to introduce differing behavior based on the position of the policy in the chain. Adding custom headers, perform URL rewriting, enable CORS, and configurable caching are some of the most common API gateway capabilities implemented as policies.

1. Scroll down and expand the **POLICIES** section to define the allowed methods on our exposed API.

    ![01-policies](images/policies-01.png "Policies")

    *The default policy in the Policy Chain is APIcast. This is the main policy and must of the times you want to keep it*.

1. Click the **Add Policy** link to add a new policy to the chain.

    ![02-add-policy](images/policies-02.png)

    _Out-of-the-box 3scale includes a set of policies you can use to modify the way your API gateway behaves. For this lab, we will focus on the **Cross Origin Resource Sharing (CORS)** one as we will use it in the consumption lab_.

1. Click in the **CORS Request Handling** link to add the policy.

    ![03-cors-policy](images/policies-03.png "CORS")

1. Put your mouse over the right side of the policy name to enable the reorder of the chain. Drag and drop the CORS policy to the top of the chain.

    ![04-chain-order](images/policies-04.png "Chain Order")

1. Now **CORS** policy will be executed before the **APIcast**. Click the **CORS Request Handling** link to edit the policy.

    ![05-cors-configuration](images/policies-05.png "Cors Configuration")

1. In the *Edit Policy* section, click the green **+** button to add the allowed headers.

    ![06-add-headers](images/policies-06.png "Add Allow Headers")

1. Type **Authorization** in the *Allowed headers* field. 

    ![07-authorization-header](images/policies-07.png "Add Authorization Header")

1. Tick the **allow_credentials** checkbox and fill in with a star (**\***) the *allow_origin* text box.

    ![08-allow-origin](images/policies-08.png "Allow Origin")

1. Click twice the green **+** button under *ALLOW_METHODS* to enable two combo boxes for the CORS allowed methods.

1. Select **GET** from the first box and **OPTIONS** from the second box.

    ![09-allow-methods](images/policies-09.png "Allow Methods")

1. Click the **Update Policy** button to save the policy configuration.

### Step 3: Setup the API Backend

1. Click in the main menu, and then click in API Backend section.
   ![3-api-backend](images/3-api-backend.png "Api Backend")
2. Click **edit** button in the **Backend Overview**
   ![3-edit-backend](images/3-edit-backend.png "Edit Backend")
3. Under the **API** section edit the **Private Base Url**:
   
    * Private Base URL: **http://location-service.userX.svc:8080**

    ![3-update-backend](images/3-update-backend.png "Update Backend")

### Step 3: Configure the Upstream Endpoint

1. Now it's time to go back again to the API section
   
   ![4-api-section](images/4-api-section.png "Api section")

2. Click the **settings** section, under **Integration**:
   
   ![4-integration-settings](images/4-integration-settings.png "Integration Settings")

3. Fill in the information for accessing your API:

    * Staging Public Base URL: **https://location-userX-api-staging.amp.apps.GUID.opentlc.com:443**

    * Production Public Base URL: **https://location-userX-api.amp.apps.GUID.opentlc.com:443**

    *Remember to replace the GUID with your [environment](#environment) value*.

    *We are using the internal API service, as we are deploying our services inside the same OpenShift cluster*.

    ![07-baseurl-configuration.png](images/07-baseurl-configuration.png)

4. Scroll down and click **Update product**
   ![07a-save.png](images/07a-save.png)

5. Go to **Configuration**
    ![08a-configuration.png](images/08a-configuration.png)

6. Click on the **Promote v. 1 to Staging APICast** to promote the current configuration to Staging.

    ![08b-update-staging.png](images/08-update-staging.png)

    *You can test if it's fine, by picking up the content inside **Example curl for testing** and testing it in your terminal. (-k option maybe it's needed due to self signed certificates)*.

    Example:
    ```bash
        curl "https://location-userX-api-staging.amp.apps.GUID.opentlc.com:443/locations?user_key=APIKEY" -k
    ```

7. Click on the **Promote v.1 to Production** button to promote your configuration from staging to production.

    ![08a-promote-production.png](images/08a-promote-production.png)

*Congratulations!* You have configured 3scale access control layer as a proxy to only allow authenticated calls to your backend API. 3scale is also now:

* Authenticating (If you test with an incorrect API key it will fail) 
* Recording calls (Visit the Analytics tab to check who is calling your API).

## Steps Beyond

In this lab we just covered the basic creating of a proxy for our API service. Red Hat 3scale API Management also allows us to get a track of the security (as you can see in the next lab) as well as the usage of our API. If getting value from APIs is also important to you, 3scale allows you to monetize your APIs with it's embedded billing system.

Try to navigate through the rest of the tabs of your Administration Portal. Did you notice that there are application plans associated to your API? Application Plans allow you to take actions based on the usage of your API, like doing rate limiting or charging by hit or monthly usage.

## Summary

You set up an API management service and API proxies to control traffic into your API. From now on you will be able to issue keys and rights to users wishing to access the API.

You can now proceed to [Lab 4](../lab04/#lab-4)

## Notes and Further Reading

* [Red Hat 3scale API Management](http://microcks.github.io/)
* [Developers All-in-one 3scale install](https://developers.redhat.com/blog/2017/05/22/how-to-setup-a-3scale-amp-on-premise-all-in-one-install/)
* [ThoughtWorks Technology Radar - Overambitious API gateways](https://www.thoughtworks.com/radar/platforms/overambitious-api-gateways)
