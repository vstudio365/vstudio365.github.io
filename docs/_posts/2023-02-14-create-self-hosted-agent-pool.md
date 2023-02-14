---
title: "How to Create self hosted agent pool in Azure Devops"
description: "How to Create self hosted agent pool in Azure Devops"
layout: post
author: "venkadesh Sundaramurthy"
category: AzureDevops
---

In this Article, we'll walkthrough how to **Create Self hosted agent pool** in Azure Devops


### Prerequisites:
* Azure Devops User Account
* Devops Project created under your organization

### Implementation Steps:
1. Generate Personal Access Token (PAT)
2. Create Agent pool
3. Add Agent
4. Check Agent Status

#### Step 1: Generate Personal Access Token (PAT)
1. Sign in to your Azure DevOps organization (https://dev.azure.com/{your_organization}).
1. From your home page, open your user settings, and then select Personal access tokens.
2. ![screenshot](/assets/screenshots/article004/pool-type.png)
#### Step 2: Create agent pool
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
1. Navigate to **Agents** tab > click **New Agent**
1. Follow the Instructions outlined below
> - Click Download button, to downlad the agent
> - Create a directory using below command
> - D:\> md your-desired-directory-name
> - D:\> cd your-desired-directory-name
> - D:\your-desired-directory-nam> Add-Type -AssemblyName System.IO.Compression.FileSystem ; [System.IO.Compression.ZipFile]::ExtractToDirectory("<download-directory>\vsts-agent-win-x64-2.217.2.zip", "$PWD")
> - Configure the Agent usinf below command
> - PS C:\agent> .\config.cmd
> - PS C:\agent> .\run.cmd

#### Step 3: Check Agent Status

