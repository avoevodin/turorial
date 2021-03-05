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
Content-Length: 185

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
Accept: application/pdf
Content-Type: application/json
Content-Length: 68 

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
Content-Type: application/pdf
Content-Length: ...

Pdf in binary
```

### multipart/form-data
* Request:
```http request
POST /api/profile/ HTTP/1.1
Host: andrew-money.ru
User-agent: Mozilla/5.0
Accept: application/json
Content-Type: multipart/form-data; boundary=-------delimiter

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
"Account_id": 448192,
"Passport": 1243 345654,
"BirthDate: "1990-02-08",
"Account_Expires": "2021-05-01",
"leverage": 10000,
"Currency": USD 
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
Content-Length: 188s

{
"Account_id": 448192,
"Name": "Ad Aloy",
"Passport": 1243 345654,
"BirthDate: "1990-02-08",
"Account_Expires": "2021-05-01",
"leverage": 10000,
"Currency": USD,
"Passport_Is_Verified": yes,
"Photo_Is_Verified": no
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
Content-Length: 68 

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
Content-Length: 185

{
"donor_id": 123,
"donor_name": Kindman,
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
Content-Length: 12 

{
"sum": 10000
}
```
* Response:
```http request
HTTP/1.1 201 Created
Server: nginx/1.19.1
Date: Fri, 05 Mar 2021 02:58:39 GMT
Location: https://andrew-money.ru/api/gift_certificate/8348448
Content-Type: application/json
Content-Length: 167

{
"donor_id": 123,
"donor_name": Kindman,
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
```
* Response:
```http request
HTTP/1.1 403 Forbidden
```