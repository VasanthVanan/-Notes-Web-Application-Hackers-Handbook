# Chapter-3 Web Application Technologies

# 3.1 The HTTP Protocol
* HTTP is the primary protocol used to access the World Wide Web and is utilized by modern web applications.
* Initially designed for retrieving static text-based resources, HTTP has evolved to support complex distributed applications.
* HTTP operates on a message-based model, where a client sends a request message and the server responds with a message.
* The protocol is connectionless, despite TCP being used as the transport mechanism and can use a different TCP connection.
  
## 3.1.1 HTTP Requests

```
  GET /auth/488/YourDetails.ashx?uid=129 HTTP/1.1
  Accept: application/x-ms-application, image/jpeg, application/xaml+xml,
  image/gif, image/pjpeg, application/x-ms-xbap, application/x-shockwave-
  flash, */*
  Referer: https://mdsec.net/auth/488/Home.ashx
  Accept-Language: en-GB
  User-Agent: Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1; WOW64;
  Trident/4.0; SLCC2; .NET CLR 2.0.50727; .NET CLR 3.5.30729; .NET CLR
  3.0.30729; .NET4.0C; InfoPath.3; .NET4.0E; FDM; .NET CLR 1.1.4322)
  Accept-Encoding: gzip, deflate
  Host: mdsec.net
  Connection: Keep-Alive
  Cookie: SessionId=5B70C71F3FD4968935CDB6682E545476
```

* **HTTP method**: GET retrieves resources.
* Requested **URL**: Identifies the resource, may include query parameters.
* **HTTP version**: Commonly 1.0 or 1.1, with minor differences.
* **Referer header**: Indicates the origin URL.
* **User-Agent header**: Provides client software information.
* **Host header**: Specifies the accessed hostname.
* **Cookie header**: Sends additional server-issued parameters.

## 3.1.2 HTTP Responses

```
HTTP/1.1 200 OK
  Date: Tue, 19 Apr 2011 09:23:32 GMT
  Server: Microsoft-IIS/6.0
  X-Powered-By: ASP.NET
  Set-Cookie: tracking=tI8rk7joMx44S2Uu85nSWc
  X-AspNet-Version: 2.0.50727
  Cache-Control: no-cache
  Pragma: no-cache
  Expires: Thu, 01 Jan 1970 00:00:00 GMT
  Content-Type: text/html; charset=utf-8
  Content-Length: 1067
  <!DOCTYPE html PUBLIC “-//W3C//DTD XHTML 1.0 Transitional//EN” “http://
  www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd”><html xmlns=”http://
  www.w3.org/1999/xhtml” ><head><title>Your details</title>
  ...
```
* **HTTP response**: First line contains HTTP version, status code, and reason phrase.
* **Server header**: Indicates the web server software.
* **Set-Cookie header**: Issues a new cookie to the browser.
* **Pragma header**: Instructs the browser not to cache the response.
* **Expires header**: Indicates that the content has expired.
* **Message body**: Contains the response content, often an HTML document.
* **Content-Length header**: Specifies the length of the message body.

## 3.1.3 HTTP Methods
* When attacking web applications, the commonly used methods are **GET** and **POST**.
* GET is used to retrieve resources and can send parameters in the URL query string.
* GET URLs are visible, logged, and transmitted in the `Referer header`, 
   so sensitive information should not be included in the query string.
* POST is used to perform actions and allows parameters in both the URL and message body.
* POST parameters in the message body are not included in bookmarks, logs, or the Referer header.
* The browser warns users when navigating back to a page accessed with `POST` to prevent unintended actions.
* POST should be used for performing actions to prevent unintentional repetitions.
---
* **HEAD**: Similar to GET but without a message body, used to check resource presence before a GET request.
* **TRACE**: Returns the exact contents of the received request, helpful for diagnosing proxy manipulation.
* **OPTIONS**: Requests available HTTP methods for a resource, server responds with an Allow header listing the methods.
* **PUT**: Attempts to upload a resource to the server using the request body, potentially exploitable for attacks like arbitrary script execution.
* Other HTTP methods exist but are not directly relevant to attacking web applications. <br>However, the presence of certain dangerous methods can make a web server vulnerable to attack.

