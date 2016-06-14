JSON-LD (JSON-linked data)
==========================

[w3 specification of json-ld](http://www.w3.org/TR/json-ld)


## General Terminology

### JSON object
- { } denotes a JSON-LD object
- contains zero or more key value pairs
- key and value are separated by a colon

        "key : value"

- multiple key value pairs are separated by a comma:

        "key1 : value1 , key2 : value2"

- NOTE: JSON-LD keys must be unique!

### Array
- [] denotes a JSON-LD array
- contains zero or more values
- values are separated by a comma
- NOTE: JSON-LD arrays are unordered by default!

### String
- "" denotes a JSON-LD string
- contains zero or more unicode characters
- \ is used to escape special characters

### Number
- Leading zeros are not allowed
- octal and hexadecimal are not supported

### Boolean
- true
- false

### null
- The null value is used to clear or forget data.