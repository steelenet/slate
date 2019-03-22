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

## Login

> <font size="4">POST /AmeliaRest/api/v1/auth/login</font>

> Example Request:

```python
import amelia

amelia.authentication().login(url, username, password)
```

```shell
# With shell, you can just pass the correct header with each request
curl -vk -X POST "https://{ameliaUrl}/AmeliaRest/api/v1/auth/login"
  -H "Content-Type:application/json"
  -d '{"ameliaUrl"}:{ameliaUrl}/Amelia,"username":username,"password":password'
```

> Example Response:

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

## Logout

> <font size="4">POST /AmeliaRest/api/v1/auth/logout</font>

> Example Request:

```python
import amelia

amelia.authentication().logout(url, token)
```

```shell
# With shell, you can just pass the correct header with each request
curl -vk -X POST "https://{ameliaUrl}/AmeliaRest/api/v1/auth/logout"
 -H "X-Amelia-Rest-Token: token"
```

> Example Response:

```json
{
	"status": 201,
	"token": "34272e4c-d269-406f-a4cf-5580788ab65e"
}
```

Ends the current token session.

# Authentication Policies

## Get authentication policies

> <font size="4">GET /AmeliaRest/api/v1/admin/system/authenticationPolicies/</font>

> Example Request:

```python
import amelia
```

```shell
curl -vk "https://{ameliaUrl}/AmeliaRest/api/v1/admin/system/authenticationPolicies/"
-H "X-Amelia-Rest-Token: token"
```

> Example Response:

```json
{
	"first": true,
	"last": true,
	"totalElements": 4,
	"totalPages": 1,
	"content": [{
		"id": "0d3c73d3-3410-4ef0-9c85-154304ld0293",
		"name": "IPsoft ADFS",
		"authenticationSystemName": "ipsoft_adfs",
		"passwordChangedAllowed": false
	}, {
		"id": "1a29f2aa-97e8-4ec2-a887-2f73c5ld932",
		"name": "Default User Authentication Policy",
		"authenticationSystemName": "Internal",
		"passwordChangedAllowed": true
	}, {
		"id": "ca58a003-00a2-4831-8e31-45b6316asdf2",
		"name": "IPsoft LDAP Authentication Policy",
		"authenticationSystemName": "IPsoft LDAP",
		"passwordChangedAllowed": false
	}, {
		"id": "e83002be-ab6e-4505-93d3-debcd29qw231",
		"name": "System User Authentication Policy",
		"authenticationSystemName": "Deny All",
		"passwordChangedAllowed": false
	}]
}
```

Returns a list of authentication policies that a given Amelia instance has available.

### Optional URL parameters

| Parameter     | Description   | Type  |
| :------------- |:-------------| :-----|
| name          | Name of the policy to search for | string |
| page          | Page of the results to return      |   int |
| size          | Number of results to return      |    int |
| sort          | A list of {field}, {dir} directives. Dir can either be asc or desc      |    array[string] |

# Conversation

## Start a new conversation

> <font size="4">POST /AmeliaRest/api/v1/conversations/new</font>

> Example Request:

```python
import amelia

amelia.conversation().new(url, token, domain, deliveryMode, debugMode, webhookUrl, secret)
```

```shell
curl -vk -X POST  "https://{ameliaUrl}/AmeliaRest/api/v1/conversations/new"
-H "X-Amelia-Rest-Token:token"
-H "Content-Type:application/json"
-d '{"domain":domain,"deliveryMode":"POLLING","receiveDebugMessages":"false"}'
```

> Example Response:

```json
{
	"sessionId": "a76a5e20-0ea3-4ceb-9c7b-e8efc8a6f4e2",
	"conversationId": "LIXMGGIFAAIAA-1",
	"sessionMode": "USER"
}
```

This endpoint starts a new conversation with Amelia.

### Required URL parameters

| Parameter     | Description   | Type  |
| :------------- |:-------------| :-----|
| url          | The Amelia instance you want to connect to | string |
| token          | The API token issued to you      |   string |
| domain          | The Amelia domain code where you want to start a conversation      |    string |

### Optional URL parameters

| Parameter     | Description   | Type  |
| :------------- |:-------------| :-----|
| deliveryMode          | POLLING or WEBHOOK. Default to POLLING. Will deliver messages to a specific URL | string |
| webhookUrl          | Required if WEBHOOK deliveryMode is chosen. URL to deliver message to     |   string |
| secret          | If specified, this exact text will be included in all posts to the specified webhookUrl as a header X-Amelia-Webbook-Secret, which can be used to verify that the POST in question is coming from Amelia, and even from a specific conversation.      |   string |

## Send a message to Amelia

> <font size="4">POST /AmeliaRest/api/v1/conversations/{sessionId}/send</font>

> Example Request:

```python
import amelia

amelia.conversation().sendMessage(url, token, sessionId, message)
```

```shell
curl -kv --header 'Content-Type: application/json'
-X POST "https://{ameliaUrl}/AmeliaRest/api/v1/conversations/sessionId/send"
-H 'X-Amelia-Rest-Token: token'
-d '{"messageText":"some message to Amelia here"}'
```

