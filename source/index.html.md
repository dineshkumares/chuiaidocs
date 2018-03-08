---
title: API Reference

language_tabs:
  - python
  - shell

toc_footers:
  - <a href='http://trueface.ai'>Trueface.ai</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Trueface.ai API documentation! You can use our API to detect, match, identify and validate faces.



# Authentication

> To authorize, use this code:

```python
import requests

headers = {
  "x-api-key":"your_api_key"
}

r = requests.post(url,headers=headers)
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "x-api-key: Truefaceai-api-key"
```


> Make sure to replace `Truefaceai-api-key` with your API key.

Trueface.ai uses API keys to allow access. You can register a new API key at our [developer portal](https://trueface.ai/).

Trueface.ai expects the API key to be included in all requests to the server in a header that looks like the following:

`x-api-key: your_api_key`

<aside class="notice">
You must replace <code>truefaceai-api-key</code> with your personal API key.
</aside>



# Concepts

##Preforming 1:1 Matching
To perform a 1:1 matching operation (check if any faces in an image match an enrolled profile), you need to:

1. Create s profile with POST /enroll

2. Call POST /match with the id of the enrollment along with the  image you’d like to check in base64 encoded format

The default threshold is 0.4 (scores over 99.2% on the LFW). To increase the matching threshold simply pass a "threshold" param.

##Preforming 1:n Identification

To preform 1:n identification (identify faces in an image), you need to:

1. Create a collection with POST /collection

2. Create profiles with /enroll passing the collection id from above, if the profiles are already enrolled then you can use PUT /collection to update the collection with the enrollment)

3. Call /train with the collection id to train the collection classifier

4. Call /identify with the collection_id and the image you’d like to preform identify on, and you’ll get the names and ids of identified profiles!


# Postman Collection


