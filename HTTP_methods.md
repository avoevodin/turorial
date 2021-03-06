# Examples of HTTP methods

## GET
* GET request:
```http request
GET /api/order/?date_from=01.01.2020&date_to=02.01.2020&items=20&page=3 HTTP/1.1
Host: andrew-money.ru
User-Agent: Mozilla/5.0
Accept: */*
```
* GET response:
```http request
HTTP/1.1 200 OK
Server: nginx/1.14.1
Date: Wed, 03 Mar 2021 17:58:39 GMT
Content-Type: text/html
Content-Length: 185
Set-Cookie: ....

<html>
.....
</html>
```

## POST

### X-FORM-URLENCODED request
* Request:
```http request
POST /api/order/ HTTP/1.1
Host: andrew-money.ru
User-Agent: Mozilla/5.0
Accept: */*
Content-Type: application/x-www-form-urlencoded
Content-Length: 67

sum=10000&receiver_id=123&payer_id=344&currency=RUB&date=03.01.2020
```
* Response:
```http request
HTTP/1.1 201 Created
Server: nginx/1.14.1
Date: Wed, 03 Mar 2021 17:58:39 GMT
Location: https://andrew-money.ru/api/order/81231203
Content-Type: application/json
Content-Length: 135

{
"sum": 10000,
"receiver_id": "123",
"receiver": "Andrew",
"payer_id": 344,
"payer": "Elia",
"currency": "RUB",
"date": "2020-01-03"
}
```

### application/json
* Request:
```http request
POST /api/gift_certificate/ HTTP/1.1
Host: andrew-money.ru
User-agent: Mozilla/5.0
Accept: application/json
Content-Type: application/json
Content-Length: 72 

{
"donor_id": 123,
"cert_type_id": 4,
"sum": 5000,
"recepient_id": 888
}
```
* Response:
```http request
HTTP/1.1 201 Created
Server: nginx/1.19.1
Date: Fri, 05 Mar 2021 02:58:39 GMT
Location: https://andrew-money.ru/api/gift_certificate/8348448
Content-Type: application/json
Content-Length: 120

{
"donor_id": 123,
"donor": "Kindman",
"cert_type_id": 4,
"cert_type": "Full access",
"sum": 5000,
"recepient_id": 888
}
```

### multipart/form-data simple
* Request:
```http request
POST /api/profile/ HTTP/1.1
Host: andrew-money.ru
User-agent: Mozilla/5.0
Accept: application/json
Content-Type: multipart/form-data; boundary=-------delimiter
Content-Length: 484

-------delimiter
Content-disposition: form-data; name="profile_id"

448192
-------delimiter
Content-disposition: form-data; name="passport"

"1243 345654"
-------delimiter
Content-disposition: form-data; name="leverage"

10000
-------delimiter
Content-disposition: form-data; name="currency"

"USD"
-------delimiter
Content-disposition: form-data; name="birth_date"

"1990-02-08"
-------delimiter
Content-disposition: form-data; name="account_expires"

"2021-05-01"
-------delimiter--
```
* Response:
```http request
HTTP/1.1 201 Created
Server: nginx/1.19.1
Date: Fri, 05 Mar 2021 02:58:39 GMT
Location: https://andrew-money.ru/api/profile/448192
Content-Type: application/json
Content-Length: 219

{
"profile_id": 448192,
"name": "Ad Aloy",
"passport": "1243 345654",
"birth_date: "1990-02-08",
"account_expires": "2021-05-01",
"leverage": 10000,
"currency": "USD",
"passport_is_verified": 1,
"photo_is_verified": 0
}
```

### multipart/form-data with file
* Request:
```http request
POST /api/profile/ HTTP/1.1
Host: andrew-money.ru
User-agent: Mozilla/5.0
Accept: application/json
Content-Type: multipart/form-data; boundary=-------delimiter
Content-Length: ...

-------delimiter
Content-disposition: form-data; name="Main Description"

Ad ALoy
-------delimiter
Content-disposition: form-data; name="Photo"; filename="AdAloy.png"
Content-Type: image/png

Image in binary
-------delimiter
Content-disposition: form-data; name="Profile Data"
Content-Type: application/json
Content-Length: 136

{
"profile_id": 448192,
"passport": "1243 345654",
"leverage": 10000,
"currency": "USD",
"birth_date: "1990-02-08",
"account_expires": "2021-05-01"
}
-------delimiter--
```
* Response:
```http request
HTTP/1.1 201 Created
Server: nginx/1.19.1
Date: Fri, 05 Mar 2021 02:58:39 GMT
Location: https://andrew-money.ru/api/profile/448192
Content-Type: application/json
Content-Length: 219

{
"profile_id": 448192,
"name": "Ad Aloy",
"passport": "1243 345654",
"birth_date: "1990-02-08",
"account_expires": "2021-05-01",
"leverage": 10000,
"currency": "USD",
"passport_is_verified": 1,
"photo_is_verified": 0
}
```

## PUT
* Request:
```http request
PUT /api/gift_certificate/8348448 HTTP/1.1
Host: andrew-money.ru
User-agent: Mozilla/5.0
Accept: application/json
Content-Type: application/json
Content-Length: 72 

{
"donor_id": 123,
"cert_type_id": 4,
"sum": 5000,
"recepient_id": 888
}
```
* Response:
```http request
HTTP/1.1 200 OK
Server: nginx/1.19.1
Date: Fri, 05 Mar 2021 02:58:39 GMT
Content-Type: application/json
Content-Length: 194

{
"donor_id": 123,
"donor_name": "Kindman",
"cert_id": 8348448,
"cert_type_id": 4,
"cert_type": "Unlimited access for add analytics",
"sum": 5000,
"recepient_id": 888,
"recepient": "Lucky Man"
}
```

## PATCH
* Request:
```http request
PATCH /api/gift_certificate/8348448 HTTP/1.1
Host: andrew-money.ru
User-agent: Mozilla/5.0
Accept: application/json
Content-Type: application/json
Content-Length: 16 

{
"sum": 10000
}
```
* Response:
```http request
HTTP/1.1 200 OK
Server: nginx/1.19.1
Date: Fri, 05 Mar 2021 02:58:39 GMT
Content-Type: application/json
Content-Length: 175

{
"donor_id": 123,
"donor_name": "Kindman",
"cert_type_id": 4,
"cert_type": "Unlimited access for add analytics",
"sum": 10000,
"recepient_id": 888,
"recepient": "Lucky Man"
}
```

## DELETE
* Request:
```http request
DELETE /api/gift_certificate/8348448 HTTP/1.1
Host: andrew-money.ru
User-agent: Mozilla/5.0
Accept: */*
```
* Response Fail:
```http request
HTTP/1.1 403 Forbidden
Host: andre-money.ru
Content-Length: 106
Content-Type: application/json

{
"message": "Server error occurred",
"status": "Error",
"code": 10,
"date": "2021-03-06T112:24:48.512Z"
}
```
* Response Fail:
```http request
HTTP/1.1 200 OK
Host: andre-money.ru
Content-Length: 102
Content-Type: application/json

{
"message": "The certificate has been deleted",
"status": "OK",
"date": "2021-03-06T112:24:48.512Z"
}
```