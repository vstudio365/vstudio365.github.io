
---
title: "How to Create self hosted agent pool in Azure Devops"
description: "How to Create self hosted agent pool in Azure Devops"
layout: post
author: "venkadesh Sundaramurthy"
category: AzureDevops
---
In this Article, we'll walkthrough how to **Create Self hosted agent pool** in Azure Devops

### Prerequisites:
* SharePoint Site Collection in [M365 Tenant](https://www.office.com/)
* Azure AD access to register app (Azure Subscription linked to the Azure AD used by your M365 tenant)
* Basic knowledge on [SharePoint REST API](https://docs.microsoft.com/en-us/sharepoint/dev/sp-add-ins/complete-basic-operations-using-sharepoint-rest-endpoints) and [OAuth2 Authorization code flow](https://auth0.com/docs/get-started/authentication-and-authorization-flow/authorization-code-flow).
* [Postman](https://www.postman.com/product/tools/) tool to send HTTP Request

### Implementation Steps:
1. Create Agent pool
1. Add Agent
1. Send HTTP Request to SharePoint with OAuth2 access Token from Postman
#### Step 1: Create agent pool
1. Go to Desired Project
1. Select **project settings** gear button located in the left-bottom
1. Click **Agent Pools** > **Add pool**
1. Select **Self-hosted** Pool type
1. Fill Agent Pool **Name**
1. Enable **Grant access permission to all pipelines**
1. Click **Create**
1. >![screenshot](/assets/screenshots/article004/pool-type.png)
#### Step 2: Add agent
1. Select newly created Agent Pool
2. Navigate to **Agents** tab > click **New Agent**
3. Follow the Instructions as described
> - Click Download button
> - Create the agent using following command
> - PS C:\> mkdir agent ; cd agent
> - PS C:\agent> Add-Type -AssemblyName System.IO.Compression.FileSystem ; [System.IO.Compression.ZipFile]::ExtractToDirectory("<directory-path>\vsts-agent-win-x64-2.217.2.zip", "$PWD")
> - Configure the Agent usinf below command
> - PS C:\agent> .\config.cmd
> - PS C:\agent> .\run.cmd
## Summary

