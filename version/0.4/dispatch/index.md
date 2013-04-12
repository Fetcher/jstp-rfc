[Table of Contents](../index.md) | [Previous: Terminology](../terminology.md) | [Next: Forwarding](../forwarding.md)

---

Dispatch
========

The JSTP Dispatch is both the message object that gets sent over the network and the event that gets triggered in the destination application. When is sent over the network, the Dispatch is a JSON serialization: within the Engine, it may be represented as a Dispatch object according to the programming language conventions.

A Dispatch is conformed by a series of _Headers_. There are 9 supported Headers, but other, non-normalized Headers may be added. Engines should discard any headers that can't be understood.

Headers
-------

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
- [**Resource** Header](resource.md)
- [**Timestamp** Header](timestamp.md)
- [**Token** Header](token.md)
- [**Host** Header](host.md)
- [**Body** Header](body.md)
- [**Endpoint** Header](endpoint.md)
- [**Exception** Header](exception.md)
  - [400 Bad Dispatch](exception.md#400-bad-dispatch)
  - [404 Not Found](exception.md#404-not-found)
  - [502 Not Gateway](exception.md#502-not-gateway)
  - [505 JSTP Version Not Supported](exception.md#505-jstp-version-not-supported)

Extensions
----------

JSTP Headers can be extended just by adding the properties to the Dispatch object. There are currently no restrictions as to what can be added as an Extension, but it is still recommended not to crowd Dispatches with a lot of extra options and make use of the Body and Token headers instead.

Engines should disregard any unrecognized Extension Headers, although must not eliminate them so resources may use them.

> Modifications of the properties or structure of a native Header beyond the guidelines are illegal and will raise a [400 Bad Dispatch](exception.md#400-bad-dispatch) Exception.

---

[Table of Contents](../index.md) | [Previous: Terminology](../terminology.md) | [Next: Forwarding](../forwarding.md)
