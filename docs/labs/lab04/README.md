# Lab 4

## API Security

### Securing APIs with OpenID Connect and Red Hat Single Sign On

* Duration: 30 mins
* Audience: API Owners, Product Managers, Developers, Architects

## Overview

Once you have APIs in your organization and have applications being written, you also want to be sure in many cases that thee various types of users of the APIs are correctly authenticated. In this lab you will discover how to set up the widely used OpenID connect pattern for Authentication. 

### Why Red Hat?

The Red Hat SSO product provides important functionality for managing identities at scale. In this lab you can see how it fits together with 3scale and OpenShift.

### Skipping The Lab

We know sometime we don't have enough time to go over step by step on the labs. So here is a [short video](https://vimeo.com/middlewarepro/3scale-security-oidc-demo) where you can see how to configure OpenID Connect for your service using Red Hat Single Sign On.

If you are planning to follow to the next lab, there is an already running OpenID Connect secured API proxy for the Location API Service in this endpoint:

```bash
https://location-service-sso.amp.apps.GUID.opentlc.com
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

### Step 1: Get Red Hat Single Sign On Service Account Credentials

1. Open a browser window and navigate to:

    ```bash
    http://sso-rh-sso.apps.GUID.opentlc.com/auth/admin/userX/console/
    ```

    *Remember to replace the GUID with your [environment](#environment) value and your user number.*

1. Log into Red Hat Single Sign On using your designated [user and password](#environment). Click on **Sign In**.

    ![00-login-sso.png](images/00-login-sso.png "RH SSO Login")

1. Select **Clients** from the left menu.

    ![00-clients.png](images/00-clients.png "Clients")

    *A 3scale-admin client and service account was already created for you*.

1. Click on the **3scale-admin** link to view the details.

    ![00-3scale-admin.png](images/00-3scale-admin.png "3scale admin account")

1. Click the **Credentials** tab.

    ![00-sa-credentials.png](images/00-sa-credentials.png "Service Account Credentials")

1. Take notice of the service account **Secret**. Copy and save it or write it down as you will use it to configure 3scale.

    ![00-sa-secret.png](images/00-sa-secret.png "Service Account Secret")

### Step 2: Add User to Realm

1. Click on the Users menu on the left side of the screen.

    ![00-users.png](images/00-users.png "Users Menu")

1. Click the Add user button.

    ![00-add-user.png](images/00-add-user.png "Add User")

1. Type **apiuser** as the Username.

    ![00-username.png](images/00-username.png "User Details")

1. Click on the **Save** button.

1. Click on the **Credentials** tab to reset the password. Type **apipassword** as the *New Password* and *Password Confirmation*. Turn OFF the *Temporary* to avoid the password reset at the next login.

    ![00-user-credentials.png](images/00-user-credentials.png "User Credentials")

1. Click on **Reset Password**.

1. Click on the **Change password** button in the pop-up dialog.

    ![00-change-password.png](images/00-change-password.png "Change Password Dialog")

    *Now you have a user to test your integration.*

### Step 3: Configure 3scale Integration

1. Open a browser window and navigate to:

    ```bash
    https://userX-admin.apps.bilbao-8ecb.open.redhat.com/
    ```

    *Remember to replace the GUID with your [environment](#environment) value and your user number.*

1. Accept the self-signed certificate if you haven't.

    ![selfsigned-cert](images/00-selfsigned-cert.png "Self-Signed Cert")

1. Log into 3scale using your designated [user and password](#environment). Click on **Sign In**.

    ![01-login.png](images/01-login.png)

1. The first page you will land is the *API Management Dashboard*. Click on the **API** menu link.

    ![01a-dashboard.png](images/01a-dashboard.png)

1. This is the *API Overview* page. Here you can take an overview of all your services. Click on the **Settings** menu link and the **Settings** menu link.

    ![02-api-integration.png](images/02-api-integration.png)

1. Scroll down the page, under the *Authentication* section, select **OpenID Connect Use OpenID Connect for any OAuth 2.0 flow**.

    ![03-authentication.png](images/03-authentication.png)


2. In the **OPENID CONNECT (OIDC) BASICS** section, change the  type in your previously noted client credentials with the URL of your Red Hat Single Sing On instance:

    ```bash
    http://3scale-admin:CLIENT_SECRET@sso-rh-sso.apps.GUID.opentlc.com/auth/realms/userX
    ```

    *Remember to replace the GUID with your [environment](#environment) value, your user number and the CLIENT_SECRET you get in the [Step 1](#step-1-get-red-hat-single-sign-on-service-account-credentials)*.

    ![04-openid-issuer.png](images/04-openid-issuer.png "OpenID Connect Issuer")

1. Scroll down the page and click on the **Update Product** button.

    ![05-update.png](images/05-update.png "Update Product")

1. Once it has been updated, you will notice that a warning appears under **Configuration** menu, click on the menu link.

    ![06-configuration.png](images/06-configuration.png "Configuration")

1.  Promote to Staging by clicking the **Promote to Stanging APIcast** button.

    ![07-promote-staging.png](images/07-promote-staging.png "Promote to Staging")

2.  Promote to Production by clicking the **Promote to Production** button.

    ![08a-promote-production.png](images/08a-promote-production.png "Update Environment")

### Step 4: Create a Test App

1. Go to **Audience** main menu link.
   ![09-audience.png](images/09-audience.png "Audience")

2. Click on **Developer**.

    ![09a-developer.png](images/09-developer.png "Developer")

3. Click on the **Applications** link.

    ![10-applications.png](images/10-applications.png "Applications")

4. Click on **Create Application** link.

    ![11-create-application.png](images/11-create-application.png "Create Application")

5. Select **Basic** plan from the combo box. Type the following information:

    * Name: **Secure App**
    * Description: **OpenID Connect Secured Application**

    ![12-application-details.png](images/12-application-details.png "Application Details")

6. Finally, scroll down the page and click on the **Create Application** button.

    ![13-create-app.png](images/13-create-app.png "Create App")

7. Note the *API Credentials*. Write them down as you will need the **Client ID** and the **Client Secret** to test your integration.

    ![14-app-credentials.png](images/14-app-credentials.png "App Credentials")

*Congratulations!* You have now an application to test your OpenId Connect integration.

## Steps Beyond

So, you want more? Login to the Red Hat Single Sign On admin console for your realm if you are not there already. Click on the Clients menu. Now you can check that 3scale zync component creates a new Client in SSO. This new Client has the same ID as the Client ID and Secret from the 3scale admin portal.

### Test the integration

You can try to use Postman or OpenID Connet playground to test your integration. Remember to update the *Redirect URL*.

1. Download the postman client if you already don't have it:

   [Postman](https://www.postman.com/downloads/)

2. Create a new Request in postman click ****:
   ![15-create-request.png](images/15-create-request.png "Create Request")
3. Add the following properties:
   * Request Name: **Get Locations**
   * Collection Name: **Locations Collection**

   ![16-add-properties.png](images/16-add-properties.png "Add properties")

4. Click **Save to locations** button.

5. Select **Authorization** tab, then click **Type** and select **Oauth 2.0**

    ![17-select-auth.png](images/17-select-auth.png "Select Auth")
6. Configure a new Token:

   * Token Name: **RHSSO**
   * Gran Type: **Authorization Code**
   * Callback URL: **https://openidconnect.net/callback**
   * Auth URL: **http://sso-rh-sso.apps.GUID.opentlc.com/auth/realms/userX/protocol/openid-connect/auth**
   * Access Token URL: **http://sso-rh-sso.apps.GUID.opentlc.com/auth/realms/userX/protocol/openid-connect/token**
   * Client ID: **<AplicationClient>**
   * Client Secret: **<ApplicationToken>**
   * Client Authentication: Send as Basic Auth header

    ![18-get-access-token.png](images/18-get-access-token.png "New access token")

7. Click in **Get New Access token**
8. Add username and password in the rhsso browser:

   ![19-rhsso-browser.png](images/19-rhsso-browser.png "RHSSO Browser")

9. Click **Log in** button.

10. If successful, you will see a page with the generated token. Click **Use Token** button to add it to the request.

    ![20-use-token.png](images/20-use-token.png "RHSSO Browser")

11. Add the url to test the service

    * URL : http://location-service-userX.apps.GUID.opentlc.com/locations

12. Click **Send** Blue button:

    ![21-send.png](images/21-send.png "Send")

## Summary

Now that you can secure your API using three-leg authentication with Red Hat Single Sign-On, you can leverage the current assets of your organization like current LDAP identities or even federate the authentication using other IdP services.

For more information about Single Sign-On, you can check its [page](https://access.redhat.com/products/red-hat-single-sign-on).

You can now proceed to [Lab 5](../lab05/#lab-5)

## Notes and Further Reading

* [Red Hat 3scale API Management](http://microcks.github.io/)
* [Red Hat Single Sign On](https://access.redhat.com/products/red-hat-single-sign-on)
* [Setup OIDC with 3scale](https://developers.redhat.com/blog/2017/11/21/setup-3scale-openid-connect-oidc-integration-rh-sso/)
