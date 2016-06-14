HTTP Methods
============
To interact with a REST API, HTTP methods are used. The mainly used methods are:

- GET
- POST 

## GET
- Requests data from a specific resource.
- Information required to get to the requested data can be sent via a query string in the URL of a GET request.
- Can also be sent as a conditional GET, when the request message header includes an
    - `If-Modified-Since`
    - `If-Unmodified-Since`
    - `If-Match`
    - `If-None-Match`
    - `If-Range`

NOTE: See HTML query for more information regarding request message query.

## POST
- Submits data to a specific resource.
- Any necessary query string is sent in the HTTP message body of the POST request.
- In detail, POST requires the server to accept an enclosed entity and process it at the
    provided Request-URI in the request line.

## Further HTTP methods:
- PUT       ... Submits data and requests, that it should be stored at an also provided Request-URI.
- HEAD      ... like GET, but returns only the HTTP headers without a document body
- DELETE    ... Sends a Request-URI and requests, that the resource found at this URI should be deleted.
- OPTIONS   ... Returns which HTTP methods are supported by the server.
- CONNECT   ... Converts the connection to a TCP/IP tunnel

Find more basics [here](http://www.w3schools.com/tags/ref_httpmethods.asp)
and more details [here](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

HTML query
==========

- A query string is a part of the URL and can be used to submit data to a server.
- A query string is composed of key-value pairs, each pair is separated by "&"
- Key-value of each pair is separated by "=" e.g. field1=value1
- The query added to the main URL and is separated from it by "?".

    http://example.org/nowhere?field1=value1&field2=value2

NOTE: HTML URLs have to solely consist of characters from the ASCII character-set, other characters need to be encoded.
Find a description how to properly encode a URL [here](http://www.w3schools.com/tags/ref_urlencode.asp).


REST API status codes
=====================

A list of REST API status codes can be found [here](http://www.restapitutorial.com/httpstatuscodes.html)
or [here](http://www.w3schools.com/tags/ref_httpmessages.asp).


LESS
====

less files require the metadata tags:
    fileExtension='.less'
    mimeType='text/css'


JSON (Java Script Object Notation)
==================================
http://www.w3schools.com/json/default.asp

Data format for information exchange between applications. Every valid JSON document should be valid javascript
and be interpretable by `eval()`. The default character set is utf-8.
In its syntax its similar to XML, but reduces some of the overhead.


###The main differences and similarities compared to XML are:
- both JSON and XML are plain text, human readable, hierarchical and can be fetched by httpRequest
- JSON has no end tags, can use arrays
- JSON is parsed by the standard JavaScript parser or custom functions even within a page,
XML requires its own parser before its converted to e.g. HTML.

###JSON files
- the file type is ".json"
- the MIME type is "application/json"


###JSON values can be
- null
- number (integer & floating point)
- string ... ""
- arrays ... []
- objects ... {}


###The JSON syntax defines the following main rules:
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


###JSON uses JavaScript syntax
JSON objects can be accessed using JavaScript

        var litCharacters = [
            {"firstName":"Abdul", "lastName":"Nachtigaller"},
            {"firstName":"Randolp", "lastName":"Carter"},
            {"firstName":"Arlen", "lastName":"Bales"}
        ];

        litCharacters[0].firstName +" "+ litCharacters[0].lastName;
        litCharacters[0].firstName = "Dr. Abdul";


###JSON object from string
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
