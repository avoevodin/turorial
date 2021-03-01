# HTTP requests tutorial.

## Basic definitions.
    > HTTP - hypertext transfer protocol. Data transfer protocol of application level.

### Main characteristics:

* Client-Server technology:
    > Client initiates a connection and send queries. Server waits for connection
        for getting query, processes query and return the result.
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
        on its current role.

## Structure of HTTP-request.
* **Starting line** - indicates message type.
* **Headers** - describe message body, transfer's parameters and other options.
* **Message Body** - message data. Must be separated with empty line from 
  another part of message.
    > _Message body may not exist, but another parts of HTTP-request 
        are obligatory._

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
    text explanation for response's code for an user.
* Example of response:
```http request
HTTP/1.1 200 OK
```

### **Methods**.
> **HTTP Method** - sequence of any symbols, except of operators 
    and splitters, indicates the main operation for resource. Usually
    methods are named with upper-case english word. Registry sensitive.

#### OPTIONS.
> Method returns possibilities of web-server or connection's parameters for 
    only resource. Response should include a header _allow_ with supported
    methods, sometimes may include information about supported extensions.
* Request for exploring possibilities of entire server. May be used as ping.
  result doesn't cache.
```http request
OPTIONS * HTTP/1.1
```

#### GET.
> Method returns contents of selected resource. Also, it's possible to start some
    processes, information about progress should be included in response's body
    in that case. 
> Example of passing parameters:
```http request
GET /path/resource?param1=value1&param2=value2 HTTP/1.1
```

#### HEAD.
> Method is analog of GET, but doesn't contain a message's body.
    Usually it's used for getting metadata, checking for resource existing 
    (URL validation) and resource version.
> **Headers of response may be cached.**

#### POST.
> Usually method is used for transfer user's data enclosed in the request 
    to selected web-resource. If the resource was created, method returns
    URI of this resource in response. **Response isn't cached**.

#### PUT.
> The PUT method requests that the enclosed entity be stored under the supplied URI. 
> If the URI refers to an already existing resource, it is modified; 
> if the URI does not point to an existing resource, 
> then the server can create the resource with that URI.
> 
> Unlike POST, PUT doesn't suggest that enclosed in request content 
> will be processed by the server.