---
title: PDL Mobile API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='http://github.com/tripit/slate'>Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to API documentation for PDL Mobile.

# Global Headers

> Example:

```shell
# With shell, you can just pass the correct header with each request
curl 'api_endpoint' -H 'OS: IOS' -H 'Accept-Language: vi'

# For requests with required authentication and wrong token 401 will be returned
curl -X GET -H 'Auth-Token: aa380ba1669a57948dcfbd686ed2aa2' 'https://api.mfi.vn/api/mobile/client' -k -I
HTTP/1.0 401 UNAUTHORIZED
Content-Type: text/html
Content-Length: 339
Server: Werkzeug/0.10.4 Python/2.7.6
Date: Wed, 04 Nov 2015 13:28:41 GMT
```

> if request has errors in its sent data, special dictionary will be returned with keys corresponding to the data ones, e.g:

```json
{
    "errors": {
        "version": "version is missing",
        "os_id": "Header OS is missing"
    }
}
```

> 'common' key is used if error does not define any other particular key in request, like this:

```json
{
    "errors": {
        "common": "Pin is invalid"
    }
}
```

Header | Required | Values | Description
-------|----------|--------|------------
OS | required | IOS, ANDROID, WINPHONE | phone OS type
Accept-Language | required | vi, en, ru | locale

For POST requests one extra header should exist:

Header | Required | Value
-------|----------|--------
Content-Type | required | application/json

For requests with required authentication another header should exist:

Header | Required | Value
-------|----------|--------
Auth-Token | required | 'token'

### Errors in response

If for the endpoint required query arguments and/or headers are missing then error response will be returned.

<aside class="notice">
Not all endpoints have data available in 'ru' and 'en' locales.
</aside>

# Mobile Version

## Get Current Version

> Examples:

```shell
curl -X GET -H 'OS: IOS' 'https://api.mfi.vn/api/mobile/version?version=169' -k
```

> Return data structure:

```json
{
    "need_update": true,
    "version": 144
}
```

### HTTP Request

`GET https://api.mfi.vn/api/mobile/version`

### Query Parameters

Parameter | Required | Description
--------- | -------- | -----------
version   | required | version of the mobile application

### Return parameters

Parameter | Type | Description
--------- | ---- | -----------
need_update | boolean | defines whether mobile version needs update or not
version | integer | current version of the mobile application 

### Test data

OS type | version
-------- | -------
IOS | 144
IOS | 169
ANDROID | 9801
ANDROID | 10000
WINPHONE | 188356
WINPHONE | 189225

# Calculator

## Get calculator data

> Example:

```shell
curl -X GET -H 'Accept-Language: vi' 'https://api.mfi.vn/api/mobile/calculator' -k
```

> Response JSON Structure:

```json
{
  "currency": "VND",
  "mappings": [
    {
      "terms": [
        {
          "term": 7,
          "is_available": true,
          "fx_service_fee": 17500000,
          "repay_until": 1447102800
        },
        {
          "term": 15,
          "is_available": true,
          "fx_service_fee": 37500000,
          "repay_until": 1447794000
        },
        {
          "term": 21,
          "is_available": true,
          "fx_service_fee": 52500000,
          "repay_until": 1448312400
        },
        {
          "term": 30,
          "is_available": true,
          "fx_service_fee": 75000000,
          "repay_until": 1449090000
        }
      ],
      "fx_amount": 250000000
    },
    ...
  ]
}
```

This endpoint retrives data for credit calculator.

### HTTP Request

`GET https://api.mfi.com/api/mobile/calculator`

### Query Parameters

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
client_id | not required | integer | If set, the result will contain calculator data for client with particular id otherwise return calculator data for non authorized users.
lastupdatetime | true | integer, unixtime | If set to particular timestamp and no newer data on the server were found then response will be empty ({}) otherwise returs calculator data.

### Return parameters

Main response object:

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
currency  | required | string | currency value
mappings  | required | array  | array of amount data


Amount data object:

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
fx_amount | required | integer | amount of credit money
terms     | required | array   | array of term data

Term data object:

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
term      | required | integer | number of days for credit
is_available | required | boolean | defines if current amount + term available to choose in calculator
fx_service_fee | required | integer | service fee
fx_service_fee_special | not required | integer | promotional service fee
repay_until | required | integer, unixtime | repay deadline date

# Faq

## Get faq data

> Example:

```shell
curl -X GET -H 'Accept-Language: vi' 'https://api.mfi.vn/api/mobile/faq' -k
```

