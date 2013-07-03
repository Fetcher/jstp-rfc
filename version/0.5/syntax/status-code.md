[Back: Syntax](index.md)

---

Status Codes
============

JSTP supports a vocabulary of Status Codes analog to that of [HTTP](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html), while greatly reduced in quantity due both to Status Codes being less prominent in JSTP and many HTTP scenarios not applying in JSTP.

JSTP Status Codes are used as the second element of the Resource Header in the context of an Answer Dispatch and are part of the [Answer Morphology](index.md#answer-morphology). For more discussion on the behaviour of the Answer Morphology see the [Answer instructions for the Engines](../engine.md#answer). 

The Status Codes represent the type of the answer. Unlike HTTP, JSTP does not send the message description of the Status Code in the Dispatch since is not intended for humans to read, but each Status Code has a description in the recommendation.

200: Ok
-------

The 200 
400 Bad Dispatch
401 Unauthorized
403 Forbidden
404 Not Found
405 Method Not Allowed
409 Unrecognized Transport Protocol
500 Internal Error
501 Not Implemented
502 Unreachable Remote Host
505 JSTP Version Not Supported
400 Bad Dispatch
----------------

- Code: 400
- Message: Bad Dispatch

The Bad Dispatch exception type should be sent when the processed Dispatch has one of the following problems:

- Some required headers are missing 
- The content of some required headers is missing or malformed
- The engine was unable to parse the JSON serialization
- Malformed Endpoint Header

Since in either case the Dispatch is assumed to be unrecoverable, no Method nor Resource should be sent in the Exception Dispatch.

> For the dis _todo_

> For tolerance to malformed Dispatches in the engine implementation please see section [6.2.2] Tolerance and Quirks Mode

404 Not Found
-------------

_todo_ 

406 Unbound Endpoint
--------------------

_todo_

409 Unrecognized Transport Protocol
-----------------------------------

This means that the Host Header features a Transport Protocol that is not supported by this implementation.

502 Not Gateway
---------------

_todo_

505 JSTP Version Not Supported
------------------------------

_todo_

---

[Back: Syntax](index.md)
