[Back: Syntax](index.md)

---

JSTP Status Codes
=================

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

Engines running in Quirks Mode may choose to ignore recoverable malformations that will otherwise merit a `400` Bad Dispatch Answer.

### 401: Unauthorized

The `401` Status Code represents an Application-Level indication that the Emitter is currently not authorized to request the processing of the Dispatch, but may if it provides the authentication credentials. There is currently no specification on how those credentials should be sent but this Status Codes remains distinct from `403` to indicate that although the Dispatch was rejected, credentials may allow the Emitter to be able to perform it successfully in the future.

This Status Code can only be issued by Callbacks.

### 403: Forbidden

The `403` Status Code represents the Application-Level indication that the selected resource is not allowed for the Emitter to act on. 

While this can be solved with credentials the `403` code is more drastic in that it does not suggest that the resource is actually accessible.

### 404: Not Found

The `404` Status Code represents the Protocol-Level indication that the selected resources triggered no subscription in the destination Engine, so nothing resulted from processing it.

It can only be issued by Engines.

### 405: Method Not Allowed

The `405` Status Code represents a specific type of Dispatch malformation: it represents the notification that the value of the Method Header does not match any of the allowed Methods. 

It can only be issued by Engines.

### 406: Unbound Endpoint

The `406` Status Code represents the Protocol-Level notification that the received RELEASE Dispatch does not match any current subscription. It should be issued by Engines when no subscription was matched by the RELEASE Dispatch. Since it is a Protocol-Level notification, it can only be issued by Engines.

### 409: Unrecognized Transport Protocol

The `409` Status Code represents the Protocol-Level notification to the Emitter that the [Transport Protocol Label](host.md#transport-protocol-label) as defined in the Host Header to be processed by this Engine does not match any valid Transport Protocol. 

It can only be issued by Engines.

5xx Range
---------

Status Codes in the `5xx` Range represent errors caused by the Callback or the Engine that processed the Dispatch. Connection problems and version incompatibilities are indicated with codes in this range.

### 500: Internal Error

The `500` Status Code represents the Application-Level notification that the execution of the actions described in the Source Dispatch have failed because of reasons unrelated to JSTP. 

It can only be issued by Callbacks.

### 501: Not Implemented

The `501` Status Code represents the Application-Level notification that the requested action is not yet supported by the receiver. It is an alternative notification to `404` where no subscription has been triggered: applications may subscribe to endpoints but notify that they have provided no support yet to the selected actions.

### 502: Not Gateway

---

**CONTINUE**

---


















502 Not Gateway
---------------

_todo_

505 JSTP Version Not Supported
------------------------------

_todo_

---

[Back: Syntax](index.md)