> Response JSON Structure:

```json
[
  {
    "category": "Sản phẩm/ Dịch vụ",
    "questions": [
      {
        "header": "Lãi suất, phí là bao nhiêu?",
        "list": [
          "Hotline: 1900 63 60 72",
          "E-mail: hotro@micro-money.vn"
        ],
        "text": "Tại thời điểm hiện tại, vui lòng tham khảo bảng tính lãi và phí dịch vụ sau. Mức lãi suất và phí dịch vụ sẽ được Chúng tôi công bố theo từng thời kỳ và sẽ được ghi rõ trong Hợp đồng. Để biết thêm thông tin, vui lòng liên hệ Bộ phận Dịch Vụ Khách Hàng của chúng tôi để được giải đáp: "
      },
      ...
    ]
  },
  ...
]
```

This endpoint retrives data for FAQ.

### HTTP Request

`GET https://api.mfi.com/api/mobile/faq`

### Query Parameters

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
lastupdatetime | true | integer, unixtime | If set to particular timestamp and no newer data on the server were found then response will be empty ({}) otherwise returs current data.

### Return parameters

Returns array of category object.

Category object:

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
category  | required | string | name of category
questions  | required | array  | array of questions


Question object:

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
header | required | string | question
text   | required | string | anwser
list   | required | array of strings | additional list for answer

### Test data

There are test data for en, ru and vi languages.

# Questionnaire Form

## Get Questionnaire Form

> Example:

```shell
curl -X GET -H 'Accept-Language: vi' 'https://api.mfi.vn/api/mobile/forms/questionnaire?lastupdatetime=14464614' -k
```

> Response JSON Structure:

```json
{
  "guid": "V4EBHAF4K66OKIC",
  "steps": [
    {
      "step": 1,
      "name": "Personal Information",
      "fields": [
        {
          "placeholder": "Choose",
          "error_message": "Field is required",
          "field_id": "status",
          "label": "Job",
          "is_required": true,
          "type": "select",
          "options": [
            {
              "name": "Worker",
              "value": "10",
              "order": 1
            },
            {
              "name": "Office worker",
              "value": "8",
              "order": 2
            }
          ]
        },
        {
          "placeholder": "Nguyễn Thu Giang",
          "error_message": "Field is required",
          "field_id": "full_name",
          "label": "Full name",
          "regexp": "/^([a-z0-9A-Z]{2,}[\s]*$/",
          "is_required": true,
          "type": "text"
        },
        {
          "placeholder": "0123 456 78 90",
          "error_message": "Field is required",
          "field_id": "mobile",
          "label": "Mobile phone",
          "regexp": "/^[\d]{4}[\s][\d]{3}[\s][\d]{2}[\s][\d]{1,2}$/",
          "is_required": true,
          "type": "phone"
        },
        {
          "placeholder": "20.01.1990",
          "error_message": "Field is required",
          "field_id": "birth",
          "label": "Birthday",
          "regexp": "/^[\d]{2}[.][\d]{2}[.][\d]{4}$/",
          "is_required": true,
          "type": "date"
        },
        {
          "placeholder": "thugiang@mail.com",
          "error_message": "Field is required",
          "field_id": "email",
          "label": "E-mail",
          "is_required": false,
          "type": "email"
        }
      ]
    },
    ...
  ]
}
```

Retrives data for credit application.

### HTTP Request

`GET https://api.mfi.com/api/mobile/forms/questionnaire`

### Query Parameters

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
lastupdatetime | true | integer, unixtime | If set to particular timestamp and no newer data on the server were found then response will be empty ({}) otherwise returs applicaiton data.
client_id (not implemented yet) | not required | integer | If set, the result will contain data for client with particular id otherwise return data for non authorized users.

<aside class="warning">
Please notice that support for client_id hasn't implemented yet!
</aside>

### Return parameters

Main response object:

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
guid      | required | string | identifier string for current application
steps     | required | array | array of steps data

Step object:

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
step      | required | integer | number of the current application step
name      | required | string  | name of the current application step
fields    | required | array   | array of field data

