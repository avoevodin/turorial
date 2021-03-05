# HTTP requests tutorial.

## Basic definitions.
    > HTTP - hypertext transfer protocol. Data transfer protocol of application level.

### Main characteristics:

* Client-Server technology:
    > Client initiates a connection and send queries. Server waits for connection 
    > for getting query, processes query and return the result.
* HTTP is used as transport for another application level protocols.
* **URI** (Uniform resource identifier) point the main object of HTTP-manipulations.
    It may be a file, logical object or something abstract.
* HTTP can specify different ways of presentation for the resource
  (format, encoding, language etc.) in request and response, 
  that makes possible to send binary data between client and server.
* HTTP **doesn't save its status**. _Components may save status of last 
  request-response pairs by its own. For example Cookie._
* HTTP creates new TCP sessions for every request.
* HTTP indicates type of sending data in a special header "Content-Type"

### Main categories of software for HTTP.
* **Servers** - the main supplier for services of information storing 
  and processing (Origin server). 
* **Client** - the end-point consumer of server's services. 
* **Proxy** - agent in transporting services.

    > The same software may be a server, client or proxy depending 
    > on its current role.

## Structure of HTTP-request.
* **Starting line** - indicates message type.
* **Headers** - describe message body, transfer's parameters and other options.
* **Message Body** - message data. Must be separated with empty line from 
  another part of message.
    > _Message body may not exist, but another parts of HTTP-request 
    > are obligatory._

### Starting Line.
* Abstract request:
```http request
selected_method URI HTTP/Version
```
* Example of request:
```http request
GET /wiki/HTTP HTTP/1.1
Host: ru.wikipedia.org
```
* Abstract response:
```http request
HTTP/Version StatusCode ReasonPhrase
```
> **Version** is like in request, **Status Code**, **Reason Phrase** is a short 
> text explanation for response's code for an user.
* Example of response:
```http request
HTTP/1.1 200 OK
```

### **Methods**.
> **HTTP Method** - sequence of any symbols, except of operators 
> and splitters, indicates the main operation for resource. Usually 
> methods are named with upper-case english word. Registry sensitive.

#### OPTIONS
> Method returns possibilities of web-server or connection's parameters for 
> only resource. Response should include a header _allow_ with supported 
> methods, sometimes may include information about supported extensions.
* Request for exploring possibilities of entire server. May be used as ping.
  result doesn't cache.
```http request
OPTIONS * HTTP/1.1
```

#### GET
> Method returns contents of selected resource. Also, it's possible to start some 
> processes, information about progress should be included in response's body 
> in that case. 
> Example of passing parameters:
```http request
GET /path/resource?param1=value1&param2=value2 HTTP/1.1
```

#### HEAD
> Method is analog of GET, but doesn't contain a message's body.
> Usually it's used for getting metadata, checking for resource existing
> (URL validation) and resource version.
> **Headers of response may be cached.**

#### POST
> Usually method is used for transfer user's data enclosed in the request 
    to selected web-resource. If the resource was created, method returns
    URI of this resource in response. **Response isn't cached**.

#### PUT
> The PUT method requests that the enclosed entity be stored under the supplied URI. 
> If the URI refers to an already existing resource, it is modified; 
> if the URI does not point to an existing resource, 
> then the server can create the resource with that URI.
> 
> Unlike POST, PUT doesn't suggest that enclosed in request content 
> will be processed by the server.
> 
> **Response isn't cahed**

#### PATCH
> Method is similarly to PUT, but applies only to the fraction 
> of the resource.

#### DELETE
> Method deletes selected resource.

#### TRACE
> Method returns response with information about the changes, 
> that staged servers makes with the request.

#### CONNECT
> Transform the request connection to transparent TCP/IP tunnel.
> Usually it contributes to establish protected SSL-connection through
> the unprotected proxy.

### **HTTP status codes**
> Part of the first line of the server's response on HTTP-requests.

#### 1xx - Informational:
* 100 - Continue
* 101 - Switching Protocols
* 102 - Processing
* 103 - Early Hints

#### 2xx - Success:
* 200 - OK 
  > Data may be enclosed in a message body or in a header.
* 201 - Created 
  > Preferred address of created resource is in 
  > field Location. Format of response body contains in field
  > Content-Type.
* 202 - Accepted 
  > Request is accepted for processing.
* 204 - No Content
  > Response contains only headers. Client shouldn't update the content, but can
  > apply received metadata to content.

#### 3xx - Redirection:
* 301 - Moved permanently
  > The requested document was finally transferred to the address, enclosed in field Location.
* 302 - Moved temporarily or Found
  > The requested document is temporarily available at the address, specified in
  > the field Location.
* 304 - Not Modified
  > Server returns that code if client use GET-method with headers 
  > If-Modified-Since or If-None-Match and requested document haven't
  > been changed since specified moment. Response shouldn't contain
  > message body.

#### 4xx - Client Error:
* 400 - Bad Request
  > There's a syntax error in a client's request.
* 401 - Unauthorized
  > Authentication is required for the resource. In response header
  > should be the field WWW-Authenticate with authentication conditions.
* 403 - Forbidden
  > Client isn't allowed to operate requested resource.
* 404 - Not Found
* 405 - Method Not Allowed
  > Response should enclose allowed methods in the header ALlow.

#### 5xx - Server Error:
* 500 - Internal Server Error
  > Any outlaw error.
* 502 - Bad Gateway
  > Server as a gate has received invalid response from the upstream
  > server.
* 503 - Service Unavailable
  > Server is unavailable by technical reasons (maintenance, reboot etc.)
* 504 - Gateway Timeout
  > Server as a gate hasn't received respons from the upstream server.

### HTTP Headers.
* Ex:
```http request
Server: Apache/2.2.11 (Win32) PHP/5.3.0
Last-Modified: Sat, 16 Jan 2010 21:16:42 GMT
Content-Type: text/plain; charset=windows-1251
Content-Language: ru
```
* Ex of multi-line headers:
    1. Wrong:
    ```http request
        content-type: text/html;
                      charset=windows-1251
        Allow: GET, HEAD
        Content-Length: 356
        ALLOW: GET, OPTIONS
        Content-Length:   1984
    ```
    2. Correct:
    ```http request
        Content-Type: text/html;charset=windows-1251
        Allow: GET,HEAD,OPTIONS
        Content-Length: 1984
    ```

#### Headers Hierarchy:
1. General Headers - headers of any message: client or server.
2. Request Headers - headers of client's message.
3. Response Headers - headers of server's message.
4. Entity Headers - headers of entity in a message.

#### Headers in HTML:
```html
<html>
<head>
<meta http-equiv="Content-Type" content="text/html;charset=windows-1251">
<meta http-equiv="Content-Language" content="ru">
```
