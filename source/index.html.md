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

### REQUIRED PARAMETERS
* url = The Amelia instance you want to connect to.
* token = The API token issued to you.
* domain = The Anmelia domain where you want to start a conversation, use the domain code.

### OPTIONAL PARAMETERS
* deliveryMode = POLLING or WEBHOOK. Defaults to POLLING. Will deliver messages to a specific URL.
* webhookUrl = Required if WEBHOOK deliveryMode is chosen. URL to deliver messages to.
* secret = If specified, this exact text will be included in all posts to the specified webhookUrl as a header X-Amelia-Webhook-Secret, which you may use to verify that the POST in question is coming from Amelia, and even from a specific conversation.

## Send a message to Amelia

```python
import Amelia
```

```shell
curl -kv --header 'Content-Type: application/json'
-X POST "https://amelia_url_here/AmeliaRest/api/v1/conversations/sessionId/send"
-H 'X-Amelia-Rest-Token: token'
-d '{"messageText":"some message to Amelia here"}'
```

This endpoint sends a message to Amelia for a given conversation.

> The above command, when successful, simply returns a 200 OK message.

## Poll a conversation

```python
import Amelia
```

```shell
curl -kv --header 'Content-Type: application/json'
-X POST "https://amelia_url_here/AmeliaRest/api/v1/conversations/sessionId/poll"
-H 'X-Amelia-Rest-Token: token'
```

This endpoint polls a conversation and returns data about a conversation.

> The above command returns a lot of data about a conversation in JSON.

An example response:

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
			"X-Amel* Connection #0 to host amelia_url_here[{
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
			"X-Amel* Connection #0 to host amelia_url_here left intact
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

### Things to look for in the poll response
* X-Amelia-Conversation-Id = This is the ID denotes a unique conversation with Amelia.
* messageText = The raw message text returned from Amelia.
* timeStamp = The time a message was delivered in epoch time.
