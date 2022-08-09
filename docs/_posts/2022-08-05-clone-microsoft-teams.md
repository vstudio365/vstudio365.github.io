---
title: "Power Automate: How to create Microsoft Teams from Template Teams using Microsoft Graph API"
description: "Power Automate: How to create Microsoft Teams from Template using Microsoft Graph API"
layout: post
author: "venkadesh Sundaramurthy"
category: MSTeams GraphAPI PowerAutomate
---

In this Article, we'll walkthrough how to **Create Microsoft Teams** from Template Teams using **Microsoft Graph API** in Power Automate


### Pre-requisites:
- Teams created in Microsoft Teams which acts a Template
- Access to Azure AD to register App
- Access to Power Automate maker portal

### Implementation Steps:

1. Register an application in Azure AD and add Teams/Group Permissions
1. Build flow in Power Automate that do the following,
    - Get Template Teams based on Template Teams Name
    - Send Graph API Request to Create Teams by Cloning the Template Teams
    - Check Teams Provisioning Status and ensure Teams created

#### Step 1: Register an application in Azure AD and add Teams/Group Permission
1. Go to [Azure AD](https://aad.portal.azure.com/)
1. Select **App Registration** > New Registration > Fill out the form
>![screenshot](/assets/screenshots/article003/app-registration.png)
1. Copy **Client ID** and **Tenant ID** for later use
>![screenshot](/assets/screenshots/article003/grab-app-details.png)
1. Generate Secret: **Certificates & Secrets** > New Client Secret > Fill Description > Add
>![screenshot](/assets/screenshots/article003/generate-secret.png)
1. Copy Secret (It won't be visible later. So, note down for later use)
>![screenshot](/assets/screenshots/article003/copy-app-secret.png)
1. Select target API to add Permission: **API Permissions** > Add a Permission > Microsoft Graph
>![screenshot](/assets/screenshots/article003/select-api-type.png)
1. Add Permission: **Application Permissions** > (scroll down to Groups)Select Group.ReadWrite.All > Add Permissions
>![screenshot](/assets/screenshots/article003/select-permission.png)
1. Grant Consent: Click 'Grant admin consent for \<tenant ID>' (Status should turn to green)
>![screenshot](/assets/screenshots/article003/grant-consent.png)

#### Step 2: Build flow in Power Automate
1. Go to Power Automate maker portal : Go to [Microsoft365 portal](www.office.com) > Select Power Automate
>![screenshot](/assets/screenshots/article003/select-power-automate.png)
1. Click **New flow** > Instant cloud flow
>![screenshot](/assets/screenshots/article003/create-flow.png)
>
>
>**Note** : In automation scenarios, we need to use 'Automated cloud flow' with relevant trigger
1. Enter Flow name, Select **Manually trigger a flow** > Click Create
>![screenshot](/assets/screenshots/article003/select-flow-trigger.png)
1. Create Trigger Input Parameters: Add Input
>![screenshot](/assets/screenshots/article003/flow-add-input.png)
1. Add 2 input properties: Select Text(Input Type) > fill name and leave blank value
> 1. Teams Template Name
> 1. New Teams Name
>![screenshot](/assets/screenshots/article003/flow-add-trigger-inputs.png)
1. Click New Step > Initialize variable (select from list of actions) > fill details. (Repeat this step and add variables as described in screenshot)
>![screenshot](/assets/screenshots/article003/initialize-graph-client-variables.png)
1. Click New Step > select **Http** > fill details and rename it as 'GET Template Teams ID'
```javascript
        "method": "GET",
        "uri": "https://graph.microsoft.com/v1.0/groups?$filter=displayName eq '@{triggerBody()['text']}'&$select=id",
        "authentication": {
            "type": "ActiveDirectoryOAuth",
            "authority": "@variables('varAuthorityUrl')",
            "tenant": "@variables('varTenantId')",
            "audience": "@variables('varGraphApiAudience')",
            "clientId": "@variables('varClientID')",
            "secret": "@variables('varClientSecret')"
        }
```
>![screenshot](/assets/screenshots/article003/graph-request-to-get-template-teams.png)
1. Run and check : Click **Test** (option exist at top right corner) > Manually > Test > Fill Details > Click **Run flow** > Done
> - Teams Template Name : Explore-Graph-API (Name of your Template Teams. This must have created as part of pre-requisites )
> - New Teams Name : Project-001 (or as you wish)
>![screenshot](/assets/screenshots/article003/test-run-get-template-teams.png)
1. Succeeded Test run looks as below
> ![screenshot](/assets/screenshots/article003/test-run-1-outcome.png)
1. Click **GET Template Teams ID** and Copy response body (which is required for next step)
>![screenshot](/assets/screenshots/article003/template-teams-response.png)
1. Continue to edit the flow : Click **Edit** (option exist at top right corner)
1. Click New Step > **parse JSON** (select from list of actions) > Fill details and Rename it as 'Parse JSON response of GET Template Teams ID'
>![screenshot](/assets/screenshots/article003/select-body-of-template-response.png)
> - Content - Body of 'GET Template Teams ID'
> - Schema - Click 'Generate from Sample' > Paste the value which copied from previous step > Done
>
1. Click New Step > **Compose** (select from list of actions) > Fill Input field by pasting below formula in Expression textbox > Rename it as 'Compose Template Teams ID'
```javascript
body('Parse_JSON_response_of_GET_Template_Teams_ID')?['value'][0]['id']
```
>![screenshot](/assets/screenshots/article003/compose-Template-Teams-Id.png)
1. Clone Teams: Click New Step > **Http** (select from list of action) > fill details and Rename it as 'Create Teams'
```javascript
"method": "POST",
"uri": "https://graph.microsoft.com/v1.0/teams/@{outputs('Compose_Template_Teams_ID')}/clone",
"body": {
           "displayName": "@triggerBody()['text_1']",
            "description": "created by cloning @{triggerBody()['text']}",
            "mailNickname": "@triggerBody()['text_1']",
            "partsToClone": "apps,tabs,settings,channels,members"
        },
        "authentication": {
            "type": "ActiveDirectoryOAuth",
            "authority": "@variables('varAuthorityUrl')",
            "tenant": "@variables('varTenantId')",
            "audience": "@variables('varGraphApiAudience')",
            "clientId": "@variables('varClientID')",
            "secret": "@variables('varClientSecret')"
        }
```
>![screenshot](/assets/screenshots/article003/create-teams.png)
>
>NOTE: Clone request returns **Response code 202**, means request accepted. Provisioning might take more time depends on size of Template Teams. So, write more logic to check the provisioning status
1. Click New Step > **Compose** (select from list of actions) > Fill Input field by pasting below formula in Expression textbox > Rename it as 'Compose URL to get Teams provision status'
```javascript
concat('https://graph.microsoft.com/v1.0',outputs('Create_Teams')['headers']?['Location'])
```
1. Click New Step > Initialize Variable (select from list of actions) > Fill details as mentioned below and Rename it as 'Initialize Teams Provisioning Status'
> - Name : varTeamsProvisioningStatus
> - Type : String
> - Value : inProgress
1. Click New Step > Do until (select from list of actions) > Fill details and Rename it as 'Do until Teams Provisioning Status to Succeeded'
> Condition :- 'varTeamsProvisioningStatus' is equal to 'succeeded'
1. Add action inside Do until block: Click Add an Action > select **Http** > fill details and Rename it as 'Get Teams provisioning status'
```javascript
        "method": "GET",
        "uri": "@{outputs('Compose_URL_to_get_Teams_provision_status')}",
        "authentication": {
            "type": "ActiveDirectoryOAuth",
            "authority": "@variables('varAuthorityUrl')",
            "tenant": "@variables('varTenantId')",
            "audience": "@variables('varGraphApiAudience')",
            "clientId": "@variables('varClientID')",
            "secret": "@variables('varClientSecret')"
        }
```
>![screenshot](/assets/screenshots/article003/get-teams-provisioning-status.png)
1.  Add action inside Do until block:Click Add an Action > select 'Set Variable' and Rename it as 'Set Teams Provisioning status' > Fill details as below and Click OK
```javascript
Name: varTeamsProvisioningStatus
Value: body('Get_Teams_provisioning_status')?['status']
```
>![screenshot](/assets/screenshots/article003/set-teams-provisioning-status.png)
1. Add action inside Do until block: Click Add an Action > select 'Condition' and Rename it as 'Check Teams Provisioning Status' 
> Condition : 'varTeamsProvisioningStatus' is equal to 'inProgress'
>   - If Yes : Delay for 1 Minute
>   - If No : no action
>![screenshot](/assets/screenshots/article003/delay-if-inprogress.png)
1. After adding the actions inside Do until block, it looks like this
>![screenshot](/assets/screenshots/article003/check-provisioning-status-block.png)
1. Run and check : Click **Test** (option exist at top right corner) > Manually > Save & Test > Fill Details > Click **Run flow** > Done
> - Teams Template Name : Explore-Graph-API
> - New Teams Name : Project-001
>
>![screenshot](/assets/screenshots/article003/flow-test-run.png)
1. Check your Teams
>![screenshot](/assets/screenshots/article003/outcome.png)

### Things to consider:
It is allowed to create more Teams with same name. During the automation, check the exitance of teams with the display name before creating new Teams

Graph endpoint to check: https://graph.microsoft.com/v1.0/groups?$filter=displayName eq 'new-teams-name'

