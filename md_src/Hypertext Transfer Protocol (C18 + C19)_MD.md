>Thasneem Mohamed

### HyperText Transfer Protocol (HTTP)


> HTTP (HyperText Transfer Protocol) is the communication protocol used to transfer hypertext and other resources over the Internet. It is the foundation of the World Wide Web (WWW).

HTTP follows a client-server model where a client sends a request and a server responds with data such as web pages, images, videos, or files.

Other common Internet protocols include:

* FTP / SFTP → file transfer
* SSH / RDP / Telnet → remote access
* SMTP / POP3 / IMAP → email communication
* BitTorrent → peer-to-peer file sharing
* IRC → online chat systems

The World Wide Web is the collection of publicly available websites and multimedia resources accessible using HTTP. However, not everything on the Internet is part of the WWW. Examples outside the scope of the web include:

* Online games
* Email systems
* Cloud storage services
* P2P networks

Examples:

* Counterstrike
* Outlook
* Dropbox
* BitTorrent

---

## Client-Server Model and HTTP Flow

HTTP operates using the client-server architecture.

#### Client

The client is usually:

* A web browser
* Mobile application
* Software application

Responsibilities of the client:

1. Send requests
2. Wait for responses
3. Process returned data

#### Server

The server stores and manages resources.

Responsibilities of the server:

1. Wait for incoming requests
2. Process the requests
3. Send responses back to the client

---

### HTTP Communication Flow

Basic HTTP communication process:

1. Client sends an HTTP request
2. Server receives and processes the request
3. Server sends an HTTP response
4. Client processes or displays the response

Although TCP handles low-level networking details such as handshakes and acknowledgements, HTTP abstracts those details and mainly focuses on the application data being exchanged.

---

## HTTP Request

>An HTTP request is structured text sent from a client to a server requesting a resource.

Resources may include:

* HTML documents
* Images
* CSS files
* JavaScript files
* Videos

### Structure of an HTTP Request

An HTTP request contains:

* Request line
* Headers
* Optional body

Example:

```http
GET /index.html HTTP/1.1
```

### Request Components

#### Request Line

Contains:

* HTTP method
* Resource path
* HTTP version

#### Headers

Headers provide additional information about the request.

Examples:

* Host
* User-Agent
* Accept
* Accept-Language
* Cookie

Headers help customize communication between client and server.

---

## HTTP Response

> After processing a request, the server sends an HTTP response.

### Structure of an HTTP Response

An HTTP response contains:

* Status line
* Headers
* Response body

Example:

```http
HTTP/1.1 200 OK
```

### Response Components

#### Status Line

Contains:

* HTTP version
* Status code
* Status message

#### Headers

Provide information about the response.

Examples:

* Content-Type
* Content-Length
* Date
* Server

#### Body

Contains the actual content:

* Web pages
* Images
* Files
* JSON data

---

## HTTP Status Codes

Status codes indicate the result of a request.

#### 2XX – Success

The request was successful.

Example:

* 200 OK

#### 3XX – Redirection

Additional action is required.

Example:

* 304 Not Modified

#### 4XX – Client Errors

Problem caused by the client.

Examples:

* 403 Forbidden
* 404 Not Found

#### 5XX – Server Errors

Problem occurred on the server.

Example:

* 500 Internal Server Error

---

### Browser Caching and Headers

Caching improves performance by preventing unnecessary downloads of unchanged resources.

### ETag

An ETag is a unique identifier assigned to a file or resource.

Example:

```http
ETag: "578-58049612f21c0"
```

The browser stores this identifier after downloading the file.

### If-None-Match

When revisiting a page, the browser may send:

```http
If-None-Match: "578-58049612f21c0"
```

This asks the server whether the file has changed.


#### 304 Not Modified

If the file has not changed, the server responds:

```http
304 Not Modified
```

The browser then uses the locally cached version instead of downloading the file again.

### Advantages of Caching

* Faster loading speed
* Lower bandwidth usage
* Reduced server load

---

## Browsers and Secondary Requests

A browser is software that automates:

* Sending HTTP requests
* Receiving HTTP responses
* Rendering web pages

Examples:

* Chrome
* Firefox
* Edge
* Safari

The user usually only provides a URL, while the browser handles the communication process automatically.

---

## Secondary Requests

When a browser loads an HTML document, it scans the page for embedded resources such as:

* Images
* CSS files
* JavaScript files
* Audio and video

The browser automatically sends additional requests for these resources.

Example:

```html
<img src="image.png">
```

This generates:

```http
GET /image.png
```

If the file exists:

```http
200 OK
```

If the file does not exist:

```http
404 Not Found
```

A missing image may appear as a broken image icon in the browser.

Modern web pages may generate:

* Secondary requests
* Tertiary requests

because scripts and stylesheets may also contain additional resources.

---

## Browser Developer Tools

Developer tools help inspect network activity and debugging information.

Shortcut:

```text
CTRL + SHIFT + J
```

The Network tab shows:

* Requests
* Responses
* Loading times
* Waterfall charts

This helps visualize how resources are downloaded and rendered.

---

### Stateless Nature of HTTP

HTTP is a stateless protocol.

This means:

* Each request is independent
* The server does not automatically remember previous requests

If two identical requests are sent, the server treats them separately.

HTTP itself has no built-in memory.



### Maintaining State

Web applications simulate memory using:

* Cookies
* Sessions
* Tokens

These technologies help websites remember:

* Login sessions
* User preferences
* Shopping carts

However, clients can delete cookies or refuse tracking, making persistent identification difficult using HTTP alone.

