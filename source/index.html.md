---
title: API Reference

language_tabs:
  - shell
  - python

toc_footers:
  - <a href='http://chui.ai'>chui.ai</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the chui.ai API documentation! You can use our API to access detect, identify and validate faces.



# Authentication

> To authorize, use this code:

```python
import requests

headers = {
  "x-api-key":"chuiai-api-key"
}

r = requests.post(url,headers=headers)
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "x-api-key: chuiai-api-key"
```


> Make sure to replace `meowmeowmeow` with your API key.

Chui.ai uses API keys to allow access to the API. You can register a new API key at our [developer portal](http://chui.ai/).

Chui.ai expects for the API key to be included in all API requests to the server in a header that looks like the following:

`x-api-key: chuiai-api-key`

<aside class="notice">
You must replace <code>chuiai-api-key</code> with your personal API key.
</aside>

# Spoof Detection

## Check if an image is a spoof attempt


```python
import requests

headers = {
  "x-api-key":"chui-api-key",
  "Content-Type":"image/jpeg",
  
}

url = "https://api.chui.ai/v1/spdetect"

r  = requests.post(url,data=open('/Users/nezare/Pictures/test_images/fake_1.jpg','rb').read(),headers=headers)

print r.json()
```

```shell
curl "https://api.chui.ai/v1/spdetect"
  -H "x-api-key: chui-api-key"
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

`POST https://api.chui.ai/v1/spdetect`

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

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
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

`POST http://api.chui.api/process`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve


# Face Detection

Coming Soon

# Face Recognition
Coming Soon


# Chui Hardware