## 3.1.4 URLs

* A URL is a unique identifier for a web resource. The format is: 
  ```
  protocol://hostname[:port]/[path/]file[?param=value]
  ```
* The URL used in the HTTP request is: 
  ```
  https://mdsec.net/auth/488/YourDetails.ashx?uid=129
  ```
* URLs can also be specified relative to a host or path. <br>Examples: 
  `/auth/488/YourDetails.ashx?uid=129`, &nbsp;`YourDetails.ashx?uid=129`.

## 3.1.5 REST

- **REST:**
  - Style of architecture for distributed systems.
  - Requests and responses represent the current state of system resources.
  - Core technologies like HTTP and URLs adhere to REST principles.

- **REST-style URL:**
  - Parameters embedded within the URL file path.
  - Used to signify parameterized URL structure.
  - Example: Query string URL vs. REST-style URL.
    ```
    http://wahh-app.com/search?make=ford&model=pinto (Query string)
    http://wahh-app.com/search/ford/pinto (REST-style)
    ```

## 3.1.6 HTTP Headers

- **General Headers:**
  - `Connection`: Tells whether to close or keep the TCP connection open.
  - `Content-Encoding`: Specifies the encoding used for content compression.
  - `Content-Length`: Indicates the length of the message body in bytes.
  - `Content-Type`: Specifies the type of content in the message body.
  - `Transfer-Encoding`: Indicates any encoding applied to facilitate transfer.

---
- **Request Headers:**
  - `Accept`: Informs the server about acceptable content types.
  - `Accept-Encoding`: Specifies acceptable content encoding.
  - `Authorization`: Submits credentials for HTTP authentication.
  - `Cookie`: Submits previously issued cookies.
  - `Host`: Specifies the hostname from the requested URL.
  - `If-Modified-Since`: Indicates the last received time of the requested resource.
  - `If-None-Match`: Specifies an entity tag to determine resource caching.
  - `Origin`: Used in cross-domain Ajax requests to indicate the request's domain.
  - `Referer`: Specifies the URL from which the request originated.
  - `User-Agent`: Provides information about the client software.

---
- **Response Headers:**
  - `Access-Control-Allow-Origin`: Indicates cross-domain Ajax request permissions.
  - `Cache-Control`: Passes caching directives to the browser.
  - `ETag`: Specifies an entity tag for resource identification.
  - `Expires`: Tells the browser how long the contents are valid.
  - `Location`: Used in redirection responses to specify the target of the redirect.
  - `Pragma`: Passes caching directives to the browser.
  - `Server`: Provides information about the web server software.
  - `Set-Cookie`: Issues cookies to the browser for subsequent requests.
  - `WWW-Authenticate`: Provides details on supported authentication types.
  - `X-Frame-Options`: Specifies how the response may be loaded within a browser frame.


## 3.1.7 Cookies

* Key part of the HTTP protocol used by web applications.
* Can be exploited to target vulnerabilities.
* Mechanism to send and store data between server and client.

- **Set-Cookie Header:**
  - Server issues a cookie using the Set-Cookie response header.
  - Example: `Set-Cookie: tracking=tI8rk7joMx44S2Uu85nSWc`

- **Cookie Header:**
  - Client's browser automatically adds the `Cookie` header to subsequent requests.
  - Example: `Cookie: tracking=tI8rk7joMx44S2Uu85nSWc`

- **Cookie Structure:**
  - Cookies consist of name/value pairs.
  - Multiple cookies can be issued using multiple Set-Cookie headers.
  - Individual cookies are separated by a semicolon in the Cookie header.