This endpoint sends a message to Amelia for a given conversation. The sessionId can be obtained when starting a conversation with Amelia using the [start a new conversation endpoint](#start-a-new-conversation).

> The above command, when successful, simply returns a 200 OK message.

## Poll a conversation

> <font size="4">POST /AmeliaRest/api/v1/conversations/{sessionId}/poll</font>

> Example Request:

```python
import amelia

amelia.conversation().poll(url, token, sessionId)
```

```shell
curl -kv --header 'Content-Type: application/json'
-X POST "https://{ameliaUrl}/AmeliaRest/api/v1/conversations/sessionId/poll"
-H 'X-Amelia-Rest-Token: token'
```

This endpoint polls a conversation and returns data about a conversation. The sessionId can be obtained when starting a conversation with Amelia using the [start a new conversation endpoint](#start-a-new-conversation).

> Example Response:

```json
{
		"messageId": "7e4a4698-9f8e-408d-aba8-deb2368c31b6",
		"headers": {
			"X-Amelia-Conversation-Id": "LIX7HSR7YAIAA-1",
			"X-Amelia-Self-Echo": "false",
			"X-Amelia-Clock-Skew": "167",
			"X-Amelia-Session-Sequence": "5",
			"X-Amelia-Session-Id": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
			"X-Amelia-Input-Enabled": "false",
			"X-Amelia-Debug-Message": "false",
			"X-Amelia-Message-Id": "7e4a4698-9f8e-408d-aba8-deb2368c31b6",
			"X-Amelia-Timestamp": "1549406252772",
			"X-Amelia-Session-Mode": "USER",
			"X-Amelia-Source-User-Type": "Amelia",
			"X-Amelia-Message-Type": "OutboundBmlStatusMessage"
		},
		"messageText": null,
		"inResponseToMessageId": "40cb6e06-d389-476c-a637-3347d6e046ca",
		"fromUserDisplayName": "Amelia",
		"secureResponse": false,
		"bml": "[[\"{\\\"Speech1595985\\\":{\\\"text\\\":\\\"<rate absspeed='-1'> _SyncWord0_ Hello. I’m <rate absspeed='1'> still </rate> <rate absspeed='1'> learning </rate> and _SyncWord5_ my <rate absspeed='1'> knowledge </rate> _SyncWord7_ is <rate absspeed='1'> growing </rate> _SyncWord9_ all the time. _SyncWord12_ Some tips : <rate absspeed='1'> Use </rate> _SyncWord16_ <rate absspeed='1'> full </rate> sentences or questionsType `` What <rate absspeed='1'> can </rate> _SyncWord23_ I _SyncWord24_ ask?'' to <rate absspeed='1'> see </rate> what I _SyncWord30_ <rate absspeed='1'> can </rate> <rate absspeed='1'> do </rate> </rate>\\\",\\\"start\\\":\\\"0\\\"},\\\"Gest1596007\\\":{\\\"speed\\\":\\\"1\\\",\\\"name\\\":\\\"MeLf01\\\",\\\"start\\\":\\\"Speech1595985:_SyncWord5_\\\"},\\\"Gest1596009\\\":{\\\"speed\\\":\\\"1\\\",\\\"name\\\":\\\"BeatMidLf01\\\",\\\"start\\\":\\\"Speech1595985:_SyncWord23_\\\"},\\\"Gest1596005\\\":{\\\"speed\\\":\\\"1\\\",\\\"name\\\":\\\"WeRt01\\\",\\\"start\\\":\\\"Speech1595985:_SyncWord30_\\\"},\\\"facs1595998\\\":{\\\"amount\\\":\\\"0.5\\\",\\\"au\\\":\\\"62\\\",\\\"attackPeak\\\":\\\"0.5\\\",\\\"name\\\":\\\"Facs\\\",\\\"side\\\":\\\"BOTH\\\",\\\"end\\\":\\\"1\\\",\\\"relax\\\":\\\"0.5\\\",\\\"start\\\":\\\"Speech1595985:_SyncWord0_\\\"},\\\"faceLexeme30\\\":{\\\"amount\\\":\\\"0.45\\\",\\\"attackPeak\\\":\\\"0.25\\\",\\\"lexeme\\\":\\\"AMUSED\\\",\\\"end\\\":\\\"1.0\\\",\\\"relax\\\":\\\"0.75\\\",\\\"start\\\":\\\"0\\\"},\\\"facs1595987\\\":{\\\"amount\\\":\\\"0.4\\\",\\\"au\\\":\\\"1\\\",\\\"attackPeak\\\":\\\"Speech1595985:_SyncWord9_+0.5\\\",\\\"name\\\":\\\"Facs\\\",\\\"side\\\":\\\"BOTH\\\",\\\"end\\\":\\\"Speech1595985:_SyncWord9_+1\\\",\\\"relax\\\":\\\"Speech1595985:_SyncWord9_+0.5\\\",\\\"start\\\":\\\"Speech1595985:_SyncWord9_\\\"},\\\"facs1595988\\\":{\\\"amount\\\":\\\"0.4\\\",\\\"au\\\":\\\"2\\\",\\\"attackPeak\\\":\\\"Speech1595985:_SyncWord9_+0.5\\\",\\\"name\\\":\\\"Facs\\\",\\\"side\\\":\\\"BOTH\\\",\\\"end\\\":\\\"Speech1595985:_SyncWord9_+1\\\",\\\"relax\\\":\\\"Speech1595985:_SyncWord9_+0.5\\\",\\\"start\\\":\\\"Speech1595985:_SyncWord9_\\\"},\\\"facs1596010\\\":{\\\"amount\\\":\\\"0.3\\\",\\\"au\\\":\\\"55\\\",\\\"attackPeak\\\":\\\"0.5\\\",\\\"name\\\":\\\"HEAD\\\",\\\"end\\\":\\\"1\\\",\\\"relax\\\":\\\"0.5\\\",\\\"start\\\":\\\"Speech1595985:_SyncWord5_\\\"},\\\"facs1596011\\\":{\\\"amount\\\":\\\"0.3\\\",\\\"au\\\":\\\"56\\\",\\\"attackPeak\\\":\\\"0.5\\\",\\\"name\\\":\\\"HEAD\\\",\\\"end\\\":\\\"1\\\",\\\"relax\\\":\\\"0.5\\\",\\\"start\\\":\\\"Speech1595985:_SyncWord23_\\\"},\\\"Head63\\\":{\\\"amount\\\":\\\"0.1\\\",\\\"lexeme\\\":\\\"up\\\",\\\"speed\\\":\\\"1\\\",\\\"repeat\\\":\\\"1\\\",\\\"end\\\":\\\"2\\\",\\\"start\\\":\\\"0\\\"},\\\"faceShift1596012\\\":{\\\"amount\\\":\\\"0.0\\\",\\\"au\\\":\\\"6\\\",\\\"attackPeak\\\":\\\"0.01\\\",\\\"name\\\":\\\"faceShift\\\",\\\"side\\\":\\\"BOTH\\\",\\\"start\\\":\\\"0\\\"},\\\"faceShift1596013\\\":{\\\"amount\\\":\\\"0.0\\\",\\\"au\\\":\\\"12\\\",\\\"attackPeak\\\":\\\"0.01\\\",\\\"name\\\":\\\"faceShift\\\",\\\"side\\\":\\\"BOTH\\\",\\\"start\\\":\\\"0\\\"},\\\"faceLexeme1\\\":{\\\"amount\\\":\\\"0\\\",\\\"attackPeak\\\":\\\"0.01\\\",\\\"lexeme\\\":\\\"SIMPLE_SMILE\\\",\\\"end\\\":\\\"0.03\\\",\\\"relax\\\":\\\"0.02\\\",\\\"start\\\":\\\"0\\\"},\\\"facs1595995\\\":{\\\"amount\\\":\\\"0.4\\\",\\\"au\\\":\\\"1\\\",\\\"attackPeak\\\":\\\"Speech1595985:_SyncWord16_+0.5\\\",\\\"name\\\":\\\"Facs\\\",\\\"side\\\":\\\"BOTH\\\",\\\"end\\\":\\\"Speech1595985:_SyncWord16_+1\\\",\\\"relax\\\":\\\"Speech1595985:_SyncWord16_+0.5\\\",\\\"start\\\":\\\"Speech1595985:_SyncWord16_\\\"},\\\"facs1595996\\\":{\\\"amount\\\":\\\"0.4\\\",\\\"au\\\":\\\"2\\\",\\\"attackPeak\\\":\\\"Speech1595985:_SyncWord16_+0.5\\\",\\\"name\\\":\\\"Facs\\\",\\\"side\\\":\\\"BOTH\\\",\\\"end\\\":\\\"Speech1595985:_SyncWord16_+1\\\",\\\"relax\\\":\\\"Speech1595985:_SyncWord16_+0.5\\\",\\\"start\\\":\\\"Speech1595985:_SyncWord16_\\\"}}\"]]",
		"selfEcho": false,
		"sessionSequence": 5,
		"inputEnabled": false,
		"ameliaMessageType": "OutboundBmlStatusMessage",
		"messageType": "OutboundBmlStatusMessageFromAmelia",
		"sessionId": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
		"conversationId": "LIX7HSR7YAIAA-1",
		"clockSkew": 167,
		"sourceUserType": "Amelia",
		"timestamp": 1549406252.772000000
	}, {
		"messageId": "81531c02-85d6-4282-bb7a-27a3d9fab530",
		"headers": {
			"X-Amelia-Conversation-Id": "LIX7HSR7YAIAA-1",
			"X-Amelia-Self-Echo": "false",
			"X-Amelia-Clock-Skew": "167",
			"X-Amelia-Session-Sequence": "6",
			"X-Amelia-Session-Id": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
			"X-Amelia-Input-Enabled": "false",
			"X-Amelia-Debug-Message": "false",
			"X-Amelia-Message-Id": "81531c02-85d6-4282-bb7a-27a3d9fab530",
			"X-Amelia-Timestamp": "1549406251620",
			"X-Amelia-Session-Mode": "USER",
			"X-Amelia-Source-User-Type": "Amelia",
			"X-Amelia-Message-Type": "OutboundTextMessage"
		},
		"messageText": "Hello. I’m still learning and my knowledge is growing all the time. Some tips:<ul><li>Use full sentences or questions</li><li>Type \"What can I ask?\" to see what I can do</li></ul>",
		"inResponseToMessageId": "40cb6e06-d389-476c-a637-3347d6e046ca",
		"fromUserDisplayName": "Amelia",
		"secureResponse": false,
		"hintText": null,
		"responsePool": null,
		"emoticon": "AMUSED",
		"selfEcho": false,
		"sessionSequence": 6,
		"inputEnabled": false,
		"ameliaMessageType": "OutboundTextMessage",
		"messageType": "TextMessageFromAmelia",
		"sessionId": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
		"conversationId": "LIX7HSR7YAIAA-1",
		"clockSkew": 167,
		"sourceUserType": "Amelia",
		"timestamp": 1549406251.620000000
	}, {
		"messageId": "606cb813-e176-4455-a241-3c4103c591d2",
		"headers": {
			"X-Amelia-Conversation-Id": "LIX7HSR7YAIAA-1",
			"X-Amelia-Self-Echo": "false",
			"X-Amelia-Clock-Skew": "167",
			"X-Amelia-Session-Sequence": "11",
			"X-Amelia-Session-Id": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
			"X-Amelia-Input-Enabled": "false",
			"X-Amelia-Debug-Message": "false",
			"X-Amelia-Message-Id": "606cb813-e176-4455-a241-3c4103c591d2",
			"X-Amelia-Timestamp": "1549406252939",
			"X-Amelia-Session-Mode": "USER",
			"X-Amelia-Source-User-Type": "Amelia",
			"X-Amelia-Message-Type": "OutboundBmlStatusMessage"
		},
		"messageText": null,
		"inResponseToMessageId": "40cb6e06-d389-476c-a637-3347d6e046ca",
		"fromUserDisplayName": "Amelia",
		"secureResponse": false,
		"bml": "[[\"{\\\"Speech1596014\\\":{\\\"text\\\":\\\"<rate absspeed='-1'> _SyncWord0_ How <rate absspeed='1'> can </rate> _SyncWord2_ I _SyncWord3_ <rate absspeed='1'> help </rate> _SyncWord4_ you? </rate>\\\",\\\"start\\\":\\\"0\\\"},\\\"Gest1596030\\\":{\\\"speed\\\":\\\"1\\\",\\\"name\\\":\\\"WhenBt02\\\",\\\"start\\\":\\\"Speech1596014:_SyncWord0_\\\"},\\\"faceLexeme33\\\":{\\\"amount\\\":\\\"0.6\\\",\\\"attackPeak\\\":\\\"0.25\\\",\\\"lexeme\\\":\\\"JOY\\\",\\\"end\\\":\\\"1.0\\\",\\\"relax\\\":\\\"0.75\\\",\\\"start\\\":\\\"0\\\"},\\\"facs1596018\\\":{\\\"amount\\\":\\\"0.5\\\",\\\"au\\\":\\\"62\\\",\\\"attackPeak\\\":\\\"0.5\\\",\\\"name\\\":\\\"Facs\\\",\\\"side\\\":\\\"BOTH\\\",\\\"end\\\":\\\"1\\\",\\\"relax\\\":\\\"0.5\\\",\\\"start\\\":\\\"Speech1596014:_SyncWord0_\\\"},\\\"facs1596016\\\":{\\\"amount\\\":\\\"0.4\\\",\\\"au\\\":\\\"1\\\",\\\"attackPeak\\\":\\\"Speech1596014:_SyncWord0_+0.5\\\",\\\"name\\\":\\\"Facs\\\",\\\"side\\\":\\\"BOTH\\\",\\\"end\\\":\\\"Speech1596014:_SyncWord0_+1\\\",\\\"relax\\\":\\\"Speech1596014:_SyncWord0_+0.5\\\",\\\"start\\\":\\\"Speech1596014:_SyncWord0_\\\"},\\\"facs1596017\\\":{\\\"amount\\\":\\\"0.4\\\",\\\"au\\\":\\\"2\\\",\\\"attackPeak\\\":\\\"Speech1596014:_SyncWord0_+0.5\\\",\\\"name\\\":\\\"Facs\\\",\\\"side\\\":\\\"BOTH\\\",\\\"end\\\":\\\"Speech1596014:_SyncWord0_+1\\\",\\\"relax\\\":\\\"Speech1596014:_SyncWord0_+0.5\\\",\\\"start\\\":\\\"Speech1596014:_SyncWord0_\\\"},\\\"facs1596029\\\":{\\\"amount\\\":\\\"0.3\\\",\\\"au\\\":\\\"56\\\",\\\"attackPeak\\\":\\\"0.5\\\",\\\"name\\\":\\\"HEAD\\\",\\\"end\\\":\\\"1\\\",\\\"relax\\\":\\\"0.5\\\",\\\"start\\\":\\\"Speech1596014:_SyncWord0_\\\"},\\\"faceShift1596031\\\":{\\\"amount\\\":\\\"0.0\\\",\\\"au\\\":\\\"6\\\",\\\"attackPeak\\\":\\\"0.01\\\",\\\"name\\\":\\\"faceShift\\\",\\\"side\\\":\\\"BOTH\\\",\\\"start\\\":\\\"0\\\"},\\\"faceShift1596032\\\":{\\\"amount\\\":\\\"0.0\\\",\\\"au\\\":\\\"12\\\",\\\"attackPeak\\\":\\\"0.01\\\",\\\"name\\\":\\\"faceShift\\\",\\\"side\\\":\\\"BOTH\\\",\\\"start\\\":\\\"0\\\"},\\\"faceLexeme1\\\":{\\\"amount\\\":\\\"0\\\",\\\"attackPeak\\\":\\\"0.01\\\",\\\"lexeme\\\":\\\"SIMPLE_SMILE\\\",\\\"end\\\":\\\"0.03\\\",\\\"relax\\\":\\\"0.02\\\",\\\"start\\\":\\\"0\\\"}}\"]]",
		"selfEcho": false,
		"sessionSequence": 11,
		"inputEnabled": false,
		"ameliaMessageType": "OutboundBmlStatusMessage",
		"messageType": "OutboundBmlStatusMessageFromAmelia",
		"sessionId": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
		"conversationId": "LIX7HSR7YAIAA-1",
		"clockSkew": 167,
		"sourceUserType": "Amelia",
		"timestamp": 1549406252.939000000
	}, {
		"messageId": "4901c7d1-e92b-49c4-8fee-0afdc9b144b1",
		"headers": {
			"X-Amelia-Conversation-Id": "LIX7HSR7YAIAA-1",
			"X-Amelia-Self-Echo": "false",
			"X-Amelia-Clock-Skew": "167",
			"X-Amelia-Session-Sequence": "12",
			"X-Amelia-Session-Id": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
			"X-Amelia-Input-Enabled": "false",
			"X-Amelia-Debug-Message": "false",
			"X-Amelia-Message-Id": "4901c7d1-e92b-49c4-8fee-0afdc9b144b1",
			"X-Amelia-Timestamp": "1549406252820",
			"X-Amelia-Session-Mode": "USER",
			"X-Amelia-Source-User-Type": "Amelia",
			"X-Amelia-Message-Type": "OutboundTextMessage"
		},
		"messageText": "How can I help you?",
		"inResponseToMessageId": "40cb6e06-d389-476c-a637-3347d6e046ca",
		"fromUserDisplayName": "Amelia",
		"secureResponse": false,
		"hintText": null,
		"responsePool": null,
		"emoticon": "JOY",
		"selfEcho": false,
		"sessionSequence": 12,
		"inputEnabled": false,
		"ameliaMessageType": "OutboundTextMessage",
		"messageType": "TextMessageFromAmelia",
		"sessionId": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
		"conversationId": "LIX7HSR7YAIAA-1",
		"clockSkew": 167,
		"sourceUserType": "Amelia",
		"timestamp": 1549406252.820000000
	}, {
		"messageId": "89cadac7-cca2-4799-aca0-fe473fb30c82",
		"headers": {
			"X-Amelia-Conversation-Id": "LIX7HSR7YAIAA-1",
			"X-Amelia-Self-Echo": "false",
			"X-Amelia-Clock-Skew": "167",
			"X-Amelia-Session-Sequence": "15",
			"X-Amelia-Session-Id": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
			"X-Amelia-Input-Enabled": "true",
			"X-Amelia-Debug-Message": "false",
			"X-Amelia-Message-Id": "89cadac7-cca2-4799-aca0-fe473fb30c82",
			"X-Amelia-Timestamp": "1549406252969",
			"X-Amelia-Session-Mode": "USER",
			"X-Amelia-Source-User-Type": "Amelia",
			"X-Amelia-Message-Type": "OutboundAmeliaReadyMessage"
		},
		"messageText": null,
		"inResponseToMessageId": "40cb6e06-d389-476c-a637-3347d6e046ca",
		"fromUserDisplayName": "Amelia",
		"secureResponse": false,
		"selfEcho": false,
		"sessionSequence": 15,
		"inputEnabled": true,
		"ameliaMessageType": "OutboundAmeliaReadyMessage",
		"messageType": "AmeliaReadyMessageFromAmelia",
		"sessionId": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
		"conversationId": "LIX7HSR7YAIAA-1",
		"clockSkew": 167,
		"sourceUserType": "Amelia",
		"timestamp": 1549406252.969000000
	}, {
		"messageId": "5758e8ed-07e4-4048-b0a1-5012da12a43c",
		"headers": {
			"X-Amelia-Conversation-Id": "LIX7HSR7YAIAA-1",
			"X-Amelia-Self-Echo": "true",
			"X-Amelia-Clock-Skew": "167",
			"X-Amelia-Session-Sequence": "16",
			"X-Amelia-Session-Id": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
			"X-Amelia-Input-Enabled": "false",
			"X-Amelia-Debug-Message": "false",
			"X-Amelia-Message-Id": "5758e8ed-07e4-4048-b0a1-5012da12a43c",
			"X-Amelia-Timestamp": "1549406400454",
			"X-Amelia-Session-Mode": "USER",
			"X-Amelia-Source-User-Type": "User",
			"X-Amelia-Message-Type": "OutboundEchoMessage"
		},
		"messageText": "hi",
		"inResponseToMessageId": "5b1da039-f231-42c0-b11f-20563c11d731",
		"fromUserDisplayName": "User Name",
		"secureResponse": false,
		"selfEcho": true,
		"sessionSequence": 16,
		"inputEnabled": false,
		"ameliaMessageType": "OutboundEchoMessage",
		"messageType": "EchoMessageFromAmelia",
		"sessionId": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
		"conversationId": "LIX7HSR7YAIAA-1",
		"clockSkew": 167,
		"sourceUserType": "User",
		"timestamp": 1549406400.454000000
	}, {
		"messageId": "2598aaa1-1a66-4008-80b8-f263bfe65ca7",
		"headers": {
			"X-Amelia-Conversation-Id": "LIX7HSR7YAIAA-1",
			"X-Amelia-Self-Echo": "false",
			"X-Amelia-Clock-Skew": "167",
			"X-Amelia-Session-Sequence": "21",
			"X-Amelia-Session-Id": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
			"X-Amelia-Input-Enabled": "false",
			"X-Amelia-Debug-Message": "false",
			"X-Amelia-Message-Id": "2598aaa1-1a66-4008-80b8-f263bfe65ca7",
			"X-Amelia-Timestamp": "1549406401152",
			"X-Amelia-Session-Mode": "USER",
			"X-Amelia-Source-User-Type": "Amelia",
			"X-Amelia-Message-Type": "OutboundBmlStatusMessage"
		},
		"messageText": null,
		"inResponseToMessageId": "5b1da039-f231-42c0-b11f-20563c11d731",
		"fromUserDisplayName": "Amelia",
		"secureResponse": false,
		"bml": "[[\"{\\\"Speech1596033\\\":{\\\"text\\\":\\\"<rate absspeed='-3'> _SyncWord0_ I'm not _SyncWord2_ <rate absspeed='0'> sure </rate> _SyncWord3_ how to answer. </rate>\\\",\\\"start\\\":\\\"0\\\"},\\\"Gest1596042\\\":{\\\"speed\\\":\\\"1\\\",\\\"name\\\":\\\"BeatMidRt01\\\",\\\"start\\\":\\\"Speech1596033:_SyncWord0_\\\"},\\\"facs1596035\\\":{\\\"amount\\\":\\\"0.5\\\",\\\"au\\\":\\\"61\\\",\\\"attackPeak\\\":\\\"0.5\\\",\\\"name\\\":\\\"Facs\\\",\\\"side\\\":\\\"BOTH\\\",\\\"end\\\":\\\"1\\\",\\\"relax\\\":\\\"0.5\\\",\\\"start\\\":\\\"Speech1596033:_SyncWord0_\\\"},\\\"facs1596044\\\":{\\\"amount\\\":\\\"0.3\\\",\\\"au\\\":\\\"56\\\",\\\"attackPeak\\\":\\\"0.5\\\",\\\"name\\\":\\\"HEAD\\\",\\\"end\\\":\\\"1\\\",\\\"relax\\\":\\\"0.5\\\",\\\"start\\\":\\\"Speech1596033:_SyncWord0_\\\"},\\\"faceShift1596045\\\":{\\\"amount\\\":\\\"0.0\\\",\\\"au\\\":\\\"6\\\",\\\"attackPeak\\\":\\\"0.01\\\",\\\"name\\\":\\\"faceShift\\\",\\\"side\\\":\\\"BOTH\\\",\\\"start\\\":\\\"0\\\"},\\\"faceShift1596046\\\":{\\\"amount\\\":\\\"0.0\\\",\\\"au\\\":\\\"12\\\",\\\"attackPeak\\\":\\\"0.01\\\",\\\"name\\\":\\\"faceShift\\\",\\\"side\\\":\\\"BOTH\\\",\\\"start\\\":\\\"0\\\"},\\\"faceLexeme1\\\":{\\\"amount\\\":\\\"0\\\",\\\"attackPeak\\\":\\\"0.01\\\",\\\"lexeme\\\":\\\"SIMPLE_SMILE\\\",\\\"end\\\":\\\"0.03\\\",\\\"relax\\\":\\\"0.02\\\",\\\"start\\\":\\\"0\\\"}}\"]]",
		"selfEcho": false,
		"sessionSequence": 21,
		"inputEnabled": false,
		"ameliaMessageType": "OutboundBmlStatusMessage",
		"messageType": "OutboundBmlStatusMessageFromAmelia",
		"sessionId": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
		"conversationId": "LIX7HSR7YAIAA-1",
		"clockSkew": 167,
		"sourceUserType": "Amelia",
		"timestamp": 1549406401.152000000
	}, {
		"messageId": "99e5e13d-0949-4be9-97ef-b239b8a2a191",
		"headers": {
			"X-Amelia-Conversation-Id": "LIX7HSR7YAIAA-1",
			"X-Amelia-Self-Echo": "false",
			"X-Amelia-Clock-Skew": "167",
			"X-Amelia-Session-Sequence": "22",
			"X-Amelia-Session-Id": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
			"X-Amelia-Input-Enabled": "false",
			"X-Amelia-Debug-Message": "false",
			"X-Amelia-Message-Id": "99e5e13d-0949-4be9-97ef-b239b8a2a191",
			"X-Amelia-Timestamp": "1549406400934",
			"X-Amelia-Session-Mode": "USER",
			"X-Amelia-Source-User-Type": "Amelia",
			"X-Amelia-Message-Type": "OutboundTextMessage"
		},
		"messageText": "I'm not sure how to answer.",
		"inResponseToMessageId": "5b1da039-f231-42c0-b11f-20563c11d731",
		"fromUserDisplayName": "Amelia",
		"secureResponse": false,
		"hintText": null,
		"responsePool": "DONTKNOW",
		"emoticon": "SIMPLE_SMILE",
		"selfEcho": false,
		"sessionSequence": 22,
		"inputEnabled": false,
		"ameliaMessageType": "OutboundTextMessage",
		"messageType": "TextMessageFromAmelia",
		"sessionId": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
		"conversationId": "LIX7HSR7YAIAA-1",
		"clockSkew": 167,
		"sourceUserType": "Amelia",
		"timestamp": 1549406400.934000000
	}, {
		"messageId": "d6168518-94db-4fcd-ac8c-59dc9468feeb",
		"headers": {
			"X-Amelia-Conversation-Id": "LIX7HSR7YAIAA-1",
			"X-Amelia-Self-Echo": "false",
			"X-Amelia-Clock-Skew": "167",
			"X-Amelia-Session-Sequence": "34",
			"X-Amelia-Session-Id": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
			"X-Amelia-Input-Enabled": "false",
			"X-Amelia-Debug-Message": "false",
			"X-Amelia-Message-Id": "d6168518-94db-4fcd-ac8c-59dc9468feeb",
			"X-Amelia-Timestamp": "1549406402307",
			"X-Amelia-Session-Mode": "USER",
			"X-Amelia-Source-User-Type": "Amelia",
			"X-Amel* Connection #0 to host {ameliaUrl}[{
		"messageId": "7e4a4698-9f8e-408d-aba8-deb2368c31b6",
		"headers": {
			"X-Amelia-Conversation-Id": "LIX7HSR7YAIAA-1",
			"X-Amelia-Self-Echo": "false",
			"X-Amelia-Clock-Skew": "167",
			"X-Amelia-Session-Sequence": "5",
			"X-Amelia-Session-Id": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
			"X-Amelia-Input-Enabled": "false",
			"X-Amelia-Debug-Message": "false",
			"X-Amelia-Message-Id": "7e4a4698-9f8e-408d-aba8-deb2368c31b6",
			"X-Amelia-Timestamp": "1549406252772",
			"X-Amelia-Session-Mode": "USER",
			"X-Amelia-Source-User-Type": "Amelia",
			"X-Amelia-Message-Type": "OutboundBmlStatusMessage"
		},
		"messageText": null,
		"inResponseToMessageId": "40cb6e06-d389-476c-a637-3347d6e046ca",
		"fromUserDisplayName": "Amelia",
		"secureResponse": false,
		"bml": "[[\"{\\\"Speech1595985\\\":{\\\"text\\\":\\\"<rate absspeed='-1'> _SyncWord0_ Hello. I’m <rate absspeed='1'> still </rate> <rate absspeed='1'> learning </rate> and _SyncWord5_ my <rate absspeed='1'> knowledge </rate> _SyncWord7_ is <rate absspeed='1'> growing </rate> _SyncWord9_ all the time. _SyncWord12_ Some tips : <rate absspeed='1'> Use </rate> _SyncWord16_ <rate absspeed='1'> full </rate> sentences or questionsType `` What <rate absspeed='1'> can </rate> _SyncWord23_ I _SyncWord24_ ask?'' to <rate absspeed='1'> see </rate> what I _SyncWord30_ <rate absspeed='1'> can </rate> <rate absspeed='1'> do </rate> </rate>\\\",\\\"start\\\":\\\"0\\\"},\\\"Gest1596007\\\":{\\\"speed\\\":\\\"1\\\",\\\"name\\\":\\\"MeLf01\\\",\\\"start\\\":\\\"Speech1595985:_SyncWord5_\\\"},\\\"Gest1596009\\\":{\\\"speed\\\":\\\"1\\\",\\\"name\\\":\\\"BeatMidLf01\\\",\\\"start\\\":\\\"Speech1595985:_SyncWord23_\\\"},\\\"Gest1596005\\\":{\\\"speed\\\":\\\"1\\\",\\\"name\\\":\\\"WeRt01\\\",\\\"start\\\":\\\"Speech1595985:_SyncWord30_\\\"},\\\"facs1595998\\\":{\\\"amount\\\":\\\"0.5\\\",\\\"au\\\":\\\"62\\\",\\\"attackPeak\\\":\\\"0.5\\\",\\\"name\\\":\\\"Facs\\\",\\\"side\\\":\\\"BOTH\\\",\\\"end\\\":\\\"1\\\",\\\"relax\\\":\\\"0.5\\\",\\\"start\\\":\\\"Speech1595985:_SyncWord0_\\\"},\\\"faceLexeme30\\\":{\\\"amount\\\":\\\"0.45\\\",\\\"attackPeak\\\":\\\"0.25\\\",\\\"lexeme\\\":\\\"AMUSED\\\",\\\"end\\\":\\\"1.0\\\",\\\"relax\\\":\\\"0.75\\\",\\\"start\\\":\\\"0\\\"},\\\"facs1595987\\\":{\\\"amount\\\":\\\"0.4\\\",\\\"au\\\":\\\"1\\\",\\\"attackPeak\\\":\\\"Speech1595985:_SyncWord9_+0.5\\\",\\\"name\\\":\\\"Facs\\\",\\\"side\\\":\\\"BOTH\\\",\\\"end\\\":\\\"Speech1595985:_SyncWord9_+1\\\",\\\"relax\\\":\\\"Speech1595985:_SyncWord9_+0.5\\\",\\\"start\\\":\\\"Speech1595985:_SyncWord9_\\\"},\\\"facs1595988\\\":{\\\"amount\\\":\\\"0.4\\\",\\\"au\\\":\\\"2\\\",\\\"attackPeak\\\":\\\"Speech1595985:_SyncWord9_+0.5\\\",\\\"name\\\":\\\"Facs\\\",\\\"side\\\":\\\"BOTH\\\",\\\"end\\\":\\\"Speech1595985:_SyncWord9_+1\\\",\\\"relax\\\":\\\"Speech1595985:_SyncWord9_+0.5\\\",\\\"start\\\":\\\"Speech1595985:_SyncWord9_\\\"},\\\"facs1596010\\\":{\\\"amount\\\":\\\"0.3\\\",\\\"au\\\":\\\"55\\\",\\\"attackPeak\\\":\\\"0.5\\\",\\\"name\\\":\\\"HEAD\\\",\\\"end\\\":\\\"1\\\",\\\"relax\\\":\\\"0.5\\\",\\\"start\\\":\\\"Speech1595985:_SyncWord5_\\\"},\\\"facs1596011\\\":{\\\"amount\\\":\\\"0.3\\\",\\\"au\\\":\\\"56\\\",\\\"attackPeak\\\":\\\"0.5\\\",\\\"name\\\":\\\"HEAD\\\",\\\"end\\\":\\\"1\\\",\\\"relax\\\":\\\"0.5\\\",\\\"start\\\":\\\"Speech1595985:_SyncWord23_\\\"},\\\"Head63\\\":{\\\"amount\\\":\\\"0.1\\\",\\\"lexeme\\\":\\\"up\\\",\\\"speed\\\":\\\"1\\\",\\\"repeat\\\":\\\"1\\\",\\\"end\\\":\\\"2\\\",\\\"start\\\":\\\"0\\\"},\\\"faceShift1596012\\\":{\\\"amount\\\":\\\"0.0\\\",\\\"au\\\":\\\"6\\\",\\\"attackPeak\\\":\\\"0.01\\\",\\\"name\\\":\\\"faceShift\\\",\\\"side\\\":\\\"BOTH\\\",\\\"start\\\":\\\"0\\\"},\\\"faceShift1596013\\\":{\\\"amount\\\":\\\"0.0\\\",\\\"au\\\":\\\"12\\\",\\\"attackPeak\\\":\\\"0.01\\\",\\\"name\\\":\\\"faceShift\\\",\\\"side\\\":\\\"BOTH\\\",\\\"start\\\":\\\"0\\\"},\\\"faceLexeme1\\\":{\\\"amount\\\":\\\"0\\\",\\\"attackPeak\\\":\\\"0.01\\\",\\\"lexeme\\\":\\\"SIMPLE_SMILE\\\",\\\"end\\\":\\\"0.03\\\",\\\"relax\\\":\\\"0.02\\\",\\\"start\\\":\\\"0\\\"},\\\"facs1595995\\\":{\\\"amount\\\":\\\"0.4\\\",\\\"au\\\":\\\"1\\\",\\\"attackPeak\\\":\\\"Speech1595985:_SyncWord16_+0.5\\\",\\\"name\\\":\\\"Facs\\\",\\\"side\\\":\\\"BOTH\\\",\\\"end\\\":\\\"Speech1595985:_SyncWord16_+1\\\",\\\"relax\\\":\\\"Speech1595985:_SyncWord16_+0.5\\\",\\\"start\\\":\\\"Speech1595985:_SyncWord16_\\\"},\\\"facs1595996\\\":{\\\"amount\\\":\\\"0.4\\\",\\\"au\\\":\\\"2\\\",\\\"attackPeak\\\":\\\"Speech1595985:_SyncWord16_+0.5\\\",\\\"name\\\":\\\"Facs\\\",\\\"side\\\":\\\"BOTH\\\",\\\"end\\\":\\\"Speech1595985:_SyncWord16_+1\\\",\\\"relax\\\":\\\"Speech1595985:_SyncWord16_+0.5\\\",\\\"start\\\":\\\"Speech1595985:_SyncWord16_\\\"}}\"]]",
		"selfEcho": false,
		"sessionSequence": 5,
		"inputEnabled": false,
		"ameliaMessageType": "OutboundBmlStatusMessage",
		"messageType": "OutboundBmlStatusMessageFromAmelia",
		"sessionId": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
		"conversationId": "LIX7HSR7YAIAA-1",
		"clockSkew": 167,
		"sourceUserType": "Amelia",
		"timestamp": 1549406252.772000000
	}, {
		"messageId": "81531c02-85d6-4282-bb7a-27a3d9fab530",
		"headers": {
			"X-Amelia-Conversation-Id": "LIX7HSR7YAIAA-1",
			"X-Amelia-Self-Echo": "false",
			"X-Amelia-Clock-Skew": "167",
			"X-Amelia-Session-Sequence": "6",
			"X-Amelia-Session-Id": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
			"X-Amelia-Input-Enabled": "false",
			"X-Amelia-Debug-Message": "false",
			"X-Amelia-Message-Id": "81531c02-85d6-4282-bb7a-27a3d9fab530",
			"X-Amelia-Timestamp": "1549406251620",
			"X-Amelia-Session-Mode": "USER",
			"X-Amelia-Source-User-Type": "Amelia",
			"X-Amelia-Message-Type": "OutboundTextMessage"
		},
		"messageText": "Hello. I’m still learning and my knowledge is growing all the time. Some tips:<ul><li>Use full sentences or questions</li><li>Type \"What can I ask?\" to see what I can do</li></ul>",
		"inResponseToMessageId": "40cb6e06-d389-476c-a637-3347d6e046ca",
		"fromUserDisplayName": "Amelia",
		"secureResponse": false,
		"hintText": null,
		"responsePool": null,
		"emoticon": "AMUSED",
		"selfEcho": false,
		"sessionSequence": 6,
		"inputEnabled": false,
		"ameliaMessageType": "OutboundTextMessage",
		"messageType": "TextMessageFromAmelia",
		"sessionId": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
		"conversationId": "LIX7HSR7YAIAA-1",
		"clockSkew": 167,
		"sourceUserType": "Amelia",
		"timestamp": 1549406251.620000000
	}, {
		"messageId": "606cb813-e176-4455-a241-3c4103c591d2",
		"headers": {
			"X-Amelia-Conversation-Id": "LIX7HSR7YAIAA-1",
			"X-Amelia-Self-Echo": "false",
			"X-Amelia-Clock-Skew": "167",
			"X-Amelia-Session-Sequence": "11",
			"X-Amelia-Session-Id": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
			"X-Amelia-Input-Enabled": "false",
			"X-Amelia-Debug-Message": "false",
			"X-Amelia-Message-Id": "606cb813-e176-4455-a241-3c4103c591d2",
			"X-Amelia-Timestamp": "1549406252939",
			"X-Amelia-Session-Mode": "USER",
			"X-Amelia-Source-User-Type": "Amelia",
			"X-Amelia-Message-Type": "OutboundBmlStatusMessage"
		},
		"messageText": null,
		"inResponseToMessageId": "40cb6e06-d389-476c-a637-3347d6e046ca",
		"fromUserDisplayName": "Amelia",
		"secureResponse": false,
		"bml": "[[\"{\\\"Speech1596014\\\":{\\\"text\\\":\\\"<rate absspeed='-1'> _SyncWord0_ How <rate absspeed='1'> can </rate> _SyncWord2_ I _SyncWord3_ <rate absspeed='1'> help </rate> _SyncWord4_ you? </rate>\\\",\\\"start\\\":\\\"0\\\"},\\\"Gest1596030\\\":{\\\"speed\\\":\\\"1\\\",\\\"name\\\":\\\"WhenBt02\\\",\\\"start\\\":\\\"Speech1596014:_SyncWord0_\\\"},\\\"faceLexeme33\\\":{\\\"amount\\\":\\\"0.6\\\",\\\"attackPeak\\\":\\\"0.25\\\",\\\"lexeme\\\":\\\"JOY\\\",\\\"end\\\":\\\"1.0\\\",\\\"relax\\\":\\\"0.75\\\",\\\"start\\\":\\\"0\\\"},\\\"facs1596018\\\":{\\\"amount\\\":\\\"0.5\\\",\\\"au\\\":\\\"62\\\",\\\"attackPeak\\\":\\\"0.5\\\",\\\"name\\\":\\\"Facs\\\",\\\"side\\\":\\\"BOTH\\\",\\\"end\\\":\\\"1\\\",\\\"relax\\\":\\\"0.5\\\",\\\"start\\\":\\\"Speech1596014:_SyncWord0_\\\"},\\\"facs1596016\\\":{\\\"amount\\\":\\\"0.4\\\",\\\"au\\\":\\\"1\\\",\\\"attackPeak\\\":\\\"Speech1596014:_SyncWord0_+0.5\\\",\\\"name\\\":\\\"Facs\\\",\\\"side\\\":\\\"BOTH\\\",\\\"end\\\":\\\"Speech1596014:_SyncWord0_+1\\\",\\\"relax\\\":\\\"Speech1596014:_SyncWord0_+0.5\\\",\\\"start\\\":\\\"Speech1596014:_SyncWord0_\\\"},\\\"facs1596017\\\":{\\\"amount\\\":\\\"0.4\\\",\\\"au\\\":\\\"2\\\",\\\"attackPeak\\\":\\\"Speech1596014:_SyncWord0_+0.5\\\",\\\"name\\\":\\\"Facs\\\",\\\"side\\\":\\\"BOTH\\\",\\\"end\\\":\\\"Speech1596014:_SyncWord0_+1\\\",\\\"relax\\\":\\\"Speech1596014:_SyncWord0_+0.5\\\",\\\"start\\\":\\\"Speech1596014:_SyncWord0_\\\"},\\\"facs1596029\\\":{\\\"amount\\\":\\\"0.3\\\",\\\"au\\\":\\\"56\\\",\\\"attackPeak\\\":\\\"0.5\\\",\\\"name\\\":\\\"HEAD\\\",\\\"end\\\":\\\"1\\\",\\\"relax\\\":\\\"0.5\\\",\\\"start\\\":\\\"Speech1596014:_SyncWord0_\\\"},\\\"faceShift1596031\\\":{\\\"amount\\\":\\\"0.0\\\",\\\"au\\\":\\\"6\\\",\\\"attackPeak\\\":\\\"0.01\\\",\\\"name\\\":\\\"faceShift\\\",\\\"side\\\":\\\"BOTH\\\",\\\"start\\\":\\\"0\\\"},\\\"faceShift1596032\\\":{\\\"amount\\\":\\\"0.0\\\",\\\"au\\\":\\\"12\\\",\\\"attackPeak\\\":\\\"0.01\\\",\\\"name\\\":\\\"faceShift\\\",\\\"side\\\":\\\"BOTH\\\",\\\"start\\\":\\\"0\\\"},\\\"faceLexeme1\\\":{\\\"amount\\\":\\\"0\\\",\\\"attackPeak\\\":\\\"0.01\\\",\\\"lexeme\\\":\\\"SIMPLE_SMILE\\\",\\\"end\\\":\\\"0.03\\\",\\\"relax\\\":\\\"0.02\\\",\\\"start\\\":\\\"0\\\"}}\"]]",
		"selfEcho": false,
		"sessionSequence": 11,
		"inputEnabled": false,
		"ameliaMessageType": "OutboundBmlStatusMessage",
		"messageType": "OutboundBmlStatusMessageFromAmelia",
		"sessionId": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
		"conversationId": "LIX7HSR7YAIAA-1",
		"clockSkew": 167,
		"sourceUserType": "Amelia",
		"timestamp": 1549406252.939000000
	}, {
		"messageId": "4901c7d1-e92b-49c4-8fee-0afdc9b144b1",
		"headers": {
			"X-Amelia-Conversation-Id": "LIX7HSR7YAIAA-1",
			"X-Amelia-Self-Echo": "false",
			"X-Amelia-Clock-Skew": "167",
			"X-Amelia-Session-Sequence": "12",
			"X-Amelia-Session-Id": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
			"X-Amelia-Input-Enabled": "false",
			"X-Amelia-Debug-Message": "false",
			"X-Amelia-Message-Id": "4901c7d1-e92b-49c4-8fee-0afdc9b144b1",
			"X-Amelia-Timestamp": "1549406252820",
			"X-Amelia-Session-Mode": "USER",
			"X-Amelia-Source-User-Type": "Amelia",
			"X-Amelia-Message-Type": "OutboundTextMessage"
		},
		"messageText": "How can I help you?",
		"inResponseToMessageId": "40cb6e06-d389-476c-a637-3347d6e046ca",
		"fromUserDisplayName": "Amelia",
		"secureResponse": false,
		"hintText": null,
		"responsePool": null,
		"emoticon": "JOY",
		"selfEcho": false,
		"sessionSequence": 12,
		"inputEnabled": false,
		"ameliaMessageType": "OutboundTextMessage",
		"messageType": "TextMessageFromAmelia",
		"sessionId": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
		"conversationId": "LIX7HSR7YAIAA-1",
		"clockSkew": 167,
		"sourceUserType": "Amelia",
		"timestamp": 1549406252.820000000
	}, {
		"messageId": "89cadac7-cca2-4799-aca0-fe473fb30c82",
		"headers": {
			"X-Amelia-Conversation-Id": "LIX7HSR7YAIAA-1",
			"X-Amelia-Self-Echo": "false",
			"X-Amelia-Clock-Skew": "167",
			"X-Amelia-Session-Sequence": "15",
			"X-Amelia-Session-Id": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
			"X-Amelia-Input-Enabled": "true",
			"X-Amelia-Debug-Message": "false",
			"X-Amelia-Message-Id": "89cadac7-cca2-4799-aca0-fe473fb30c82",
			"X-Amelia-Timestamp": "1549406252969",
			"X-Amelia-Session-Mode": "USER",
			"X-Amelia-Source-User-Type": "Amelia",
			"X-Amelia-Message-Type": "OutboundAmeliaReadyMessage"
		},
		"messageText": null,
		"inResponseToMessageId": "40cb6e06-d389-476c-a637-3347d6e046ca",
		"fromUserDisplayName": "Amelia",
		"secureResponse": false,
		"selfEcho": false,
		"sessionSequence": 15,
		"inputEnabled": true,
		"ameliaMessageType": "OutboundAmeliaReadyMessage",
		"messageType": "AmeliaReadyMessageFromAmelia",
		"sessionId": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
		"conversationId": "LIX7HSR7YAIAA-1",
		"clockSkew": 167,
		"sourceUserType": "Amelia",
		"timestamp": 1549406252.969000000
	}, {
		"messageId": "5758e8ed-07e4-4048-b0a1-5012da12a43c",
		"headers": {
			"X-Amelia-Conversation-Id": "LIX7HSR7YAIAA-1",
			"X-Amelia-Self-Echo": "true",
			"X-Amelia-Clock-Skew": "167",
			"X-Amelia-Session-Sequence": "16",
			"X-Amelia-Session-Id": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
			"X-Amelia-Input-Enabled": "false",
			"X-Amelia-Debug-Message": "false",
			"X-Amelia-Message-Id": "5758e8ed-07e4-4048-b0a1-5012da12a43c",
			"X-Amelia-Timestamp": "1549406400454",
			"X-Amelia-Session-Mode": "USER",
			"X-Amelia-Source-User-Type": "User",
			"X-Amelia-Message-Type": "OutboundEchoMessage"
		},
		"messageText": "hi",
		"inResponseToMessageId": "5b1da039-f231-42c0-b11f-20563c11d731",
		"fromUserDisplayName": "User Name",
		"secureResponse": false,
		"selfEcho": true,
		"sessionSequence": 16,
		"inputEnabled": false,
		"ameliaMessageType": "OutboundEchoMessage",
		"messageType": "EchoMessageFromAmelia",
		"sessionId": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
		"conversationId": "LIX7HSR7YAIAA-1",
		"clockSkew": 167,
		"sourceUserType": "User",
		"timestamp": 1549406400.454000000
	}, {
		"messageId": "2598aaa1-1a66-4008-80b8-f263bfe65ca7",
		"headers": {
			"X-Amelia-Conversation-Id": "LIX7HSR7YAIAA-1",
			"X-Amelia-Self-Echo": "false",
			"X-Amelia-Clock-Skew": "167",
			"X-Amelia-Session-Sequence": "21",
			"X-Amelia-Session-Id": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
			"X-Amelia-Input-Enabled": "false",
			"X-Amelia-Debug-Message": "false",
			"X-Amelia-Message-Id": "2598aaa1-1a66-4008-80b8-f263bfe65ca7",
			"X-Amelia-Timestamp": "1549406401152",
			"X-Amelia-Session-Mode": "USER",
			"X-Amelia-Source-User-Type": "Amelia",
			"X-Amelia-Message-Type": "OutboundBmlStatusMessage"
		},
		"messageText": null,
		"inResponseToMessageId": "5b1da039-f231-42c0-b11f-20563c11d731",
		"fromUserDisplayName": "Amelia",
		"secureResponse": false,
		"bml": "[[\"{\\\"Speech1596033\\\":{\\\"text\\\":\\\"<rate absspeed='-3'> _SyncWord0_ I'm not _SyncWord2_ <rate absspeed='0'> sure </rate> _SyncWord3_ how to answer. </rate>\\\",\\\"start\\\":\\\"0\\\"},\\\"Gest1596042\\\":{\\\"speed\\\":\\\"1\\\",\\\"name\\\":\\\"BeatMidRt01\\\",\\\"start\\\":\\\"Speech1596033:_SyncWord0_\\\"},\\\"facs1596035\\\":{\\\"amount\\\":\\\"0.5\\\",\\\"au\\\":\\\"61\\\",\\\"attackPeak\\\":\\\"0.5\\\",\\\"name\\\":\\\"Facs\\\",\\\"side\\\":\\\"BOTH\\\",\\\"end\\\":\\\"1\\\",\\\"relax\\\":\\\"0.5\\\",\\\"start\\\":\\\"Speech1596033:_SyncWord0_\\\"},\\\"facs1596044\\\":{\\\"amount\\\":\\\"0.3\\\",\\\"au\\\":\\\"56\\\",\\\"attackPeak\\\":\\\"0.5\\\",\\\"name\\\":\\\"HEAD\\\",\\\"end\\\":\\\"1\\\",\\\"relax\\\":\\\"0.5\\\",\\\"start\\\":\\\"Speech1596033:_SyncWord0_\\\"},\\\"faceShift1596045\\\":{\\\"amount\\\":\\\"0.0\\\",\\\"au\\\":\\\"6\\\",\\\"attackPeak\\\":\\\"0.01\\\",\\\"name\\\":\\\"faceShift\\\",\\\"side\\\":\\\"BOTH\\\",\\\"start\\\":\\\"0\\\"},\\\"faceShift1596046\\\":{\\\"amount\\\":\\\"0.0\\\",\\\"au\\\":\\\"12\\\",\\\"attackPeak\\\":\\\"0.01\\\",\\\"name\\\":\\\"faceShift\\\",\\\"side\\\":\\\"BOTH\\\",\\\"start\\\":\\\"0\\\"},\\\"faceLexeme1\\\":{\\\"amount\\\":\\\"0\\\",\\\"attackPeak\\\":\\\"0.01\\\",\\\"lexeme\\\":\\\"SIMPLE_SMILE\\\",\\\"end\\\":\\\"0.03\\\",\\\"relax\\\":\\\"0.02\\\",\\\"start\\\":\\\"0\\\"}}\"]]",
		"selfEcho": false,
		"sessionSequence": 21,
		"inputEnabled": false,
		"ameliaMessageType": "OutboundBmlStatusMessage",
		"messageType": "OutboundBmlStatusMessageFromAmelia",
		"sessionId": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
		"conversationId": "LIX7HSR7YAIAA-1",
		"clockSkew": 167,
		"sourceUserType": "Amelia",
		"timestamp": 1549406401.152000000
	}, {
		"messageId": "99e5e13d-0949-4be9-97ef-b239b8a2a191",
		"headers": {
			"X-Amelia-Conversation-Id": "LIX7HSR7YAIAA-1",
			"X-Amelia-Self-Echo": "false",
			"X-Amelia-Clock-Skew": "167",
			"X-Amelia-Session-Sequence": "22",
			"X-Amelia-Session-Id": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
			"X-Amelia-Input-Enabled": "false",
			"X-Amelia-Debug-Message": "false",
			"X-Amelia-Message-Id": "99e5e13d-0949-4be9-97ef-b239b8a2a191",
			"X-Amelia-Timestamp": "1549406400934",
			"X-Amelia-Session-Mode": "USER",
			"X-Amelia-Source-User-Type": "Amelia",
			"X-Amelia-Message-Type": "OutboundTextMessage"
		},
		"messageText": "I'm not sure how to answer.",
		"inResponseToMessageId": "5b1da039-f231-42c0-b11f-20563c11d731",
		"fromUserDisplayName": "Amelia",
		"secureResponse": false,
		"hintText": null,
		"responsePool": "DONTKNOW",
		"emoticon": "SIMPLE_SMILE",
		"selfEcho": false,
		"sessionSequence": 22,
		"inputEnabled": false,
		"ameliaMessageType": "OutboundTextMessage",
		"messageType": "TextMessageFromAmelia",
		"sessionId": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
		"conversationId": "LIX7HSR7YAIAA-1",
		"clockSkew": 167,
		"sourceUserType": "Amelia",
		"timestamp": 1549406400.934000000
	}, {
		"messageId": "d6168518-94db-4fcd-ac8c-59dc9468feeb",
		"headers": {
			"X-Amelia-Conversation-Id": "LIX7HSR7YAIAA-1",
			"X-Amelia-Self-Echo": "false",
			"X-Amelia-Clock-Skew": "167",
			"X-Amelia-Session-Sequence": "34",
			"X-Amelia-Session-Id": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
			"X-Amelia-Input-Enabled": "false",
			"X-Amelia-Debug-Message": "false",
			"X-Amelia-Message-Id": "d6168518-94db-4fcd-ac8c-59dc9468feeb",
			"X-Amelia-Timestamp": "1549406402307",
			"X-Amelia-Session-Mode": "USER",
			"X-Amelia-Source-User-Type": "Amelia",
			"X-Amel* Connection #0 to host {ameliaUrl} left intact
			ia - Message - Type ":"
			OutboundBmlStatusMessage "},"
			messageText ":null,"
			inResponseToMessageId ":"
			5 b1da039 - f231 - 42 c0 - b11f - 20563 c11d731 ","
			fromUserDisplayName ":"
			Amelia ","
			secureResponse ":false,"
			bml ":" [
				[\"{\\\"Speech1596047\\\":{\\\"text\\\":\\\"<rate absspeed='-2'> _SyncWord0_ <rate absspeed='0'> Try </rate> again, and _SyncWord3_ <rate absspeed='0'> provide </rate> _SyncWord4_ as <rate absspeed='0'> much </rate> <rate absspeed='0'> detail </rate> as possible. </rate>\\\",\\\"start\\\":\\\"0\\\"},\\\"Gest1596053\\\":{\\\"speed\\\":\\\"1\\\",\\\"name\\\":\\\"WeLf01\\\",\\\"start\\\":\\\"Speech1596047:_SyncWord3_\\\"},\\\"facs1596049\\\":{\\\"amount\\\":\\\"0.5\\\",\\\"au\\\":\\\"61\\\",\\\"attackPeak\\\":\\\"0.5\\\",\\\"name\\\":\\\"Facs\\\",\\\"side\\\":\\\"BOTH\\\",\\\"end\\\":\\\"1\\\",\\\"relax\\\":\\\"0.5\\\",\\\"start\\\":\\\"Speech1596047:_SyncWord0_\\\"},\\\"Head1596054\\\":{\\\"amount\\\":\\\"0.2\\\",\\\"lexeme\\\":\\\"sweep\\\",\\\"repeat\\\":\\\"1\\\",\\\"speed\\\":\\\"1\\\",\\\"name\\\":\\\"HEAD\\\",\\\"start\\\":\\\"Speech1596047:_SyncWord0_\\\"},\\\"faceShift1596056\\\":{\\\"amount\\\":\\\"0.0\\\",\\\"au\\\":\\\"6\\\",\\\"attackPeak\\\":\\\"0.01\\\",\\\"name\\\":\\\"faceShift\\\",\\\"side\\\":\\\"BOTH\\\",\\\"start\\\":\\\"0\\\"},\\\"faceShift1596057\\\":{\\\"amount\\\":\\\"0.0\\\",\\\"au\\\":\\\"12\\\",\\\"attackPeak\\\":\\\"0.01\\\",\\\"name\\\":\\\"faceShift\\\",\\\"side\\\":\\\"BOTH\\\",\\\"start\\\":\\\"0\\\"},\\\"faceLexeme1\\\":{\\\"amount\\\":\\\"0\\\",\\\"attackPeak\\\":\\\"0.01\\\",\\\"lexeme\\\":\\\"SIMPLE_SMILE\\\",\\\"end\\\":\\\"0.03\\\",\\\"relax\\\":\\\"0.02\\\",\\\"start\\\":\\\"0\\\"}}\"]]", "selfEcho": false, "sessionSequence": 34, "inputEnabled": false, "ameliaMessageType": "OutboundBmlStatusMessage", "messageType": "OutboundBmlStatusMessageFromAmelia", "sessionId": "ef490d45-a450-4c29-bb01-bc9dc24b5612", "conversationId": "LIX7HSR7YAIAA-1", "clockSkew": 167, "sourceUserType": "Amelia", "timestamp": 1549406402.307000000
				}, {
					"messageId": "a459e608-f6f0-40ed-8cd8-cd97f6ad3c07",
					"headers": {
						"X-Amelia-Conversation-Id": "LIX7HSR7YAIAA-1",
						"X-Amelia-Self-Echo": "false",
						"X-Amelia-Clock-Skew": "167",
						"X-Amelia-Session-Sequence": "35",
						"X-Amelia-Session-Id": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
						"X-Amelia-Input-Enabled": "false",
						"X-Amelia-Debug-Message": "false",
						"X-Amelia-Message-Id": "a459e608-f6f0-40ed-8cd8-cd97f6ad3c07",
						"X-Amelia-Timestamp": "1549406402156",
						"X-Amelia-Session-Mode": "USER",
						"X-Amelia-Source-User-Type": "Amelia",
						"X-Amelia-Message-Type": "OutboundTextMessage"
					},
					"messageText": "Try again, and provide as much detail as possible.",
					"inResponseToMessageId": "5b1da039-f231-42c0-b11f-20563c11d731",
					"fromUserDisplayName": "Amelia",
					"secureResponse": false,
					"hintText": null,
					"responsePool": null,
					"emoticon": "SIMPLE_SMILE",
					"selfEcho": false,
					"sessionSequence": 35,
					"inputEnabled": false,
					"ameliaMessageType": "OutboundTextMessage",
					"messageType": "TextMessageFromAmelia",
					"sessionId": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
					"conversationId": "LIX7HSR7YAIAA-1",
					"clockSkew": 167,
					"sourceUserType": "Amelia",
					"timestamp": 1549406402.156000000
				}, {
					"messageId": "9248b6a9-d752-4a8e-8f9e-3899715ec0bf",
					"headers": {
						"X-Amelia-Conversation-Id": "LIX7HSR7YAIAA-1",
						"X-Amelia-Self-Echo": "false",
						"X-Amelia-Clock-Skew": "167",
						"X-Amelia-Session-Sequence": "40",
						"X-Amelia-Session-Id": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
						"X-Amelia-Input-Enabled": "true",
						"X-Amelia-Debug-Message": "false",
						"X-Amelia-Message-Id": "9248b6a9-d752-4a8e-8f9e-3899715ec0bf",
						"X-Amelia-Timestamp": "1549406402410",
						"X-Amelia-Session-Mode": "USER",
						"X-Amelia-Source-User-Type": "Amelia",
						"X-Amelia-Message-Type": "OutboundAmeliaReadyMessage"
					},
					"messageText": null,
					"inResponseToMessageId": "5b1da039-f231-42c0-b11f-20563c11d731",
					"fromUserDisplayName": "Amelia",
					"secureResponse": false,
					"selfEcho": false,
					"sessionSequence": 40,
					"inputEnabled": true,
					"ameliaMessageType": "OutboundAmeliaReadyMessage",
					"messageType": "AmeliaReadyMessageFromAmelia",
					"sessionId": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
					"conversationId": "LIX7HSR7YAIAA-1",
					"clockSkew": 167,
					"sourceUserType": "Amelia",
					"timestamp": 1549406402.410000000
				}
			] left intact
			ia - Message - Type ":"
			OutboundBmlStatusMessage "},"
			messageText ":null,"
			inResponseToMessageId ":"
			5 b1da039 - f231 - 42 c0 - b11f - 20563 c11d731 ","
			fromUserDisplayName ":"
			Amelia ","
			secureResponse ":false,"
			bml ":" [
				[\"{\\\"Speech1596047\\\":{\\\"text\\\":\\\"<rate absspeed='-2'> _SyncWord0_ <rate absspeed='0'> Try </rate> again, and _SyncWord3_ <rate absspeed='0'> provide </rate> _SyncWord4_ as <rate absspeed='0'> much </rate> <rate absspeed='0'> detail </rate> as possible. </rate>\\\",\\\"start\\\":\\\"0\\\"},\\\"Gest1596053\\\":{\\\"speed\\\":\\\"1\\\",\\\"name\\\":\\\"WeLf01\\\",\\\"start\\\":\\\"Speech1596047:_SyncWord3_\\\"},\\\"facs1596049\\\":{\\\"amount\\\":\\\"0.5\\\",\\\"au\\\":\\\"61\\\",\\\"attackPeak\\\":\\\"0.5\\\",\\\"name\\\":\\\"Facs\\\",\\\"side\\\":\\\"BOTH\\\",\\\"end\\\":\\\"1\\\",\\\"relax\\\":\\\"0.5\\\",\\\"start\\\":\\\"Speech1596047:_SyncWord0_\\\"},\\\"Head1596054\\\":{\\\"amount\\\":\\\"0.2\\\",\\\"lexeme\\\":\\\"sweep\\\",\\\"repeat\\\":\\\"1\\\",\\\"speed\\\":\\\"1\\\",\\\"name\\\":\\\"HEAD\\\",\\\"start\\\":\\\"Speech1596047:_SyncWord0_\\\"},\\\"faceShift1596056\\\":{\\\"amount\\\":\\\"0.0\\\",\\\"au\\\":\\\"6\\\",\\\"attackPeak\\\":\\\"0.01\\\",\\\"name\\\":\\\"faceShift\\\",\\\"side\\\":\\\"BOTH\\\",\\\"start\\\":\\\"0\\\"},\\\"faceShift1596057\\\":{\\\"amount\\\":\\\"0.0\\\",\\\"au\\\":\\\"12\\\",\\\"attackPeak\\\":\\\"0.01\\\",\\\"name\\\":\\\"faceShift\\\",\\\"side\\\":\\\"BOTH\\\",\\\"start\\\":\\\"0\\\"},\\\"faceLexeme1\\\":{\\\"amount\\\":\\\"0\\\",\\\"attackPeak\\\":\\\"0.01\\\",\\\"lexeme\\\":\\\"SIMPLE_SMILE\\\",\\\"end\\\":\\\"0.03\\\",\\\"relax\\\":\\\"0.02\\\",\\\"start\\\":\\\"0\\\"}}\"]]", "selfEcho": false, "sessionSequence": 34, "inputEnabled": false, "ameliaMessageType": "OutboundBmlStatusMessage", "messageType": "OutboundBmlStatusMessageFromAmelia", "sessionId": "ef490d45-a450-4c29-bb01-bc9dc24b5612", "conversationId": "LIX7HSR7YAIAA-1", "clockSkew": 167, "sourceUserType": "Amelia", "timestamp": 1549406402.307000000
				}, {
					"messageId": "a459e608-f6f0-40ed-8cd8-cd97f6ad3c07",
					"headers": {
						"X-Amelia-Conversation-Id": "LIX7HSR7YAIAA-1",
						"X-Amelia-Self-Echo": "false",
						"X-Amelia-Clock-Skew": "167",
						"X-Amelia-Session-Sequence": "35",
						"X-Amelia-Session-Id": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
						"X-Amelia-Input-Enabled": "false",
						"X-Amelia-Debug-Message": "false",
						"X-Amelia-Message-Id": "a459e608-f6f0-40ed-8cd8-cd97f6ad3c07",
						"X-Amelia-Timestamp": "1549406402156",
						"X-Amelia-Session-Mode": "USER",
						"X-Amelia-Source-User-Type": "Amelia",
						"X-Amelia-Message-Type": "OutboundTextMessage"
					},
					"messageText": "Try again, and provide as much detail as possible.",
					"inResponseToMessageId": "5b1da039-f231-42c0-b11f-20563c11d731",
					"fromUserDisplayName": "Amelia",
					"secureResponse": false,
					"hintText": null,
					"responsePool": null,
					"emoticon": "SIMPLE_SMILE",
					"selfEcho": false,
					"sessionSequence": 35,
					"inputEnabled": false,
					"ameliaMessageType": "OutboundTextMessage",
					"messageType": "TextMessageFromAmelia",
					"sessionId": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
					"conversationId": "LIX7HSR7YAIAA-1",
					"clockSkew": 167,
					"sourceUserType": "Amelia",
					"timestamp": 1549406402.156000000
				}, {
					"messageId": "9248b6a9-d752-4a8e-8f9e-3899715ec0bf",
					"headers": {
						"X-Amelia-Conversation-Id": "LIX7HSR7YAIAA-1",
						"X-Amelia-Self-Echo": "false",
						"X-Amelia-Clock-Skew": "167",
						"X-Amelia-Session-Sequence": "40",
						"X-Amelia-Session-Id": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
						"X-Amelia-Input-Enabled": "true",
						"X-Amelia-Debug-Message": "false",
						"X-Amelia-Message-Id": "9248b6a9-d752-4a8e-8f9e-3899715ec0bf",
						"X-Amelia-Timestamp": "1549406402410",
						"X-Amelia-Session-Mode": "USER",
						"X-Amelia-Source-User-Type": "Amelia",
						"X-Amelia-Message-Type": "OutboundAmeliaReadyMessage"
					},
					"messageText": null,
					"inResponseToMessageId": "5b1da039-f231-42c0-b11f-20563c11d731",
					"fromUserDisplayName": "Amelia",
					"secureResponse": false,
					"selfEcho": false,
					"sessionSequence": 40,
					"inputEnabled": true,
					"ameliaMessageType": "OutboundAmeliaReadyMessage",
					"messageType": "AmeliaReadyMessageFromAmelia",
					"sessionId": "ef490d45-a450-4c29-bb01-bc9dc24b5612",
					"conversationId": "LIX7HSR7YAIAA-1",
					"clockSkew": 167,
					"sourceUserType": "Amelia",
					"timestamp": 1549406402.410000000
				}
```

### Understanding a poll response

| Parameter     | Description   |
| :------------- |:-------------|
| X-Amelia-Conversation-Id          | Denotes a unique conversation with Amelia |
| messageText          | The raw message text returned from Amelia      |
| timeStamp          | The time a message was delivered in epoch time      |

# Domains

## Get all domains

> <font size="4">GET /AmeliaRest/api/v1/admin/domains/</font>

> Example Request:

```python
import amelia

amelia.domain().getAll(url, token)
```

```shell
curl -vk "https://{ameliaUrl}/AmeliaRest/api/v1/admin/domains/"
-H "X-Amelia-Rest-Token: token"
```

> Example Response:

```json
{
	"first": true,
	"last": true,
	"totalElements": 2,
	"totalPages": 1,
	"content": [{
		"id": "18f1df3a-12cd-11e9-a3ac-00505696b1a6",
		"name": "Some domain",
		"code": "some_domain_code",
		"enabled": true
	}, {
		"id": "d969610c-9c37-4864-83ff-0e9a33ee82f1",
		"name": "Some other domain",
		"code": "some_other_domain_code",
		"enabled": true
	}]
}
```

Each id returned under content is a unique UUID to identify that domain.


### Optional URL parameters

| Parameter     | Description   | Type  |
| :------------- |:-------------| :-----|
| name          | The name of the domain | string |
| code          | The code of the domain      |   string |
| search        | Searches both name and code |   string |
| page          | The page result to return   |   int    |
| size          | Number of results to return      |    int |
| sort          | A list of {field}, {dir} directives. Dir can either be asc or desc      |    array[string] |

## Create a Domain

> <font size="4">POST /AmeliaRest/api/v1/admin/domains/</font>

> Example Request:

```python
import amelia
```

```shell
curl here
```

### JSON Parameters

* agentTranslationEnabled = Translation service to translate what an agent says in one language back to the user. Not currently available. Set to false.
* allowAnonymousUsers = Allow non-authenticated users to interact with Amelia. Takes a bool true/false value.
* allowAnonymousView = Allow non-authenticated users to see a domain exists, but not allow them access to that domain. Takes a bool true/false value.
* allowConversations = Allow users to have conversations with Amelia. Takes bool true/false value.
* ameliaUserId = The name you would like to give to Amelia.
* anonymizeTranscriptUser = Strip away user information from transcripts. Takes a bool true/false value.
* anonymousFirstNameOverride = Replaces the first name in transcripts with a string.
* anonymousUserLastNameOverride = Replaces the last name in transcripts with a string.
* authenticationPolicyId = Selects an authentication policy. Use the [get authentication policies endpoint](#get-authentication-policies) to get the authenticationPolicyId.
* avatarVoice = Selects a voice for Amelia. "VW Julie" is the default. See [voice avatars](#voice-avatars) to get a full list of available voices.
* conversationSummaryCleanupDaysRetained = Define how long in days chat transcripts are retained.
* defaultEscalationQueueId = Define to which queue escalations are sent. Use the [get escalation queues endpoint](#get-escalation-queues) to find the queue ID value.
* greetingBpnId = The BPN that first runs when a user enters the domain.
* id = Set this value to null and an ID will be generated.
* imageEnabled = Decide if an image avatar is going to be displayed. Takes a bool true/false value. Only affects mind view Amelia, not custom UI or integrated UI.
* localeTag = Choose the localization for Amelia. Defaults to a variety of English options. Other options available with optional language packs. See [locale options](#locale-options).
* parentId = Name of the parent domain, if one exists.
* preCloseBpnId = The BPN that runs just before a conversation ends.
* preEscalationBpnId = The BPN that runs just before a conversation is escalated.
* subsystemResponderIds = The various [subsystems](#subsystem-responders) to be enabled on a domain.
* timeZoneId = The time zone the domain will operate in.
* transcriptContentMasks = Defines content in transcript that you would like to be masked and replaced.
  * id = Set this value to null and an ID will be generated.
  * match = The string to match on.
  * replace = What to replace the matched string with.
* userTranslationEnabled = Defines if the user's utterances will be translated from other languages. Takes a bool true/false value.
* webGlEnabled = Defines if the 3D Amelia avatar should be enabled. Only affects mind view Amelia, not custom UI or integrated UI. Takes a bool true/false value.
* code = The ID, by which a domain is referred to.
* name = A user friendly reference given to a domain.
* enabled = Defines if a domain is enabled or disabled. Takes a bool true/false value.
* hidden = Defines if a domain is hidden or visible. Takes a bool true/false value.

### Locale Options

* English (Australia)
* English (Canada)
* English (New Zealand)
* English (United Kingdom)
* English (United States)

### Subsystem Responders

Amelia has a variety of subsystems that can be enabled or disabled. Enabling or disabling these subsystems can change how Amelia behaves and responds to users. By default, recommend enabling all subsystems unless otherwise noted.

* AcknowledgeDefault = Last resort responder for declarative statements.
* Affective = Amelia's humanization module. Includes a knowledge base of her responses, nonverbal behaviors (avatar), social talk grammars, and stop words.
* AIML = Artifical Intelligence Modelling Language. XML-based markup language to give Amelia a corpus of information.
* Bpn = If a business process needs to be launched, this subsystem will respond.
* Cqa = Clarifying Question & Answer. Amelia's disambiguation module. If multiple intents are too close in score, Amelia will ask the user to clarify which intent was intended.
* DontKnow = If Amelia is unable to classify an utterance, the DontKnow subsystem will respond.
* EpisodicMemory = Allows Amelia to remember information from past conversations to better follow processes in the future.
* EQA = Generates a qualifying respone if a user gives an incomplete utterance.
* Escalate = Allows Amelia to escalate a conversation if needed.
* FAQ = Provides quick responses to user questions from Semnet.
* IntentFAQ = Similar to the FAQ module, but uses intents instead.
* LogicNet = Gives Amelia the ability to answer questions based on inference. Can be disabled by default.
* Semnet = Provides answers to a conversation out of context. Can be disabled by default.
* SemnetDoc = Provides answers from documents uploaded to Amelia.
* SocialTalk = Similar to AIML, but based on grammars instead of XML.
* UnknownProblem = Responds when the user intent is not recongized by the intent classifier.

### Voice Avatars

Amelia support a variety of languages with the appropriate packs installed. American English (VW Julie) is installed by default. When setting avatarVoice, use one of the following values:

* English (American) = VW Julie
* French = Céline
* French Canadian = Chantal
* Swedish = Astrid (US) or Maja (EU)
* Norwegian = Liv
* Spanish (American) = Penélope
* Spanish (Castilian) = Conchita
* Danish = Naja
* Russian = Tatyana
* Dutch = Lotte
* Italian = Carla
* Turkish = Filiz
* Polish = Maja
* Portuguese = Vitória
* German = Marlene
* Romanian = Carmen
* Icelandic = Dóra
* Welsh = Gwyneth

# Escalation Queues

## Get escalation queues

> <font size="4">GET /AmeliaRest/api/v1/admin/system/escalationqueues</font>

> Example Request:

```python
import amelia
```

```shell
curl -vk "https://{ameliaUrl}/AmeliaRest/api/v1/admin/system/escalationqueues"
-H "X-Amelia-Rest-Token: token"
```

> Example Response:

```json
{
	"first": true,
	"last": true,
	"totalElements": 3,
	"totalPages": 1,
	"content": [{
		"id": "03bf3fe7-667f-47ac-aee4-5ceb92ed8bf4",
		"name": "Test Queue",
		"domainId": "d2d42a2f-c238-4d2f-8ce2-9740b5849724",
		"domainName": "test",
		"enabled": true
	}, {
		"id": "e7f6c769-5b5a-4408-838f-1b25bf7ab9d8",
		"name": "ipsoft queue",
		"domainId": "ad577704-4d40-4c47-85fd-f5f417163c2e",
		"domainName": "ipsoft",
		"enabled": true
	}, {
		"id": "fe3413cd-25e9-4c8b-be04-dd3087e546bd",
		"name": "escalate test",
		"domainId": "c7f87ec6-5f41-4cec-a988-82b86013a440",
		"domainName": "escalate",
		"enabled": true
	}]
}
```

Returns a list of escalation queues available within a given Amelia instance.

### Optional URL parameters

| Parameter     | Description   | Type  |
| :------------- |:-------------| :-----|
| domainId          | The ID of a given domain | string |
| search          | Search for a queue by name      |   string |
| page          | Page of the results to return      |    int |
| size          | Number of results to return      |    int |
| sort          | * sort - A list of {field}, {dir} directives. Dir cna either be asc or desc | array[string] |

# Intents

## Get the intents by domain

> <font size="4">GET /AmeliaRest/api/v1/admin/training/intents/domain/{domainId}/</font>

> Example Request:

```python
import amelia

amelia.intent().getAll(url, token, domainId)
```

```shell
curl -vk "https://{ameliaUrl}/AmeliaRest/api/v1/admin/training/intents/domain/domainId/"
-H "X-Amelia-Rest-Token: token"
```

> Example Response:

```json
{
	"first": true,
	"last": false,
	"totalElements": 2,
	"totalPages": 1,
	"content": [{
		"id": "cc865f2b-b5df-45ce-9add-76b82c141521",
		"name": "some_goal",
		"description": "description_of_some_goal",
		"active": true
	}, {
		"id": "82041084-1c16-4bb4-86de-57b2e5ed9853",
		"name": "some_other_goal",
		"description": "description_of_some_other_goal",
		"active": true
	}]
}
```

The domainId to be passed in is not the domain name, but rather the UUID associated with a specific domain. Use the [domain API](#get-all-domains) to obtain the domainId. The id returned under content is the individual intent id, needed to look up information about that intent.

## Get the details of a single intent

> <font size="4">GET /AmeliaRest/api/v1/admin/training/intents/{intentId}/</font>

> Example Request:

```python
import amelia

amelia.intent().getIntent(url, token, intentId)
```

```shell
curl -vk "https://{ameliaUrl}/AmeliaRest/api/v1/admin/training/intents/intentId/"
-H "X-Amelia-Rest-Token: token"
```

> Example Response:

```json
{
	"id": "03c26b9b-295f-445d-9ba1-e3360213c13d",
	"intentId": "cc865f2b-b5df-45ce-9add-76b82c141521",
	"domainId": "2a563698-bbe4-4eb3-a5f6-08bcea57cb23",
	"name": "some_goal",
	"code": "some_goal_code",
	"description": "description_of_some_goal",
	"keyPhrases": ["identify", "theft", "is", "not", "a", "joke", "jim"],
	"actionPhrase": null,
	"confirmContinueQuestion": "",
	"actionType": "EXECUTE_BPN",
	"actionSystemData": "some_bpn_to_execute",
	"status": "ACTIVE",
	"grammarId": null,
	"onResumeContextConfirmBehavior": "ALWAYS",
	"onSwitchContextAction": "SUSPEND"
}
```

The intentId can be obtained from the [get intents endpoint](#get-the-intents-by-domain).

# Subsystem Responder

## Get subsystem responders

> <font size="4">GET /AmeliaRest/api/v1/admin/subsystemresponders/</font>

> Example Request:

```python
import amelia
```

```shell
curl -vk "https://{ameliaUrl}/AmeliaRest/api/v1/admin/subsystemresponders/"
-H "X-Amelia-Rest-Token: token"
```

> Example Response:

```json
{
	"first": true,
	"last": true,
	"totalElements": 16,
	"totalPages": 1,
	"content": [{
		"id": "43239a8b-e08f-11e7-9ae8-00505696a26e",
		"name": "AcknowledgeDefault",
		"email": null
	}, {
		"id": "4323b2d5-e08f-11e7-9ae8-00505696a26e",
		"name": "Bpn",
		"email": null
	}, {
		"id": "4323cf60-e08f-11e7-9ae8-00505696a26e",
		"name": "DontKnow",
		"email": null
	}, {
		"id": "4323e350-e08f-11e7-9ae8-00505696a26e",
		"name": "Escalate",
		"email": null
	}, {
		"id": "4323f5d3-e08f-11e7-9ae8-00505696a26e",
		"name": "Affective",
		"email": null
	}, {
		"id": "43240947-e08f-11e7-9ae8-00505696a26e",
		"name": "Semnet",
		"email": null
	}, {
		"id": "43241f5a-e08f-11e7-9ae8-00505696a26e",
		"name": "EpisodicMemory",
		"email": null
	}, {
		"id": "43243603-e08f-11e7-9ae8-00505696a26e",
		"name": "FAQ",
		"email": null
	}, {
		"id": "43244ad8-e08f-11e7-9ae8-00505696a26e",
		"name": "SocialTalk",
		"email": null
	}, {
		"id": "43246024-e08f-11e7-9ae8-00505696a26e",
		"name": "LogicNet",
		"email": null
	}, {
		"id": "432473e8-e08f-11e7-9ae8-00505696a26e",
		"name": "AIML",
		"email": null
	}, {
		"id": "432487ad-e08f-11e7-9ae8-00505696a26e",
		"name": "SemnetDoc",
		"email": null
	}, {
		"id": "56dac960-d0c9-11e8-9875-00505696a26e",
		"name": "UnknownProblem",
		"email": null
	}, {
		"id": "a998cf63-7b2b-11e8-9ae8-00505696a26e",
		"name": "EQA",
		"email": null
	}, {
		"id": "a9a4f855-7b2b-11e8-9ae8-00505696a26e",
		"name": "Cqa",
		"email": null
	}, {
		"id": "b1c014b0-1da0-11e8-9ae8-00505696a26e",
		"name": "IntentFAQ",
		"email": null
	}]
}
```

Returns a list of the subsystems in Amelia.

# User

## Get users

> <font size="4">GET /AmeliaRest/api/v1/admin/system/users/</font>

> Example Request:

```python
import amelia

amelia.user().get(url, token, domainId, memberOfGroupId, search, page, size, sort)
```

```shell
curl -vk "https://{ameliaUrl}/AmeliaRest/api/v1/admin/system/users/"
-H "X-Amelia-Rest-Token: token"
```

> Example Response:

```json
{
	"totalPages": 20,
	"totalElements": 193,
	"content": [{
		"email": "michael.scott@dundermifflin.com",
		"primaryDomain": "global",
		"id": "009ed367-2cd0-4805-8492-cf94725d0a78",
		"name": "Michael Scott"
	}, {
		"email": "pam.beasley@dundermifflin.com",
		"primaryDomain": "Exclusive Agency",
		"id": "0126b690-96eb-4dd6-b425-69cc80dc4173",
		"name": "Pam Beasley"
	}, {
		"email": "jim.halpert(dundermifflin.com",
		"primaryDomain": "Unlicensed Service",
		"id": "01b47b08-59cf-4159-b83f-82d200beb003",
		"name": "Jim Halpert"
	}],
	"last": false,
	"first": true
}
```

### Optional URL parameters

| Parameter     | Description   | Type  |
| :------------- |:-------------| :-----|
| domainId          | The domain to search within | string |
| memberOfGroupId   | The group to search within | string |
| search  | Text to search for the user | string |
| page          | Page of the results to return      |    int |
| size          | Number of results to return      |    int |
| sort          | * sort - A list of {field}, {dir} directives. Dir cna either be asc or desc | array[string] |

## Get user details by email

> <font size="4">GET /AmeliaRest/api/v1/admin/subsystemresponders/</font>

> Example Request:

```python
import amelia

amelia.user().getUserByEmail(url, token, emailAddress)
```

```shell
curl -vk "https://{ameliaUrl}/AmeliaRest/api/v1/admin/system/users/byemail/${emailAddress}"
-H "X-Amelia-Rest-Token: token"
```

> Example Response:

```json
{
	"passwordChangeAllowed": false,
	"externalUid": "",
	"domainId": "b5af2a72-6cec-41a6-85c7-a1dd4d894e64",
	"locked": false,
	"firstName": "Michael",
	"locale": "en-US",
	"lastName": "Scott",
	"memberGroups": ["5e266301-4063-11e8-820e-005056961315", "b3e77446-b3fc-4e56-a07e-2ea655ec1f76", "4d19a299-9836-4d6f-ba48-ec006c07cbda", "3bd00e37-6965-4a31-8780-f593d7ee14d2", "f9c2400f-9e61-4cae-abc6-63fd48d413f1"],
	"confirmPassword": null,
	"availability": "AWAY",
	"email": "michael.scott@dundermifflin.com",
	"changePassword": false,
	"authenticationPolicyId": "8330b290-1009-40e4-b354-4f43c761f632",
	"newPassword": null,
	"timeZone": "America/Scranton",
	"maxAgentActiveChats": 0,
	"expired": false,
	"id": "4e7e5cc3-eeaf-416f-984b-7869cbee62da",
	"enabled": true
}
```

Search for a user by their e-mail address.

### URL parameters

| Parameter     | Description   | Type  |
| :------------- |:-------------| :-----|
| emailAddress          | Email address to search for | string |
