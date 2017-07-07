---
title: API Reference

language_tabs:
  - python
  - shell

toc_footers:
  - <a href='http://chui.ai'>chui.ai</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the chui.ai API documentation! You can use our API to detect, match, identify and validate faces.


<aside class="warning">
Remember — this api doesn't accept images larger than 2mbs, resize and/or compress images before sending them.
</aside>

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


> Make sure to replace `chuiai-api-key` with your API key.

Chui.ai uses API keys to allow access. You can register a new API key at our [developer portal](http://chui.ai/).

Chui.ai expects the API key to be included in all requests to the server in a header that looks like the following:

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

The response includes the pixel locations of the bounding boxes for all faces detected in an image, the last value in the bounding box array is the confidence cofficient. The face detect endpoint also returns five main facial landmarks that include left eye, right eye, nose, left mouth corner, and right mouth corner.


## Raw Face Landmarks

The face detection endpoint supports returning 68 face landmarks. To get the raw landmarks, include a "rawlandmark=true" url param to the face detect url.

### Example

`POST https://api.chui.ai/v1/facedetect?rawlandmarks=true`


# Face Recognition
## Enroll

```python
import requests
import base64
import json

headers = {
  "x-api-key":"your_api_key",
  "Content-Type":"application/json",
  
}

url = "https://api.chui.ai/v1/enroll"

data = {
  "img0":base64.b64encode(open('0.jpg','rb').read()),
  "img1":base64.b64encode(open('1.jpg','rb').read()),
  "img2":base64.b64encode(open('2.jpg','rb').read()),
  "img3":base64.b64encode(open('3.jpg','rb').read()),
  "img4":base64.b64encode(open('4.jpg','rb').read()),
  "name":"John Doe"
}

r  = requests.post(url,data=json.dumps(data),headers=headers)

print r.json()
```

```shell
No Curl Example for this endpoint, check python.
```


> The above request returns JSON structured like this:

```json
{
	"message": "enrollment processed successfully",
	"data": {
		"enrollment_id": "ahBzfmNodWlzcGRldGVjdG9ychcLEgpFbnJvbGxtZW50GICAgID5r4YKDA"
	},
	"success": true
}

```

<aside class="warning">
Remember — our apis utilize deep learning a technique that requires a lot of data, we recommend enroll a profile with at least 5 photos.
</aside>

### HTTP Request

`POST https://api.chui.ai/v1/enroll`

This endpoint only accepts a json payload.

To enforce good training, we recommend a minimum of 3 pictures to enroll someone.

include the enrollment pictures as base64 strings as -> img0,img1,img2....img(N) --> (maximum 10 images for the intial enrollment, you can add images to a profile for more training with the update endpoing below)


<aside class="warning">
Remember — this endpoint processes one face at a time.
</aside>


### Collection ID
you can add a profile to a collection by simply including the collection id as "collection_id" in the json payload

### JSON Payload

Make sure to include an application/json Content-Type header when posting a json payload.

Parameter | Required | Description
--------- | ------- | ----------- 
img(0-n) | true | base64 encoded string
name | false | string
collection_id | false | string



### Response
The response includes the profile id which you can use in subsequent matching operations and to update the profile with more training pictures.



## Update Profile

```python
import requests
import base64
import json

headers = {
  "x-api-key":"your_api_key",
  "Content-Type":"application/json",  
}

url = "https://api.chui.ai/v1/enroll"


data = {
  "enrollment_id":"ahJkZXZ-Y2h1aXNwZGV0ZWN0b3JyFwsSCkVucm9sbG1lbnQYgICAgIDglwoM",
  "img0":base64.b64encode(open('3.jpg','rb').read()),
  
}

r  = requests.put(url,data=json.dumps(data),headers=headers)

print r.json()
```

```shell
No Curl Example for this endpoint, check python.
```


> The above request returns JSON structured like this:

```json
{
	"message": "enrollment updated successfully",
	"data": {
		"enrollment_id": "ahJkZXZ-Y2h1aXNwZGV0ZWN0b3JyFwsSCkVucm9sbG1lbnQYgICAgIDglwoM"
	},
	"success": true
}
```


this endpoint allows you to update a profile with more training pictures to improve accuracy.

### HTTP Request

`POST https://api.chui.ai/v1/enroll`

This endpoint only accepts a json payload.


### JSON Payload

Make sure to include an application/json Content-Type header when posting a json payload.

Parameter | Required | Description
--------- | ------- | ----------- 
img(0-n) | true | base64 encoded string
enrollment_id | true | string






## Create a collection

```python
import requests
import base64
import json

headers = {
  "x-api-key":"chui-api-key",
  "Content-Type":"application/json",  
}

url = "https://api.chui.ai/v1/collection"

data = {
  "name":"Home",
}

r  = requests.post(url,data=json.dumps(data),headers=headers)

print r.content
```

```shell
No Curl Example for this endpoint, check python.
```


> The above request returns JSON structured like this:

```json
{
	"data": {
		"collection_id": "ahBzfmNodWlzcGRldGVjdG9ychcLEgpDb2xsZWN0aW9uGICAgICfwYELDA"
	},
	"message": "collection created successfully",
	"success": true
}
```
This endpoint only accepts a json payload.


To preform indentification you need to group profiles in collections. To create a collection simply post name of the collection and save the returned id for later use.


### HTTP Request
`POST https://api.chui.ai/v1/collection`

### JSON Payload

Make sure to include an application/json Content-Type header when posting a json payload.

Parameter | Required | Description
--------- | ------- | ----------- 
name | true | string
unknowns | optional | boolean


### Unknown Class
If unknowns is set to true, a class with dynamic dynamic sample size added to each classifier to help it filter out unknowns and reduce false positives. Can be in conjunction with the confidence rate.

### Response
The response includeds the collection id, make sure you save this id to preform to be able to preform identification on your collection.

## Update a Collection


```python
import requests
import base64
import json

headers = {
  "content-type":"application/json",
  "x-api-key":"chui-api-key"
  
}

url = "https://api.chui.ai/v1/collection"


data = {
  "enrollment_id":"ahBzfmNodWlzcGRldGVjdG9ychcLEgpFbnJvbGxtZW50GICAgIDE25QJDA",
  "collection_id":"ahBzfmNodWlzcGRldGVjdG9ychcLEgpDb2xsZWN0aW9uGICAgICf2p8KDA"
}

r  = requests.put(url,data=json.dumps(data),headers=headers)

print r.json()
```

```shell
No Curl Example for this endpoint, check python.
```


> The above request returns JSON structured like this:

```json
{
	"message": "collection updated successfully",
	"data": {
		"collection_id": "ahBzfmNodWlzcGRldGVjdG9ychcLEgpDb2xsZWN0aW9uGICAgICf2p8KDA"
	},
	"success": true
}
```

To update a collection with an enrollment, make a put request to the collection endpoint with the collection id and the profile you'd like to add.

A classifier is created for each collection and is retrained everytime the collection is updated. Please allow for up to 30 seconds after updating your collection for the collection classifier to reflect the changes.

### JSON Payload

Make sure to include an application/json Content-Type header when posting a json payload.

Parameter | Required | Description
--------- | ------- | ----------- 
enrollment_id | true | string
collection_id | true | string


### HTTP Request
`PUT https://api.chui.ai/v1/collection`


### Response
The response includeds the collection id, make sure you save this id to preform to be able to preform identification on your collection.


## Train a Collection classifier

```python
import requests
import base64
import json

headers = {
  "content-type":"application/json",
  "x-api-key":"chui-api-key"
}

url = "https://api.chui.ai/v1/train"


data = {
  "collection_id":"ahBzfmNodWlzcGRldGVjdG9ychcLEgpDb2xsZWN0aW9uGICAgICf2p8KDA"
}

r  = requests.post(url,data=json.dumps(data),headers=headers)

print r.json()
```

```shell
No Curl Example for this endpoint, check python.
```


> The above request returns JSON structured like this:

```json
{
    "data": {
        "collection_id": "ahBzfmNodWlzcGRldGVjdG9ychcLEgpDb2xsZWN0aW9uGICAgMDMgY4LDA"
    },
    "message": "retraining the classifier",
    "success": true
}
```


After updating a collection you need to trigger a training of the classifier before its ready for use in identify requests.


<aside class="warning">
Remember — you need to trigger a training of the classifier before its ready for use in identify requests.
</aside>

### HTTP Request
`POST https://api.chui.ai/v1/train`

### JSON Payload

Make sure to include an application/json Content-Type header when posting a json payload.

Parameter | Required | Description
--------- | ------- | ----------- 
collection_id_ | true | string

### Response
A success message will indicate the classifier is in training for the collection.




## Face Match

```python
import requests
import base64
import json

headers = {
  "x-api-key":"your_api_key",
  "Content-Type":"application/json",
  
}

url = "https://api.chui.ai/v1/match"

data = {
  "img":base64.b64encode(open('2.jpg','rb').read()),
  "id":"ahBzfmNodWlzcGRldGVjdG9ychcLEgpFbnJvbGxtZW50GICAgID5r4YKDA"
}

r  = requests.post(url,data=json.dumps(data),headers=headers)

print r.json()
```

```shell
No Curl Example for this endpoint, check python.
```


> The above request returns JSON structured like this:

```json
{
	"message": "face processed successfully",
	"data": {
		"emb2_match": true,
		"emb0_score": 0.9071727430549092,
		"emb2_score": 0.8963609236288711,
		"emb1_score": 0.8896829534022603,
		"emb1_match": true,
		"emb0_match": true
	},
	"success": true
}
```

`POST https://api.chui.ai/v1/match`


### HTTP Request

`POST https://api.chui.ai/v1/match`


This endpoint only accepts a json payload.

The face match endpoint is used to match a face to a profile on the system. Send a picture of the face along with id of the person you'd like to match.

The endpoint will attempt to match the generated embedding against every embedding stored for the profile. Return the score and result for every embedding for that profile.

You can supply the threshold to be used. If no threshold is supplied, the api will default to a threshold of 0.5.


<aside class="warning">
Remember — this endpoint processes one face at a time.
</aside>


### JSON Payload

Make sure to include an application/json Content-Type header when posting a json payload.

Parameter | Required | Description
--------- | ------- | ----------- 
img | true | base64 encoded string
id | true | string
threshold | false | float


## Face Identify

```python
import requests
import base64
import json

headers = {
  "x-api-key":"chui-api-key",
  "Content-Type":"application/json",  
}

url = "https://api.chui.ai/v1/identify"


data = {
  "img":base64.b64encode(open('2.jpg','rb').read()),
  "collection_id":"chui-collection-id"
}

r  = requests.post(url,data=json.dumps(data),headers=headers)

print r.json()
```

```shell
No Curl Example for this endpoint, check python.
```


> The above request returns JSON structured like this:

```json
{
	"message": "identify processed successfully",
	"data": {
		"name": "name-if-exists",
		"key": "id-of-identified-person"
	},
	"success": true
}
```

### HTTP Request

`POST https://api.chui.ai/v1/identify`

This endpoint only accepts a json payload.

Identify preforms 1:N classification while also measuring the probablity of an unknown.

To use this endpoint send the image as a base64 encoded string along with the collection id you'd like to compare the image to.


### JSON Payload

Make sure to include an application/json Content-Type header when posting a json payload.

Parameter | Required | Description
--------- | ------- | ----------- 
img | true | base64 encoded string
collection_id | true | string