[Click here](https://chuispdetector.appspot.com/postman_collection) to download a Postman collection with examples covering most endpoints. Use Postman's code generation feature to generate snippets for your favorite language.


# Widgets
## IDVerify Widgets

> Pop up Widget

```html
<html lang="en">

<head>
    <link rel="stylesheet" href="https://trueface.ai/static/widgets/widget.css">
    <script src="https://trueface.ai/static/widgets/widget.js" type="text/javascript"></script>
    <script>
      Trueface.token = "test12345";
    </script>
</head>

<body>
    <button onclick="Trueface.IDVerify.showPopupWidget();return false;">ID Verify</button>
    <script>
      window.addEventListener("id-verify", (data) => {
        console.log("event", data.detail);
      });
    </script>
</body>

</html>

```

Include the header links, set your account id, and use our widgets anywhere in your apps.

Get your widget token and configure it from your settings page or contact support.




Example:
<html class="no-js" lang="en">

<head>
    <link rel="stylesheet" href="https://trueface.ai/static/widgets/widget.css">
    <script src="https://trueface.ai/static/widgets/widget.js" type="text/javascript"></script>
    <script>
      Trueface.token = "test12345";
  </script>
</head>

<body>
</body>
  <button onclick="Trueface.IDVerify.showPopupWidget();return false;">ID Verify</button>
</html>

JSFiddle Inline Example:
[https://jsfiddle.net/yyjf5utk/2/](https://jsfiddle.net/yyjf5utk/2/)

> Inline Widget

```html
<html lang="en">

<head>
    <link rel="stylesheet" href="https://trueface.ai/static/widgets/widget.css">
    <script src="https://trueface.ai/static/widgets/widget.js" type="text/javascript"></script>
    <script>
      Trueface.token = "test12345";
    </script>
</head>

<body>
    <div class="idverify-inline-widget" style="height:100%"></div>
</body>
</html>

```


> Listen for events

```html
<script>
  window.addEventListener("id-verify", (data) => {
    console.log("event", data.detail);
  });
</script>

```



# Trueface.ai JS SDK

[Our Typescript / Javascript SDK](https://github.com/getchui/Trueface.ai_SDK) Implements Trueface's API and simplifies using it in front-end applications

# Trueface.ai React Components

[Open-source components for React](https://github.com/getchui/Trueface.ai_SDK/tree/master/trueface-react) that allow you to quickly build facial recognition UIs for your app.


# Spoof Detection

## Check if an image is a spoof attempt


```python
import requests

headers = {
  "x-api-key":"your_api_key",
  "Content-Type":"image/jpeg",

}

url = "https://api.trueface.ai/v1/spdetect"

r  = requests.post(url,data=open('/Users/nezare/Pictures/test_images/fake_1.jpg','rb').read(),headers=headers)

print r.json()
```

```shell
curl -X POST -H "x-api-key: Truefaceai-api-key" -H "Content-Type: image/jpeg" --data-binary "@fake_1.jpg" "https://api.trueface.ai/v1/spdetect"

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

`POST https://api.trueface.ai/v1/spdetect`

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


# IDVerify

## Verify users against identity documents, digital identities and search sanctions lists.


```python
import requests

headers = {
  "x-api-key":"your_api_key",
  "Content-Type":"application/json",

}

url = "https://api.trueface.ai/v1/idverify"

data = {
  "id_image":base64.b64encode(open('id_image_path','rb').read()),
  "person_image":base64.b64encode(open('person_image_path','rb').read()),
  "email":"person_email"
}

r  = requests.post(url, data=json.dumps(data), headers=headers)

print r.json()
```

```shell
No curl example for this endpoint

```

> The above command returns JSON structured like this:

```json
{"data": {"created": "2017-11-19T23:10:36.701830",
           "id": "ahBzfmNodWlzcGRldGVjdG9ychsLEg5JRFZlcmlmaWNhdGlvbhiAgICQmcjRCww",
           "id_chip": "https://lh3.googleusercontent.com/Zgvo9pu3orowsVqVz-bkmynblwcHwaA-XCpSScvTY1_5LcmvIRPMlqG3IPMiqHBYMES5dcVASXFZaPLlKHvSLvv_7krY",
           "id_landmark_chip": "https://lh3.googleusercontent.com/eqvb4NvQewsRPMj1J8zMz9bznnWREqgbgx5pJ-46xLUn58-iipK4aAdPHTmH1Ig9Artt7ZlGBjJQlo7hn8om71VRAT0",
           "person_chip": "https://lh3.googleusercontent.com/1AKByuRFTfaCRLA7qROCo54ccQcxFh9WUoqZz0A9FvdcrDCLx2bMg31oUhKLjUSGrz9hXL-hQ8NNzjgw9I__mm_tzaje",
           "person_landmark_chip": "https://lh3.googleusercontent.com/Vq3DdNm8En3PfLjwDuDZBLgxQ3HSZIKsFTTchk_LB-yeYnSi_bo89shpb-ErGxMFjD2gno_QSVvc_XSSHgFEgpv0woY",
           "result": True,
           "score": 0.987209570383383,
           "spoof_result": "Real",
           "spoof_score": 0.582509,
           "updated": "2017-11-19T23:10:41.648510"},
 "message": "id processed successfully",
 "success": True}
```


This endpoint verifies users against images in identity documents and digital identities. It extracts, aligns, normalizes, and preforms facial recognition on identity documents and the accompanying person image as well as spoof detection to ensure its really the person that took the image. If the request includes an email and you set web_verify to True, then the operation will attempt to find and retrieve all the public digital identities associated with name and email and present them as well. The sanctions_search will perform a sanctions search on the name included.

### HTTP Request

`POST https://api.trueface.ai/v1/idverify`

### JSON Payload

this endpoints accepts JSON payloads.

Make sure to include an application/json Content-Type header when posting a json payload.

Parameter | Required | Description
--------- | ------- | -----------
id_image | true | base64 encoded string
back_id_image | false | base64 encoded string, required if preforming document verification
person_image | true | base64 encoded string
email | false | base64 encoded string
first_name | false | base64 encoded string
last_name | false | base64 encoded string
web_verify | false | boolean
sanctions_search | false | boolean
document_verify | false | boolean
document_type | false | required if preforming document verification. Use 0 for DL/ID, 1 for Passports, 2 for medical insurance card
document_region | false | required if preforming document verification. Use 0 for United States, 4 for Australia, 5 for Asia, 1 for Canada, 2 for America, 3 for Europe, 7 for Africa, 6 for General Documents

### Response Legend

field | Description 
--------- | ------- 
id | operation id 
id_chip | face chip extracted from id
id_image | id image uploaded
id_landmark_chip | face chip extracted from id with face recognition landmarks applied
person_chip | face chip extracted from person image
person_landmark_chip | face chip extracted from person image with face recognition landmarks applied
result | face recognition match results
score | face recognition match confidence rate
spoof_result | face recognition match results
spoof_score | face recognition match confidence rate
ip_address | ip address of operation
document_verification | json object containing OCR and document verification results


### Image Requirements for Document Verification

If you preform document verification make sure to use a minimum 5 Megapixel camera to capture the images. The ID image should cover 80% of the overall image.Make sure images are not blurry and there is no glare, shadow, or holographic reflection on the images. 

Recommend resolution:
Driver’sLicense /ID 1250 FOR OCR / 2024 For document verification
InsuranceCard 1500
Passport 1478


## Get verifications preformed


```python
import requests

headers = {
  "x-api-key":"your_api_key",
  "Content-Type":"application/json",

}

url = "https://api.trueface.ai/v1/idverify"

r  = requests.get(url, data=json.dumps(data), headers=headers)

print r.json()
```

```shell
No curl example for this endpoint

```

> The above command returns JSON structured like this:

```json
{
    "data": [
        {
            "created": "2017-11-29T22:42:39.013500",
            "email": "nchafni@getchui.com",
            "first_name": "Nezare",
            "id": "ahBzfmNodWlzcGRldGVjdG9ychwLEg9JRFZlcmlmeVJlcXVlc3QYgICAkOKPvQkM",
            "last_name": "Chafni",
            "operation_code": "8T6H5SQP-CHAFNI",
            "owner": "owner_key",
            "preform_doc_verify": false,
            "preform_sanctions_search": true,
            "preform_web_search": true,
            "preformed": true,
            "result": {
                "created": "2017-11-29T22:43:54.183130",
                "email": "nchafni@getchui.com",
                "first_name": null,
                "id": "ahBzfmNodWlzcGRldGVjdG9ychsLEg5JRFZlcmlmaWNhdGlvbhiAgIDQkJzPCAw",
                "id_chip": "https://lh3.googleusercontent.com/WrBrVDTal569pGg9AKQ5_WGFKP8-f8h3HmmpHf49DytKiOA82n06nG5gH34s7ivBNi7jkE1pjgNgSj1C8MjBj4Fx5g",
                "id_landmark_chip": "https://lh3.googleusercontent.com/894nhhgrdhFygcFO-Hfd24oDCx_No3WX4BRWiJWHmjHJPUMMnGYf_Kc1NWRLiceB3ThaNuq3_FSx5bjy2HTSx79U7pA",
                "last_name": null,
                "normalizd_person_chip": null,
                "normalized_id_chip": null,
                "operation_code": null,
                "owner": "owner_key",
                "person_chip": "https://lh3.googleusercontent.com/Xj78mXpeDnKbHVUoGEGGoKCcsZKIv9XWf_Os2QE2jGhr9Phylw-1LjMtDMkPo-ciZT-PQFSajwT8KCOdtUElZ_YflvI",
                "person_landmark_chip": "https://lh3.googleusercontent.com/qh191abj-CPal_oUr5zEBRGUfBw20TGrQjDmfpLgmgrMN614es3Hquq_foMYFIftpbk2mbTWe6-O3G7rlkpcSJsMsv_j",
                "result": true,
                "sanctions_search": [],
                "score": 0.35232741457502237,
                "spoof_result": "Fake",
                "spoof_score": 0.152333,
                "updated": "2017-11-29T22:44:03.230760",
                "web_match": {
                    "aboutme": {
                        "avatar": null,
                        "bio": null,
                        "handle": null
                    },
                    "avatar": "https://d1ts43dypk8bqh.cloudfront.net/v1/avatars/76f72fdc-e152-44ee-8620-fe5d1739ed90",
                    "bio": "Founder at http://getchui.com \n@getchui\nhttp://blog.nezarechafni.com",
                    "email": "nchafni@getchui.com",
                    "emailProvider": false,
                    "employment": {
                        "domain": "trueface.ai",
                        "name": "TrueFace.ai",
                        "role": "engineering",
                        "seniority": "executive",
                        "title": "CTO"
                    },
                    "facebook": {
                        "handle": null
                    },
                    "fuzzy": false,
                    "gender": "male",
                    "geo": {
                        "city": "Las Vegas",
                        "country": "United States",
                        "countryCode": "US",
                        "lat": 36.169941199999997,
                        "lng": -115.1398296,
                        "state": "Nevada",
                        "stateCode": "NV"
                    },
                    "github": {
                        "avatar": "https://avatars3.githubusercontent.com/u/1488519?v=4",
                        "blog": "http://getchui.com",
                        "company": "214 Inc - Chui",
                        "followers": 8,
                        "following": 6,
                        "handle": "nchafni",
                        "id": 1488519
                    },
                    "googleplus": {
                        "handle": null
                    },
                    "gravatar": {
                        "avatar": "https://secure.gravatar.com/avatar/21bf861825f2ae35846fc8aba3baa816",
                        "avatars": [
                            {
                                "type": "thumbnail",
                                "url": "https://secure.gravatar.com/avatar/21bf861825f2ae35846fc8aba3baa816"
                            }
                        ],
                        "handle": "nchafnichui",
                        "urls": []
                    },
                    "id": "76f72fdc-e152-44ee-8620-fe5d1739ed90",
                    "indexedAt": "2017-11-02T14:23:12.755Z",
                    "linkedin": {
                        "handle": "in/nchafni"
                    },
                    "location": "Las Vegas, NV, US",
                    "name": {
                        "familyName": "Chafni",
                        "fullName": "Nezare Chafni",
                        "givenName": "Nezare"
                    },
                    "site": "http://www.nezarechafni.com",
                    "timeZone": "America/Los_Angeles",
                    "twitter": {
                        "avatar": "https://pbs.twimg.com/profile_images/669162769581793280/YyCq1k72.png",
                        "bio": "Founder at http://getchui.com \n@getchui\nhttp://blog.nezarechafni.com",
                        "favorites": 2481,
                        "followers": 824,
                        "following": 363,
                        "handle": "nchafni",
                        "id": 28579932,
                        "location": "Boulder",
                        "site": "http://www.nezarechafni.com",
                        "statuses": 487
                    },
                    "utcOffset": -8
                }
            },
            "sanctions_search": [],
            "updated": "2017-11-29T22:44:03.485150",
            "web_results": {
              
            }
        } ],
    "message": null,
    "success": true
}
```


### HTTP Request

`GET https://api.trueface.ai/v1/idverify`


To query a single operation simply include the operation id as a url parma ?id=operation_id


### Response
View the python Tab for an example of this endpoints response.





# IDVerify Request

## Generate and Send ID Verification Requests.


```python
import requests

headers = {
  "x-api-key":"your_api_key",
  "Content-Type":"application/json",

}

url = "https://api.trueface.ai/v1/request-idverify"

data = {
  "first_name":"Nezare",
  "last_name":"Chafni",
  "email":"demo@trueface.ai"
}

r  = requests.post(url, data=json.dumps(data), headers=headers)

print r.json()
```

```shell
No curl example for this endpoint

```

> The above command returns JSON structured like this:

```json
{
    "data": {
        "created": "2017-11-20T17:49:53.942260",
        "email": "demo@trueface.ai",
        "first_name": "Nezare",
        "id": "ahBzfmNodWlzcGRldGVjdG9ychwLEg9JRFZlcmlmeVJlcXVlc3QYgICAkOO4_QsM",
        "last_name": "Chafni",
        "owner": "owner_key",
        "preformed": false,
        "result": null,
        "sanctions_search": null,
        "updated": "2017-11-20T17:49:54.123900",
        "web_results": null
    },
    "message": null,
    "success": true
}
```



This endpoint generates a verification request. If you include an email the target will recieve an email and operation code requesting the verification:

Example: 
![alt text][logo]

[logo]: https://i.imgur.com/3jlYJAv.png "Example of email sent"



### HTTP Request

`POST https://api.trueface.ai/v1/request-idverify`

### JSON Payload

this endpoints accepts JSON payloads.

Make sure to include an application/json Content-Type header when posting a json payload.

Parameter | Required | Description
--------- | ------- | -----------
first_name | true | string
last_name | true | string
email | false | string


### Response 
View the python Tab for an example of this endpoints response.


## Get Generated IDVerify Requests


```python
import requests

headers = {
  "x-api-key":"your_api_key",
  "Content-Type":"application/json",

}

url = "https://api.trueface.ai/v1/request-idverify"

r  = requests.get(url, data=json.dumps(data), headers=headers)

print r.json()
```

```shell
No curl example for this endpoint

```

> The above command returns JSON structured like this:

```json
{
    "data": [
         {
            "created": "2017-11-29T22:42:39.013500",
            "email": "nchafni@getchui.com",
            "first_name": "Nezare",
            "id": "ahBzfmNodWlzcGRldGVjdG9ychwLEg9JRFZlcmlmeVJlcXVlc3QYgICAkOKPvQkM",
            "last_name": "Chafni",
            "operation_code": "8T6H5SQP-CHAFNI",
            "owner": "owner_key",
            "preform_doc_verify": false,
            "preform_sanctions_search": true,
            "preform_web_search": true,
            "preformed": true,
            "result": {
                "created": "2017-11-29T22:43:54.183130",
                "email": "nchafni@getchui.com",
                "first_name": null,
                "id": "ahBzfmNodWlzcGRldGVjdG9ychsLEg5JRFZlcmlmaWNhdGlvbhiAgIDQkJzPCAw",
                "id_chip": "https://lh3.googleusercontent.com/WrBrVDTal569pGg9AKQ5_WGFKP8-f8h3HmmpHf49DytKiOA82n06nG5gH34s7ivBNi7jkE1pjgNgSj1C8MjBj4Fx5g",
                "id_landmark_chip": "https://lh3.googleusercontent.com/894nhhgrdhFygcFO-Hfd24oDCx_No3WX4BRWiJWHmjHJPUMMnGYf_Kc1NWRLiceB3ThaNuq3_FSx5bjy2HTSx79U7pA",
                "last_name": null,
                "normalizd_person_chip": null,
                "normalized_id_chip": null,
                "operation_code": null,
                "owner": "owner_key",
                "person_chip": "https://lh3.googleusercontent.com/Xj78mXpeDnKbHVUoGEGGoKCcsZKIv9XWf_Os2QE2jGhr9Phylw-1LjMtDMkPo-ciZT-PQFSajwT8KCOdtUElZ_YflvI",
                "person_landmark_chip": "https://lh3.googleusercontent.com/qh191abj-CPal_oUr5zEBRGUfBw20TGrQjDmfpLgmgrMN614es3Hquq_foMYFIftpbk2mbTWe6-O3G7rlkpcSJsMsv_j",
                "result": true,
                "sanctions_search": [],
                "score": 0.35232741457502237,
                "spoof_result": "Fake",
                "spoof_score": 0.152333,
                "updated": "2017-11-29T22:44:03.230760",
                "web_match": {
                    "aboutme": {
                        "avatar": null,
                        "bio": null,
                        "handle": null
                    },
                    "avatar": "https://d1ts43dypk8bqh.cloudfront.net/v1/avatars/76f72fdc-e152-44ee-8620-fe5d1739ed90",
                    "bio": "Founder at http://getchui.com \n@getchui\nhttp://blog.nezarechafni.com",
                    "email": "nchafni@getchui.com",
                    "emailProvider": false,
                    "employment": {
                        "domain": "trueface.ai",
                        "name": "TrueFace.ai",
                        "role": "engineering",
                        "seniority": "executive",
                        "title": "CTO"
                    },
                    "facebook": {
                        "handle": null
                    },
                    "fuzzy": false,
                    "gender": "male",
                    "geo": {
                        "city": "Las Vegas",
                        "country": "United States",
                        "countryCode": "US",
                        "lat": 36.169941199999997,
                        "lng": -115.1398296,
                        "state": "CALIFORNIA",
                        "stateCode": "CA"
                    },
                    "github": {
                        "avatar": "https://avatars3.githubusercontent.com/u/1488519?v=4",
                        "blog": "http://getchui.com",
                        "company": "214 Inc - Chui",
                        "followers": 8,
                        "following": 6,
                        "handle": "nchafni",
                        "id": 1488519
                    },
                    "googleplus": {
                        "handle": null
                    },
                    "gravatar": {
                        "avatar": "https://secure.gravatar.com/avatar/21bf861825f2ae35846fc8aba3baa816",
                        "avatars": [
                            {
                                "type": "thumbnail",
                                "url": "https://secure.gravatar.com/avatar/21bf861825f2ae35846fc8aba3baa816"
                            }
                        ],
                        "handle": "nchafnichui",
                        "urls": []
                    },
                    "id": "76f72fdc-e152-44ee-8620-fe5d1739ed90",
                    "indexedAt": "2017-11-02T14:23:12.755Z",
                    "linkedin": {
                        "handle": "in/nchafni"
                    },
                    "location": "San Francisco, CA, US",
                    "name": {
                        "familyName": "Chafni",
                        "fullName": "Nezare Chafni",
                        "givenName": "Nezare"
                    },
                    "site": "http://www.nezarechafni.com",
                    "timeZone": "America/Los_Angeles",
                    "twitter": {
                        "avatar": "https://pbs.twimg.com/profile_images/669162769581793280/YyCq1k72.png",
                        "bio": "Founder at http://getchui.com \n@getchui\nhttp://blog.nezarechafni.com",
                        "favorites": 2481,
                        "followers": 824,
                        "following": 363,
                        "handle": "nchafni",
                        "id": 28579932,
                        "location": "Boulder",
                        "site": "http://www.nezarechafni.com",
                        "statuses": 487
                    },
                    "utcOffset": -8
                }
            },
            "sanctions_search": [],
            "updated": "2017-11-29T22:44:03.485150",
            "web_results": {}
        },
    ],
    "message": null,
    "success": true
}
```



This endpoint allows you to retrieve the verification requests generated



### HTTP Request

`GET https://api.trueface.ai/v1/request-idverify`


### Response 
View the python Tab for an example of this endpoints response.


# Face Detection

## Detect faces in an image


```python
import requests

headers = {
  "x-api-key":"Truefaceai-api-key",
  "Content-Type":"image/jpeg",

}

url = "https://api.trueface.ai/v1/facedetect"

r  = requests.post(url,data=open('/Users/nezare/Pictures/test_images/reat_1.jpg','rb').read(),headers=headers)

print r.json()
```

```shell
curl -X POST -H "x-api-key: chui-api-key" -H "Content-Type: image/jpeg" --data-binary "@real_1.jpg" "https://api.trueface.ai/v1/facedetect"

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

`POST https://api.trueface.ai/v1/facedetect`

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

`POST https://api.trueface.ai/v1/facedetect?rawlandmarks=true`


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

url = "https://api.trueface.ai/v1/enroll"

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
		"enrollment_id": "your_enrollment_id"
	},
	"success": true
}

```

### HTTP Request

`POST https://api.trueface.ai/v1/enroll`

This endpoint only accepts a json payload.

Include the enrollment pictures as base64 strings as -> img0,img1,img2....img(N) --> (maximum 10 images for the intial enrollment, you can add images to a profile for more training with the update endpoing below)


<aside class="warning">
Remember — don't include different faces when enrolling someone
</aside>


### Collection ID
You can add a profile to a collection by simply including the collection id as "collection_id" in the json payload

### JSON Payload

Make sure to include an application/json Content-Type header when posting a json payload.

Parameter | Required | Description
--------- | ------- | -----------
img(0-n) | true | base64 encoded string
name | false | string
collection_id | false | string



### Response
The response includes the profile id which you can use in subsequent matching operations and to update the profile with more training pictures.



## Update Enrollment

```python
import requests
import base64
import json

headers = {
  "x-api-key":"your_api_key",
  "Content-Type":"application/json",  
}

url = "https://api.trueface.ai/v1/enroll"


data = {
  "enrollment_id":"your_enrollment_id",
  "img0":base64.b64encode(open('3.jpg','rb').read())
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
		"enrollment_id": "your_enrollment_id"
	},
	"success": true
}
```


This endpoint allows you to update a profile with more training pictures to improve accuracy.

### HTTP Request

`PUT https://api.trueface.ai/v1/enroll`

This endpoint only accepts a json payload.


### JSON Payload

Make sure to include an application/json Content-Type header when posting a json payload.

Parameter | Required | Description
--------- | ------- | -----------
img(0-n) | true | base64 encoded string
enrollment_id | true | string


## Get a list of your Enrollments


```python
import requests
import base64
import json

headers = {
  "content-type":"application/json",
  "x-api-key":"Truefaceai-api-key"

}

url = "https://api.trueface.ai/v1/enroll"


r  = requests.get(url,headers=headers)

print r.json()
```

```shell
No Curl Example for this endpoint, check python.
```


> The above request returns JSON structured like this:

```json
{
    "data": [
        {
            "collection_id": null,
            "created": "2017-10-25T18:41:00.190750",
            "embeddings": [
                "ahBzfmNodWlzcGRldGVjdG9ychcLEgpFbWJlZGRpbmdzGICAgJDonIsJDA"
            ],
            "id": "ahBzfmNodWlzcGRldGVjdG9ychcLEgpFbnJvbGxtZW50GICAgJDoq_cIDA",
            "images": [],
            "name": "Lena",
            "owner": "owner_key",
            "updated": "2017-10-25T18:41:00.370150"
        }
          ]
}
```

To get a list of your enrollments, simply make a GET request to /enroll


### HTTP Request
`GET https://api.trueface.ai/v1/enroll`


### Response
The response includes a list of the embeddings that belong to the enrollment.

## Delete Enrollment

```python
import requests
import base64
import json

headers = {
  "x-api-key":"your_api_key",
  "Content-Type":"application/json",  
}

url = "https://api.trueface.ai/v1/enroll"


data = {
  "enrollment_id":"your_enrollment_id"
}

r  = requests.delete(url,data=json.dumps(data),headers=headers)

print r.json()
```

```shell
No Curl Example for this endpoint, check python.
```


> The above request returns JSON structured like this:

```json
{
    "data": "Enrollment *your_enrollment_id* has been deleted",
    "message": null,
    "success": true
}
```


This endpoint allows you to delete an enrolled profile.

### HTTP Request

`DELETE https://api.trueface.ai/v1/enroll`

This endpoint only accepts a json payload.


### JSON Payload

Make sure to include an application/json Content-Type header when posting a json payload.

Parameter | Required | Description
--------- | ------- | -----------
enrollment_id | true | string

### Response
The response looks as follows

## Create a Collection

```python
import requests
import base64
import json

headers = {
  "x-api-key":"chui-api-key",
  "Content-Type":"application/json",  
}

url = "https://api.trueface.ai/v1/collection"

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
		"collection_id": "your_collection_id"
	},
	"message": "collection created successfully",
	"success": true
}
```
This endpoint only accepts a json payload.


To perform indentification you need to group profiles in collections. To create a collection simply post the name of the collection and save the returned id for later use.


### HTTP Request
`POST https://api.trueface.ai/v1/collection`

### JSON Payload

Make sure to include an application/json Content-Type header when posting a json payload.

Parameter | Required | Description
--------- | ------- | -----------
name | true | string
unknowns | optional | boolean


### Unknown Class
If unknowns is set to true, a class with dynamic dynamic sample size added to each classifier to help it filter out unknowns and reduce false positives. Can be in conjunction with the confidence rate.

### Response
The response includes the collection id, make sure you save this id to be able to perform identification on your collection.

## Update a Collection


```python
import requests
import base64
import json

headers = {
  "content-type":"application/json",
  "x-api-key":"chui-api-key"
}

url = "https://api.trueface.ai/v1/collection"


data = {
  "enrollment_id":"your_enrollment_id",
  "collection_id":"your_collection_id"
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
		"collection_id": "your_collection_id"
	},
	"success": true
}
```

To update a collection with an enrollment, make a put request to the collection endpoint with the collection id and the profile you'd like to add.

A classifier is created for each collection and is retrained every time the collection is updated. Please allow for up to 30 seconds after updating your collection for the collection classifier to reflect the changes.

### JSON Payload

Make sure to include an application/json Content-Type header when posting a json payload.

Parameter | Required | Description
--------- | ------- | -----------
enrollment_id | true | string
collection_id | true | string


### HTTP Request
`PUT https://api.trueface.ai/v1/collection`


### Response
The response includes the collection id, make sure you save this id to be able to perform identification on your collection.

## Delete a Collection

```python
import requests
import base64
import json

headers = {
  "x-api-key":"chui-api-key",
  "Content-Type":"application/json",  
}

url = "https://api.trueface.ai/v1/collection"

data = {
  "collection_id":"your_collection_id",
}

r  = requests.delete(url,data=json.dumps(data),headers=headers)

print r.content
```

```shell
No Curl Example for this endpoint, check python.
```


> The above request returns JSON structured like this:

```json
{
    "data": "Entity *your_collection_id* has been deleted",
    "message": null,
    "success": true
}
```
This endpoint only accepts a json payload.


To delete a collection, just send a collection_id to the delete endpoint.


### HTTP Request
`DELETE https://api.trueface.ai/v1/collection`

### JSON Payload

Make sure to include an application/json Content-Type header when posting a json payload.

Parameter | Required | Description
--------- | ------- | -----------
collection_id | true | string


### Response
The response looks as follows


## Train a Collection Classifier

```python
import requests
import base64
import json

headers = {
  "content-type":"application/json",
  "x-api-key":"Truefaceai-api-key"
}

url = "https://api.trueface.ai/v1/train"


data = {
  "collection_id":"your_collection_id"
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
        "collection_id": "your_collection_id"
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
`POST https://api.trueface.ai/v1/train`

### JSON Payload

Make sure to include an application/json Content-Type header when posting a json payload.

Parameter | Required | Description
--------- | ------- | -----------
collection_id | true | string

### Response
A success message will indicate the classifier is in training for the collection.



## Get a list of your Collections


```python
import requests
import base64
import json

headers = {
  "content-type":"application/json",
  "x-api-key":"Truefaceai-api-key"

}

url = "https://api.trueface.ai/v1/collection"


r  = requests.get(url,headers=headers)

print r.json()
```

```shell
No Curl Example for this endpoint, check python.
```


> The above request returns JSON structured like this:

```json
{
    "data": [
              {
                          "classifier_url": "",
                          "created": "2017-08-19T00:13:24.971650",
                          "enrollments": [
                              "ahBzfmNodWlzcGRldGVjdG9ychcLEgpFbnJvbGxtZW50GICAgKDI0LMKDA",
                              "ahBzfmNodWlzcGRldGVjdG9ychcLEgpFbnJvbGxtZW50GICAgMDH6KYJDA"
                          ],
                          "id": "ahBzfmNodWlzcGRldGVjdG9ychcLEgpDb2xsZWN0aW9uGICAgMCluLELDA",
                          "label_encoder": "",
                          "name": "Home",
                          "owner": "",
                          "unknown": true,
                          "updated": "2017-08-20T00:58:16.616790"
              }
            ]
}
```

To get a list of your collections, simply make a GET request to /collection


### HTTP Request
`GET https://api.trueface.ai/v1/collection`


### Response
The response includeds the enrollment ids in the collection.




## Face Match

```python
import requests
import base64
import json

headers = {
  "x-api-key":"your_api_key",
  "Content-Type":"application/json",

}

url = "https://api.trueface.ai/v1/match"

data = {
  "img":base64.b64encode(open('2.jpg','rb').read()),
  "id":"your_id"
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

`POST https://api.trueface.ai/v1/match`


### HTTP Request

`POST https://api.trueface.ai/v1/match`


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

url = "https://api.trueface.ai/v1/identify"


data = {
  "img":base64.b64encode(open('2.jpg','rb').read()),
  "collection_id":"your_collection_id"
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

`POST https://api.trueface.ai/v1/identify`

This endpoint only accepts a json payload.

Identify preforms 1:N classification while also measuring the probablity of an unknown.

To use this endpoint send the image as a base64 encoded string along with the collection id you'd like to compare the image to.


### JSON Payload

Make sure to include an application/json Content-Type header when posting a json payload.

Parameter | Required | Description
--------- | ------- | -----------
img | true | base64 encoded string
collection_id | true | string
