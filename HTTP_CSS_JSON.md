HTTP
====

- HTTP is a `response-request` protocol
- A client (usually a browser) sends a `request`, a server answers with a `response`


## The HTTP Request

An HTTP request has the following form:
- Request line with syntax `<HTTP Request Method> <URI>` terminated by a CRLF (carriage return line feed).
- Zero or more request headers of syntax `key: value[, key: value[, ...]]` terminated by a CRLF.
- An empty line terminated by a CRLF (this has to be present even if the message body is empty).
- [Optional] The message body.

Example:

        GET /Protocols/rfc2616/rfc2616.html HTTP/1.1
        Host: www.w3.org, User-Agent: Mozilla/5.0
        (empty line)

### HTTP request methods:
- `GET`       ... Requests data from a specific resource, see below for details. Idempotent.
- `POST`      ... Submits data to a specific resource, see below for details.
- `PUT`       ... Submits data and requests, that it should be stored at an also provided Request-URI. Idempotent.
- `PATCH`     ... An addition to POST and PUT - handles adds, updates and deletes of a specific resource
                    in the way the request describes.
- `HEAD`      ... like GET, but returns only the HTTP headers without a document body. Idempotent.
- `DELETE`    ... Sends a Request-URI and requests, that the resource found at this URI should be deleted. Idempotent.
- `OPTIONS`   ... Returns which HTTP methods are supported by the server.
- `CONNECT`   ... Converts the connection to a TCP/IP tunnel

NOTE: The most commonly used HTTP Request Methods are `GET` and `POST`.
NOTE: Idempotent means, that no matter how often an idempotent method is executed, the side effects 
and the outcome are always the same.


##### GET
- Requests data from a specific resource.
- Information required to get to the requested data can be sent via a query string in the URL of a GET request.
- Can also be sent as a conditional GET, when the request message header includes an
    - `If-Modified-Since`
    - `If-Unmodified-Since`
    - `If-Match`
    - `If-None-Match`
    - `If-Range`

NOTE: See HTML query for more information regarding request message query.

##### POST
- Submits data to a specific resource.
- Any necessary query string is sent in the HTTP message body of the POST request.
- In detail, `POST` requires the server to accept an enclosed entity and process it at the
    provided Request-URI in the request line.

##### PATCH

The relevant RFCs regarding PATCH / PATCH + JSON:

