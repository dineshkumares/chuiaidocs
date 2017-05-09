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
curl -X POST -H "x-api-key: chui-api-key" -H "Content-Type: image/jpeg" --data-binary "@fake_1.jpg" "https://api.chui.ai/v1/spdetect"

```


> The above command returns JSON structured like this:

```json
{
  "data": {
    "class": "fake",
    "key": "ahBzfmNodWlzcGRldGVjdG9ychULEghTbmFwc2hvdBiAgICAgtCJCgw",
    "score": 0.57887299999999997,
    "success": true
  },
  "message": "data processed successfully",
  "success": true
}
```

This endpoint checks if an image is a spoof attempt.

### HTTP Request

`POST https://api.chui.ai/v1/spdetect`

### Binary Payload

Include the binary file in the body of your request.


<aside class="warning">
Remember — always include a Content-Type header in your request
</aside>


### JSON Payload

The API accepts JSON payloads as well, include "img" in the json payload as a base64 encoded string.

Parameter | Required | Description
--------- | ------- | -----------
img | true | base64 encoded string

<aside class="warning">
Remember — always include a Content-Type header in your request
</aside>



# Face Detection

Coming Soon

# Face Recognition
Coming Soon


# Chui Hardware



