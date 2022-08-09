---
title: "How to get Microsoft Teams ID using Microsoft Graph API"
description: "How to get Microsoft Teams ID using Microsoft Graph API"
layout: post
author: "venkadesh Sundaramurthy"
category: MSTeams GraphAPI
comment_id: 2
---

In this Article, we'll walkthrough how to get **Microsoft Teams ID** using **Graph API** in Graph Explorer


## Steps:

1. Go to [Graph Explorer](https://developer.microsoft.com/en-us/graph/graph-explorer)
1. Sign in with your user credentials (which is used for sign in to Microsoft Teams)
1. Fill the details as mentioned below and click **Run query**.
> - HTTP Method : GET
> - Version : v1.0
> - URL : https://graph.microsoft.com/v1.0/groups?$filter=displayName eq '{teams-display-name}'&$select=id
>
>![screenshot](/assets/screenshots/article002/get-teams-id.png)

## Note:
- Incase of **Forbidden error** thrown by Graph API, find the missing permission from the error message. In this case, consider **Team.ReadBasic.All** (or other pemission as you prefer)
> ![screenshot](/assets/screenshots/article002/forbidden-error-text.png)

- Navigate to **Modify Permissions (Preview)** Tab > (consider Team.ReadBasic.All for this scenario) click **consent** 
> ![screenshot](/assets/screenshots/article002/permission-consent-part.png)

## Summary

- We've learnt to get Microsoft Teams ID using Graph API through Graph Explorer
- There are other ways to get ID. e.g. Microsoft Teams (via get Link), Powershell Script, Power Automate etc.