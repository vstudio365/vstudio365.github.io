---
title: "How to Call SharePoint REST API from Postman using OAuth2.0 Authorization Flow"
layout: post
author: "venkadesh"
---

**Article-in-progress**

In this Article, You'll learn how to **Call SharePoint REST API** from Postman using **OAuth2.0 Authorization Flow**

### Prerequisites:

* SharePoint Site Collection in M365 Tenant
* Azure AD access to register app (Azure Subscription linked to the Azure AD used by your M365 tenant)
* Basic knowledge on [SharePoint REST API](https://docs.microsoft.com/en-us/sharepoint/dev/sp-add-ins/complete-basic-operations-using-sharepoint-rest-endpoints).
* Postman tool to send HTTP Request


### Implementation Steps:

1. Register an application in Azure AD
1. Build HTTP Request in Postman to Obtain OAuth2 access token and call
1. Send HTTP Request to SharePoint with OAuth2 access Token from Postman

#### Step 1: Register an application in Azure AD
1. Go to Azure AD (Azure Subscription linked to the Azure AD used by your Office 365 tenant)
1. Select App Registration > New Registration > Fill out the form

![screenshot](https://vstudio365.github.io/blog/assets/screenshots/app-registration-form-01.jpg)

1. Generate Secret
1. Set Permission
        API Permissions > Add a Permission > SharePoint > Application Permissions > Select Sites.FullControl.All (or as required) > Add Permissions

#### Step 2: Build HTTP Request in Postman to Obtain OAuth2 access token and call

1. Go to **Postman**
1. Create HTTP Request
1. Go to **Authorization** tab, select **Oauth 2.0** and fill values as below and click on **Get New Access Token**
1. After Authentication Complete, click proceed > Use Token

#### Step 3: Send HTTP Request to SharePoint with OAuth2 access Token from Postman

1. Click Send
1. Check reponse from SharePoint