- **Optional Cookie Attributes:**
  - `expires`: Sets a date until which the cookie is valid.
  - `domain`: Specifies the valid domain for the cookie.
  - `path`: Specifies the URL path for which the cookie is valid.
  - `secure`: Cookie submitted only in HTTPS requests.
  - `HttpOnly`: Cookie not accessible via client-side JavaScript.

- **Security Impact:**
  - Cookie attributes can impact application security.
  - Direct targeting of other users can be affected.

## 3.1.8 Status Codes

- Status codes are categorized into five groups based on the first digit.
    - 1xx: Informational
    - 2xx: Successful
    - 3xx: Redirection
    - 4xx: Client errors
    - 5xx: Server errors

<br>

| Status Code | Description |
| --- | --- |
| **100** Continue | Request headers received, continue sending the body. |
| **200** OK | Request successful, response contains the result. |
| **201** Created | Successful response to a PUT request. |
| **301** Moved Permanently | Permanent redirection to a different URL. |
| **302** Found | Temporary redirection to a different URL. |
| **304** Not Modified | Use cached copy of the resource if not modified. |
| **400** Bad Request | Invalid HTTP request submitted. |
| **401** Unauthorized | HTTP authentication required. |
| **403** Forbidden | No access allowed to the requested resource. |
| **404** Not Found | Requested resource does not exist. |
| **405** Method Not Allowed | Unsupported method for the specified URL. |
| **413** Request Entity Too Large | Request body exceeds server's handling capacity. |
| **414** Request URI Too Long | URL used in the request is too large. |
| **500** Internal Server Error | Server encountered an error fulfilling the request. |
| **503** Service Unavailable | Application accessed via the server is not responding. |


## 3.1.9 HTTP Proxies

- **HTTP Proxy:**
  - Acts as a mediator between the client browser and destination web server.
  - Provides additional services such as caching, authentication, and access control.

- **Differences with Proxy:**
  1. Unencrypted HTTP Requests:
     - Browser includes full URL in the request, proxy extracts hostname and port.
  2. HTTPS Requests:
     - Browser establishes TCP-level relay with destination server through proxy using CONNECT method.

# 3.2 Web Functionality
## 3.2.1 Server-Side Functionality 

* Web applications employ various technologies to deliver their functionality.
* Dynamic content is generated by scripts or code on the server.
* HTTP requests can send parameters in different ways: `query string`, `REST-style URLs`, `cookies`, or `POST body`.
* Server-side technologies include scripting languages, web application platforms, web servers, databases, and other components.
* Examples include `PHP`, `ASP.NET`, `Java`, `Apache`, `IIS`, databases like `MySQL`, and `SOAP-based web services`.
* Java-based applications use `EJBs`, `POJOs`, `Servlets`, and` web containers`.
  * Authentication — JAAS, ACEGI
  * Presentation layer — SiteMesh, Tapestry
  * Database object relational mapping — Hibernate
  * Logging — Log4J
* `ASP.NET`: Microsoft's web framework, with support for multiple languages.
* `PHP`: Powerful language for web app development, often used in the LAMP stack.
  * Bulletin boards — PHPBB, PHP-Nuke
  * Administrative front ends — PHPMyAdmin
  * Web mail — SquirrelMail, IlohaMail
  * Photo galleries — Gallery
  * Shopping carts — osCommerce, ECW-Shop
  * Wikis — MediaWiki, WakkaWikki
* `Ruby on Rails` is known for fast application development.
* `SQL` is used for accessing databases in web applications.
* `XML` is used for data encoding. Web services use `SOAP` and XML for data exchange.
  ```xml
  <pet>ginger</pet>
  <pets><dog>spot</dog><cat>paws</cat></pets>

  <data version=”2.1”><pets>...</pets></data>
  ```
  ```xml
  POST /doTransfer.asp HTTP/1.0
  Host: mdsec-mgr.int.mdsec.net
  Content-Type: application/soap+xml; charset=utf-8
  Content-Length: 891
  <?xml version=”1.0”?>
  <soap:Envelope xmlns:soap=”http://www.w3.org/2001/12/soap-envelope”>
    <soap:Body>
        <pre:Add xmlns:pre=http://target/lists soap:encodingStyle=
  “http://www.w3.org/2001/12/soap-encoding”>
        <Account>
          <FromAccount>18281008</FromAccount>
          <Amount>1430</Amount>
          <ClearedFunds>False</ClearedFunds>
          <ToAccount>08447656</ToAccount>
        </Account>
      </pre:Add>
    </soap:Body>
  </soap:Envelope>
  ```

