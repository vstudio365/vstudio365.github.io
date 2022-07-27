---
title: "How to Call SharePoint REST API from Postman using OAuth2 Authorization Flow"
layout: post
author: "venkadesh"
categories: sharepoint
---

In this Article, we'll walkthrough how to **Call SharePoint REST API** from Postman using **OAuth2 Authorization Flow**

### Prerequisites:

* SharePoint Site Collection in [M365 Tenant](https://www.office.com/)
* Azure AD access to register app (Azure Subscription linked to the Azure AD used by your M365 tenant)
* Basic knowledge on [SharePoint REST API](https://docs.microsoft.com/en-us/sharepoint/dev/sp-add-ins/complete-basic-operations-using-sharepoint-rest-endpoints) and [OAuth2 Authorization code flow](https://auth0.com/docs/get-started/authentication-and-authorization-flow/authorization-code-flow).
* [Postman](https://www.postman.com/product/tools/) tool to send HTTP Request


### Implementation Steps:

1. Register an application in Azure AD and add SharePoint Permissions
1. Build HTTP Request in Postman to Obtain OAuth2 access token
1. Send HTTP Request to SharePoint with OAuth2 access Token from Postman

#### Step 1: Register an application in Azure AD and add SharePoint Permissions
1. Go to **Azure AD** (Directory linked to your M365 tenant)
1. Select **App Registration** > New Registration > Fill out the form
>![screenshot](/assets/screenshots/article001/article001-app-registration.png)
>
> Note: Redirect URL [https://oauth.pstmn.io/v1/callback](https://oauth.pstmn.io/v1/callback).
1. Copy **Client ID** and **Tenant ID** for later use
>![screenshot](/assets/screenshots/article001/article001-copy-app-details.png)
1. Generate Secret: **Certificates & Secrets** > New Client Secret > Fill Description > Add
>![screenshot](/assets/screenshots/article001/article001-generate-secret.png)
1. Copy Secret (It won't be visible later. So, note down for later use)
>![screenshot](/assets/screenshots/article001/article001-copy-secret.png)
1. Select target API to add Permission: **API Permissions** > Add a Permission > SharePoint
> ![screenshot](/assets/screenshots/article001/article001-select-api.jpg)
1. Add Permission: **Application Permissions** > Select Sites.Read.All (or as required) > Add Permissions
> ![screenshot](/assets/screenshots/article001/article001-add-permission.png)
1. Grant Consent: Click 'Grant admin consent for \<tenant ID>' (Status should turn to green)
> ![screenshot](/assets/screenshots/article001/article001-grant-consent.png)

#### Step 2: Build HTTP Request in Postman to Obtain OAuth2 access token

1. Go to **Postman**
1. Create HTTP Request : Click New > HTTP Request
>![screenshot](/assets/screenshots/article001/article001-create-http-request.png)
1. Build HTTP Request : Fill Verb, Url and headers
>![screenshot](/assets/screenshots/article001/article001-request-headers.png)
> - HTTP Method : GET
> - HTTP Request : \<site-url>/_api/web/lists
> - Headers :-   Accept : application/json;odata=verbose
1. Go to **Authorization** tab, select **Oauth 2.0** in type field
1. Navigate to **Configure New Token** and fill values as below and click on **Get New Access Token**
>![screenshot](/assets/screenshots/article001/article001-authorization-data.png)
> - Token Name: \<friendly name>
> - Grant Type: Authorization Code
> - Callback URL: https://oauth.pstmn.io/v1/callback
> - Auth URL: https://login.microsoftonline.com/common/oauth2/authorize?resource=https%3A%2F%2F\<tenant_name>.sharepoint.com  (refer tenant name from SharePoint Site URL)
> - Access Token URL: https://login.microsoftonline.com/common/oauth2/token  
> - Client ID: \<client ID>  (copied from Step #1 point #3)
> - Client Secret: \<KEY>  (copied from Step #1 point #5)
> - Scope: \<Leave empty>
> - State : \<Leave empty>
>
> note: user will be prompted to fill username & password
4. After Authentication Complete, click proceed > Use Token
>![screenshot](/assets/screenshots/article001/article001-refer-access-token.png)

#### Step 3: Send HTTP Request to SharePoint with OAuth2 access Token from Postman

1. Click Send
>![screenshot](/assets/screenshots/article001/article001-send-request.png)
1. Check reponse from SharePoint
>![screenshot](/assets/screenshots/article001/article001-http-response.png)
