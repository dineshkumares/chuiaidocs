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

Make sure to include an image/jpeg or image/png Content-Type header when posting a binary payload.

<aside class="warning">
Remember — always include a Content-Type header in your request
</aside>


### JSON Payload

The API accepts JSON payloads as well, include "img" in the json payload as a base64 encoded string.

Make sure to include an application/json Content-Type header when posting a json payload.

Parameter | Required | Description
--------- | ------- | -----------
img | true | base64 encoded string

<aside class="warning">
Remember — always include a Content-Type header in your request
</aside>



# Face Detection

## Detect faces in an image


```python
import requests

headers = {
  "x-api-key":"chui-api-key",
  "Content-Type":"image/jpeg",
  
}

url = "https://api.chui.ai/v1/facedetect"

r  = requests.post(url,data=open('/Users/nezare/Pictures/test_images/reat_1.jpg','rb').read(),headers=headers)

print r.json()
```

```shell
curl -X POST -H "x-api-key: chui-api-key" -H "Content-Type: image/jpeg" --data-binary "@real_1.jpg" "https://api.chui.ai/v1/facedetect"

```


> The above command returns JSON structured like this:

```json
{
  "data": {
    "faces": [
      {
        "bounding_box": [
          272.7483750283718,
          29.899811007082462,
          335.0228900015354,
          117.26292028464377,
          0.9998049139976501
        ],
        "points": [
          291,
          319,
          306,
          296,
          316,
          65,
          62,
          79,
          97,
          95
        ]
      }
    ]
  },
  "message": "face processed successfully",
  "success": true
}
```

Detect faces in an image

### HTTP Request

`POST https://api.chui.ai/v1/facedetect`

### Binary Payload

Include the binary file in the body of your request.

Make sure to include an image/jpeg or image/png Content-Type header when posting a binary payload.

<aside class="warning">
Remember — always include a Content-Type header in your request
</aside>


### JSON Payload

The API accepts JSON payloads as well, include "img" in the json payload as a base64 encoded string.

Make sure to include an application/json Content-Type header when posting a json payload.

Parameter | Required | Description
--------- | ------- | -----------
img | true | base64 encoded string


### Response

The response includes the pixel locations of the bounding boxes for all faces detected in an image, as well as five facial landmarks that include left eye, right eye, nose, left mouth corner, and right mouth corner.


# Face Recognition
Docs Coming Soon


# Chui Hardware
Docs Coming Soon



