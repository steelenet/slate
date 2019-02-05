---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python

toc_footers:
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

The Amelia API can be used to poll Amelia for information, update BPNs, update users, carry on a conversation, and more.

# Authentication

> To authorize, use this code:

```python
import Amelia

Amelia.login(url, username, password)
```

```shell
# With shell, you can just pass the correct header with each request
curl -vk -X POST "https://amelia_url_here/AmeliaRest/api/v1/auth/login"
  -H "Content-Type:application/json"
  -d '{"ameliaUrl":amelia_url_here/Amelia,"username":username,"password":password}'
```

> The above command returns JSON structured like this:

```json
{
    "status": 201,
    "token": "6531bec3-6e80-4ab9-b0bc-d99aeda258a4",
    "userModel": {
        "agent": true,
        "anonymous": false,
        "availability": "READY",
        "email": "user@ipsoft.com",
        "name": "User Name",
        "userId": "154b9b97-8519-4404-8e9f-069a69d470e0"
    }
}
```

The Amelia API uses authentication tokens. API tokens are temporary and last as long as that user's session is active. When making a login call, an authentication token is returned.

Once a token has been issued, Amelia for the API key to be included in all API requests to the server in a header that looks like the following:

`X-Amelia-Rest-Token: token`

<aside class="notice">
You must replace <code>token</code> with your API key.
</aside>

# Conversation

## Start a new conversation

```python
import Amelia

Amelia.createNewConversation(url, token, domain, deliveryMode, debugMode, webhookUrl, secret)
#REQUIRED PARAMETERS
#url = The Amelia instance you want to connect to
#token = The API token issued to you
#domain = The Anmelia domain where you want to start a conversation, use the domain code
#
#OPTIONAL PARAMETERS
#deliveryMode = POLLING or WEBHOOK. Defaults to POLLING. Will deliver messages to a specific URL.
#webhookUrl = Required if WEBHOOK deliveryMode is chosen. URL to deliver messages to.
#secret = If specified, this exact text will be included in all posts to the specified webhookUrl as a header X-Amelia-Webhook-Secret, which you may use to verify that the POST in question is coming from Amelia, and even from a specific conversation.
```

```shell
curl -vk -X POST  "https://amelia_url_here/AmeliaRest/api/v1/conversations/new"
-H "X-Amelia-Rest-Token:token"
-H "Content-Type:application/json"
-d '{"domain":domain,"deliveryMode":"POLLING","receiveDebugMessages":"false"}'
```

> The above command returns JSON structured like this:

```json
{
	"sessionId": "a76a5e20-0ea3-4ceb-9c7b-e8efc8a6f4e2",
	"conversationId": "LIXMGGIFAAIAA-1",
	"sessionMode": "USER"
}
```

This endpoint starts a new conversation with Amelia.