Field data object:

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
field_id  | required | string | unique name for the field
type      | required | string | field type (see [Types section](/#questionnaire-types-list) for available types)
value     | not required | string | field value
label     | not required | string | field label
placeholder | not required | string | field placeholder
error_message | not required | string | error message when value is invalid for the field
hint      | not required | string | hint for the field
is_required | required | boolean | field required flag
max_limit | not required | integer | max symbols count in value
min_limit | not required | integer | min symbols count in value
options   | not required (required only if field type is 'select' or 'radio') | array | available values for the current field ()

If field type is 'select' or 'radio', additional key 'options' will be available in field data.

Option data object:

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
field_id  | required | string | unique name of the option
value     | required | string | value of the option

### Questionnaire Types List

Parameter  | Description
---------- | -----------
text       | text input
text_multi | multiline text
text_static | static text
numeric    | integer number
checkbox   | checkbox
select     | select input
radio      | radio box
password   | password input
date       | date input
photo_front| image file
photo_back | image file
email      | email input
phone_primary | phone input
phone      | phone input

## Send Verification Code

> Example:

```shell
curl -X POST -H 'Accept-Language: vi' -H 'Content-Type: application/json' 'https://api.mfi.vn/api/mobile/forms/questionnaire/send-code' -k -d '{"guid": "V4EBHAF4K66OKIC", "phone": "84555111134"}'
```

> Response JSON Structure:

```json
{
  "success": true
}
```

Sends verification code on client's phone. 

### HTTP Request

`POST https://api.mfi.com/api/mobile/forms/questionnaire/send-code`

### JSON Parameters

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
guid      | required | string | application identifier
phone      | required | string | client's phone number

### Return parameters

Main response object:

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
success   | required | boolean | defines whether code sent or not

## Check Verification Code

> Example:

```shell
curl -X POST -H 'Accept-Language: vi' -H 'Content-Type: application/json' 'https://api.mfi.vn/api/mobile/forms/questionnaire/check-code' -k -d '{"guid": "V4EBHAF4K66OKIC", "code": 1234}'
```

> Response JSON Structure:

```json
{
  "success": true
}
```

Checks verification code for particular application. There are 5 attempts to match code correctly, otherwise resend code request is needed.

### HTTP Request

`POST https://api.mfi.com/api/mobile/forms/questionnaire/check-code`

### JSON Parameters

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
guid      | required | string | application identifier
code      | required | integer | code clients received on his phone

<aside class="success">
There is a special code 111111 that works on test server and will pass sms verification.
</aside>

### Return parameters

Main response object:

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
success   | required | boolean | defines whether code is correct or not

### Common errors:

Error | Description
---- | -----------
Code is invalid | Code doesn't match with the one that was set before with [this endpoint](/#send-verification-code).

## Post Questionnaire Form

> Example:

```shell
curl -X POST -H 'Accept-Language: vi' 'https://api.mfi.vn/api/mobile/forms/questionnaire' -k 
-d '{"guid": "V4EBHAF4K66OKIC", "step": 1, "calc_sum": 100000000, "calc_days": 7,  "status": 10, "full_name": "John Doe", ...}'
```

> Response JSON Structure:

```json
{
  "success": true
}
```

Sends data for new credit application. Information sends as JSON data with keys as corresponding field_id,
which can be accessed in [get questionnaire endpoint](/#get-questionnaire-form).

### HTTP Request

`POST https://api.mfi.com/api/mobile/forms/questionnaire`

### JSON Parameters

Main JSON data corresponds with data which client got using [get questionnaire endpoint](/#get-questionnaire-form) for current step.
Also additional keys parameters needed:

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
guid      | required | string | application identifier
step      | required | integer | step of the data
client_id (not implemented yet) | not required | integer | If set, the result will contain data for client with particular id otherwise return data for non authorized users.
calc_sum  | required | integer | credit days
calc_days | required | integer | credit sum
fb_user_id | not required | string | user facebook id
fb_token | not required | string | user token

<aside class="warning">
Please notice that support for client_id hasn't implemented yet!
</aside>

### Return parameters

Main response object:

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
success   | required | boolean | defines whether application accepted by server or not

## Upload Questionnaire Photo

> Example:

```shell
curl -F "guid=V4E9876SD987G" -F "photo_front=@/path/to/file" 'https://api.mfi.vn/api/mobile/forms/questionnaire/upload-photo'
```

> Response JSON Structure:

```json
{
  "success": true
}
```

Uploads photo for current application. Make sure you use enctype with 'multipart/form-data' value.
File size is limited to 1 MB.

### HTTP Request

`POST https://api.mfi.com/api/mobile/forms/questionnaire/upload-photo`

### Parameters

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
guid      | required | string | application identifier
photo_front or photo_back | required | string | file

<aside class="notice">
Send photo_front or photo_back file per request.
</aside>

### Return parameters

Main response object:

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
success   | required | boolean | result

# Clients

## Check client

> Examples:

```shell
curl -X GET 'https://api.mfi.vn/api/mobile/client/check?client_id=01221111111'

```

> Return data structure:

```json
{
    "exist": true
}
```

### HTTP Request

`GET https://api.mfi.vn/api/mobile/client/check`

### Query Parameters

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
client_id | required | string | client id

### Return parameters

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
exist     | required | boolean | defines whether client with id exists or not

### Test data

Client | Id
------ | --------
client 1 | 01221111111
client 2 | 01221111222
client 3 | 01221111333
client 4 | 01221111444
client 5 | 01221111555
client 6 | 01221111666

## Set pin code for client

> Examples:

```shell
curl -X POST -H 'Content-Type: application/json' 'https://api.mfi.vn/api/mobile/client/pin' -k
  -d '{"client_id": "01221111666", "pin": "111111"}'
```

> Return data structure:

```json
{
    "success": true
}
```

### HTTP Request

`POST https://api.mfi.vn/api/mobile/client/pin`

### JSON Parameters

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
client_id | required | string | id of the client for which new pin will be set 
pin | required | string | new pin code

### Return parameters

Parameter | Type | Description
--------- | ---- | -----------
success   | boolean | if pin set successfully or not

## Authorize client

> Examples:

```shell
curl -X POST -H 'Content-Type: application/json' 'https://api.mfi.vn/api/mobile/client/auth' -k
  -d '{"client_id": "01221111111", "pin": "111111"}'
```

> Return data structure:

```json
{
    "token": "aa380ba1669a57948dcfbd686ed2aa2f"
}
```

### HTTP Request

`POST https://api.mfi.vn/api/mobile/client/auth`

### JSON Parameters

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
client_id | required | string | client id
pin | required | integer | current pin code

### Return parameters

Parameter | Type | Description
--------- | ---- | -----------
token   | string | token that will authenticate client for corresponding endpoint

### Common errors:

Error | Description
---- | -----------
Pin is invalid | User with client id exists but pin doesn't match
User is blocked | User is not active
User not found | User with client id does not exist

### Test data

Client ID | Pin | Active (not blocked)
--------- | --- | --------------------
0122 1111 111 | 1000 | true
0122 1111 222 | 2000 | true
0122 1111 333 | 3000 | false

## Get current client info

> Examples:

```shell
curl -X GET -H 'Auth-Token: aa380ba1669a57948dcfbd686ed2aa2f' 'https://api.mfi.vn/api/mobile/client' -k
```

> Return data structure:

```json
{
  "active": true,
  "first_name": "Shchi",
  "last_name": "Sk\u00ed",
  "middle_name": "G\u00eal",
  "client_id": "01221111111"
}
```

Requires authentication token header.

### HTTP Request

`GET https://api.mfi.vn/api/mobile/client`

### Return parameters

Parameter | Type | Description
--------- | ---- | -----------
active   | boolean | if client is active or not
first_name | string | first name
last_name | string | last name
middle_name | string | middle name

# Credits

## Get credits info

> Examples:

```shell
curl -X GET -H 'Auth-Token: aa380ba1669a57948dcfbd686ed2aa2f' 'https://api.mfi.vn/api/mobile/credits' -k
```

> Return data structure:

```json
[
  {
    "status": "issued",
    "credit_sum": 2000000,
    "appointment": "new phone",
    "nearest_payment_value": 2280000,
    "message": "",
    "contract": "MM1234567",
    "id": 1,
    "return_sum": 2280000,
    "nearest_payment_date": 1448014709,
    "credit_date": 1446805109,
    "action": "first loan free",
    "is_expired": false,
    "debt_amount": 280000
  },
  ...
]
```

Requires authentication token header. Returns list of credits for current authenticated client.

### HTTP Request

`GET https://api.mfi.vn/api/mobile/credits`

### Return parameters

<aside class="warning">
Will be updated later when actual credit structure will come. Now returns some temp data.
</aside>


## Get credit info

Requires authentication token header. Returns credit info with particular id for current authenticated client.

### HTTP Request

`GET https://api.mfi.vn/api/mobile/credits/<credit_id>`

> Examples:

```shell
curl -X GET -H 'Auth-Token: aa380ba1669a57948dcfbd686ed2aa2f' 'https://api.mfi.vn/api/mobile/credits/1' -k
```
