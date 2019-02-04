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
curl -vk -X POST "https://amelia_url_here/AmeliaRest/api/v1/auth/login
  -H "Content-Type:application/json"
  -d '{"url":url,"username":username,"password":password}'
```

> Make sure to replace `token` with your API key.

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

# Kittens

## Get All Kittens

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