## 3.2.2 Client-Side Functionality
* `Hyperlinks` are frequently used for user interactions, and they can contain preset request parameters that are submitted to the server.<br>
    ```html
    <a href=”?redir=/updates/update29.html”>What’s happening?</a>
    ```
* HTML forms provide a mechanism for gathering user input and actions, and the form data is submitted to the server.
  ```html
  <form action="/login" method="post"><input type="text" name="username"><input type="password" name="password"><input type="hidden" name="redir" value="/home"><input type="submit" name="submit" value="Log In"></form>.
  ```
* `Cascading Style Sheets (CSS)` are used to describe the presentation of HTML documents and separate content from presentation.
    ```css
    h2 { color: red; }
    ```
* `JavaScript` is a programming language used to extend web interfaces, validate user input, modify the user interface dynamically, and interact with the Document Object Model (`DOM`).
* The DOM is an abstract representation of an HTML document that allows client-side scripts to access and manipulate elements, data, and events within the browser.
* `Ajax` (Asynchronous JavaScript and XML) is a collection of techniques used to create dynamic user interfaces that can perform tasks on the client side without reloading the entire page.
* `JSON` (JavaScript Object Notation) is a data transfer format commonly used in Ajax applications as an alternative to XML.
* The `same-origin policy` is a browser mechanism that restricts interactions between content from different origins to prevent unauthorized access and protect user data.
## 3.2.3 State and Sessions
* Web applications use session management to track user interactions and maintain state across requests.
* Session data is stored server-side and updated based on user actions, providing relevant information to the user.
* User identification is achieved through tokens, usually transmitted via HTTP cookies, to ensure the correct state data is used for each request.
# 3.3 Encoding Schemes
## 3.3.1 URL Encoding
* URLs can only contain printable characters in the US-ASCII character set.
* URL encoding is used to encode problematic characters using the `%` prefix and the character's ASCII code in hexadecimal.
## 3.3.2 Unicode Encoding
* Unicode is a character encoding standard that supports all writing systems.
* 16-bit Unicode encoding uses the `%u` prefix followed by the character's Unicode code point in hexadecimal.
* UTF-8 is a variable-length encoding that uses the `%` prefix and each byte expressed in hexadecimal.
## 3.3.3 HTML Encoding
* HTML encoding is used to represent problematic characters in HTML documents.
* Characters can also be HTML-encoded using their ASCII code in decimal or hexadecimal form.
## 3.3.4 Base64 Encoding
* Base64 encoding represents binary data using printable ASCII characters.
* Input data is divided into blocks of three bytes and represented using a set of 64 characters.
* Padding characters (=) are added if the final block has fewer than three chunks of output data.
## 3.3.5 Hex Encoding
* Hex encoding represents binary data using ASCII characters to represent the hexadecimal block.
* Hex-encoded data can be easily recognized by its specific character set and the presence of padding characters.

| Character | URL Encoding | Unicode Encoding | HTML Encoding | Hex Encoding |
| --------- | ------------ | ---------------- | -------------- | ------------ |
| /         | `%2F`          | %u2215           | `&#47;`         | 2F           |
| é         | `%C3%A9`       | %u00E9           | `&#233;`        | E9           |
| "         | `%22`          | %u0022           | `&quot;`       | 22           |
| <         | `%3C`          | %u003C           | `&lt;`          | 3C           |
| >         | `%3E`          | %u003E           | `&gt;`          | 3E           |
| ©         | `%C2%A9`       | %u00A9           | `&#169;`        | A9           |