- [PATCH Method for HTTP](https://tools.ietf.org/html/rfc5789)
- [JavaScript Object Notation (JSON) Patch](https://tools.ietf.org/html/rfc6902)
- [JSON Merge Patch](https://tools.ietf.org/html/rfc7396)
- An excellent article regarding actually using PATCH can be found 
[here](http://williamdurand.fr/2014/02/14/please-do-not-patch-like-an-idiot/). 


### HTTP Request Header fields

- HTTP request headers are optional, except:
    - `Host: [URI]` has to be present in HTTP 1.1.
    - `Content-Length` OR `Transfer-Encoding` have to be present, if the request has a message body.
- RFC 7231 defines a core set of HTTP request header fields
- There is a multitude of nonstandard HTTP request header fields.
- A summary of the HTTP request header fields can be found
    [at your trusty wikipedia site](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields#Request_fields).


### The Request URI
- The request URI follows the syntax `<scheme name> : <hierarchical part> [ ?<query> ] [ # <fragment> ]`

    - the `scheme name` defines how the rest of the URI structure is to be interpreted (which protocol is used)

            http:

    - the `hierarchical part` defines the actual "identifying" part of the URI which is hierarchical in nature.
        - it can start with `/` or `//`, the hierarchical subparts are separated by `/`

                http://www.example.org/howhere

        - if it starts with `//`, it can also contain user information like username and password, separated from the URI by an `@`.

                http://myUser:myPassword@www.example.org/nowhere

    - [optional] the `query` part:
        - it always starts with a `?`.
        - it is composed of key-value pairs, separated by `&`.
        - key and value of each pair are separated by `=`.

                http://www.example.org/nowhere?field1=value1&field2=value2

    - [optional] the `fragment` part:
        - is meant to be an identifier pointing to a part within the referenced `<hierarchical part>`
        - it always starts with a `#`.
        - if the URI contains a `query` part, the `fragment` is always added after the query.

                http://www.example.org/nowhere?field1=value1&field2=value2#summary


NOTE: HTML URLs have to solely consist of characters from the ASCII character-set, other characters need to be encoded.
Find a description how to properly encode a URL at [w3schools](http://www.w3schools.com/tags/ref_urlencode.asp).


## The HTTP Response

When a server receives an HTTP Request, it will always respond with an HTTP Response of the form:
- A status line with syntax `<Status Code (as Int)> <Reason phrase>` followed by a CRLF.
- Zero or more response headers with syntax `key: value[, key: value[, ...]]` followed by a CRLF.
- An empty line followed by a CRLF (this has to be present even if the message body is empty).
- [Optional] The message body.


Example:

    200 OK
    Date: Sat, 22 Nov 2014 12:58:58 GMT
    Server: Apache/2
    Last-Modified: Thu, 28 Aug 2014 21:01:33 GMT
    Content-Length: 33115
    Content-Type: text/html; charset=iso-8859-1

    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
        "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
        <html xmlns='http://www.w3.org/1999/xhtml'> <head><title>Hypertext Transfer Protocol
        -- HTTP/1.1</title></head><body>...</body></html>


### The HTTP Response Status Code

- The response status code reflects how the server reacts to a request.
    - 1xx ... Informational
    - 2xx ... Success
    - 3xx ... Redirection
    - 4xx ... Client Error
    - 5xx ... Server Error

- Decent details about each of the HTTP Status codes can be found at
    - [REST API tutorial](http://www.restapitutorial.com/httpstatuscodes.html) and
    - [w3c schools](http://www.w3schools.com/tags/ref_httpmessages.asp).


### The HTTP Response Header fields

- HTTP response header fields are similar to the request fields in syntax and usage.
- Find a list [here](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields#Response_fields).



## Resources
- [Basics about HTTP methods](http://www.w3schools.com/tags/ref_httpmethods.asp)
- [Details about the http protocols](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)
- [More about XHR](https://en.wikipedia.org/wiki/XMLHttpRequest) and a specification [here](https://xhr.spec.whatwg.org/)
- Information about [CORS (Cross origin resource sharing)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)


CSS and Bootstrap
=================
For CSS related lookups use [GetBootstrap](http://getbootstrap.com/)


LESS
====
less files require the metadata tags:

    fileExtension='.less'
    mimeType='text/css'


Fonts and Icons for use with CSS
================================

Find resources at [Fontawesome](http://fontawesome.io/)
Find more resources at [cssreference](http://cssreference.io/).


JSON (Java Script Object Notation)
==================================
http://www.w3schools.com/json/default.asp

Data format for information exchange between applications. Every valid JSON document should be valid javascript
and be interpretable by `eval()`. The default character set is utf-8.
In its syntax its similar to XML, but reduces some of the overhead.


### The main differences and similarities compared to XML are:
- both JSON and XML are plain text, human readable, hierarchical and can be fetched by httpRequest
- JSON has no end tags, can use arrays
- JSON is parsed by the standard JavaScript parser or custom functions even within a page,
XML requires its own parser before its converted to e.g. HTML.

### JSON files
- the file type is ".json"
- the MIME type is "application/json"


### JSON values can be
- null
- number (integer & floating point)
- string ... ""
- arrays ... []
- objects ... {}


### The JSON syntax defines the following main rules:
- strings are always in double quotes
- data is in name:value pairs, separated by a colon

        "firstName":"Blaubaer"

- data is separated by commas
- curly braces hold objects -- objects can hold multiple name:value pairs

        {"firstName":"Abdul", "lastName":"Nachtigaller"}

- square brackets hold arrays, arrays can hold multiple objects

        "litCharacters": [
            {"firstName":"Abdul", "lastName":"Nachtigaller"},
            {"firstName":"Randolph", "lastName":"Carter"},
            {"firstName":"Arlen", "lastName":"Bales"}
        ]


### JSON uses JavaScript syntax
JSON objects can be accessed using JavaScript

        var litCharacters = [
            {"firstName":"Abdul", "lastName":"Nachtigaller"},
            {"firstName":"Randolp", "lastName":"Carter"},
            {"firstName":"Arlen", "lastName":"Bales"}
        ];

        litCharacters[0].firstName +" "+ litCharacters[0].lastName;
        litCharacters[0].firstName = "Dr. Abdul";


### JSON object from string
To convert a JSON string to a JSON object, the JavaScript function `JSON.parse()` is used,
this object can in turn be used within the web page.

    var text = '{ "litCharacters" : = [' +
        '{"firstName":"Abdul", "lastName":"Nachtigaller"},' +
        '{"firstName":"Randolp", "lastName":"Carter"},' +
        '{"firstName":"Arlen", "lastName":"Bales"} ]}';

    var obj = JSON.parse(text);
    <p id="demo"></p>
    <script>
        document.getElementById("demo").innerHTML =
        obj.litCharacters[1].firstName +" "+ obj.litCharacters[1].lastName;
    </script>
