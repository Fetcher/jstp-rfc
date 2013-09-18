[Back: Syntax](index.md)

---

Status Codes
============

JSTP supports a vocabulary of Status Codes analog to that of [HTTP](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html), while reduced in quantity due both to Status Codes being less prominent in JSTP and many HTTP scenarios not applying in JSTP.

JSTP Status Codes are used as the first element of the [Resource Header](resource.md#answer-morphology) in the context of an Answer Dispatch and are part of the [Answer Morphology](index.md#answer-morphology). 

The Status Codes represent the type of the Answer. Unlike HTTP, JSTP does not send the Status Code Description in the Answer Dispatch since it is not intended for humans to read, but each Status Code has a Description in the specification.

The Status Codes are furtherly categorized into Status Code Ranges. Each Range is analog to the corresponding HTTP Status Code range, but the `1xx` and `3xx` ranges are unsupported as of version `0.5` of JSTP and are left blank reserved for the future.

2xx Range
--------

Status Codes in this range represent the successful execution of the actions represented by the Source Dispatch.

### 200: Ok

The `200` Status Code represents the correct execution of the action represented by the Source Dispatch. 

It can only be issued by Callbacks.

4xx Range
---------

Status Codes in the `4xx` Range represent errors caused by the Emitter, either by misconfiguration or malformation of the Dispatch. 

> Many of the 4xx errors do not travel over a network because they are identified by the first Engine to process the broken Dispatch, and in some circunstances cannot be sent at all because a receiving Callback is missing. Instructions will be added for Engines to implement these cases in the [JSTP Engine](https://github.com/jstp/jstp-engine) specification.

### 400: Bad Dispatch

The `400` Status Code represents a Protocol-Level problem detected by the Engine while processing the Dispatch. Typically this involves missing or malformed Headers.

The `400` Status Code can only be issued by Engines. Usually it will be sent back to faulty Local Emitters that provided a Callback. When a broken Dispatch is received over a network, incorrect JSON encoding or missing Transaction IDs may prevent the receiving Engine to address the Answer Dispatch correctly: in such case, Engines should send no Answer Dispatch. 






















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
