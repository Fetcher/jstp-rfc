[Table of Contents](../index.md) | [Previous: Vocabulary](../vocabulary.md) | [Next: Engine](../engine.md)

---

Syntax
======


JSTP Message: Dispatch
----------------------

The JSTP Dispatch is both the message object that gets sent over the network and the event that gets triggered in the destination application. When is sent over the network, the Dispatch is a JSON serialization: within the Engine, it may be represented as a Dispatch object according to the programming language conventions.

A Dispatch is conformed by a series of _Headers_. There are 8 supported Headers, but other, non-normalized Headers may be added. Engines should discard any headers that can't be understood.

### Headers

All JSTP native headers are case-insensitive, but it is strongly recommended for engines to send them in lowercase form.

- [**Protocol** Header](protocol.md)
- [**Method** Header](method.md)
  - [GET](method.md#get)
  - [POST](method.md#post)
  - [PUT](method.md#put)
  - [PATCH](method.md#patch)
  - [DELETE](method.md#delete)
  - [BIND](method.md#bind)
  - [RELEASE](method.md#release)
  - [ANSWER](method.md#answer)
- [**Resource** Header](resource.md)
- [**Timestamp** Header](timestamp.md)
- [**Token** Header](token.md)
- [**Host** Header](host.md)
- [**Body** Header](body.md)
- [**Endpoint** Header](endpoint.md)

### Extensions 

JSTP Headers can be extended just by adding the properties to the Dispatch object. There are currently no restrictions as to what can be added as an Extension, but it is still recommended not to crowd Dispatches with a lot of extra options and make use of the Body and Token headers instead.

Engines should disregard any unrecognized Extension Headers, although must not eliminate them so resources may use them.

> Modifications of the properties or structure of a native Header beyond the guidelines are illegal and will raise a [400 Bad Dispatch](exception.md#400-bad-dispatch) Exception.

Morphologies
------------

JSTP Dispatches have three possible morphologies characterized by their Methods and structure details:

- [Regular Morphology](#regular-morphology)
- [Subscription Morphology](#subscription-morphology)
- [Answer Morphology](#answer-morphology)

Each one is also characterized because the Dispatches of each Morphology are processed in a different fashion by the receiving Engine. 

### Regular Morphology

> Methods: **`GET`, `POST`, `PUT`, `DELETE`, `PATCH`**

The Regular Morphology has the following Headers:

- Protocol
- Method
- Resource
- Timestamp
- Host _(optional)_
- Token _(optional)_
- Referer _(optional)_
- Body _(optional)_

No special consideration are given to each Header outside the described in the [Headers section](#headers).

### Subscription Morphology

> Methods: **`BIND`, `RELEASE`**

The Subscription Morphology has the following Headers:

- Protocol
- Method
- Endpoint
- Timestamp
- Host _(optional)_
- Token _(optional)_
- Referer _(optional)_
- Body _(optional)_

The Endpoint is as described in the [Headers section](#headers). Note that there is no Resource Header: in fact, a Resource Header in a Subscription Dispatch is invalid and will cause a 400 Bad Dispatch in an Engine set to Strict Mode.

### Answer Morphology

> Methods: **`ANSWER`**

The Answer Morphology has the following Headers:

- Protocol
- Method
- Resource
- Timestamp
- Token _(optional)_
- Body _(optional)_

There are special considerations for the Resource Header:

1. The Resource Header can only have two items. 
2. The first item must be the Transaction ID of the Dispatch to which the Answer is responding.
3. The second item must be a [Status Code](status-code.md):
  - [200 Ok](status-code.md#200-ok)
  - [400 Bad Dispatch](status-code.md#400-bad-dispatch)
  - [401 Unauthorized](status-code.md#401-unauthorized)
  - [403 Forbidden](status-code.md#403-forbidden)
  - [404 Not Found](status-code.md#404-not-found)
  - [405 Method Not Allowed](status-code.md#405-method-not-allowed)
  - [500 Internal Error](status-code.md#500-internal-error)
  - [501 Not Implemented](status-code.md#501-not-implemented)
  - [502 Unreachable Remote Host](status-code.md#502-unreachable-remote-host)
  - [503 Service Unavailable](status-code.md#503-service-unavailable)
  - [504 Timeout](status-code.md#504-timeout)
  - [505 JSTP Version Not Supported](status-code.md#505-jstp-version-not-supported)


Note that the Referer is not as much illegal as irrelevant, because the Emitter of an Answer Dispatch is always known, but also because the actual Emitter of an Answer Dispatch is the Engine that called the resources subscribed to the original Dispatch.

> Note: the reason why the Answer Dispatch features a Resource instead of a special Header is so to maintain consistency with the Regular Morphology and to be easily described by an Endpoint. Answer are to be subscribed to by the requesting party, so reusing the regular subscription feature makes sense.

---

[Table of Contents](../index.md) | [Previous: Vocabulary](../vocabulary.md) | [Next: Engine](../engine.md)
