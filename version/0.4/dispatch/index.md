Dispatch
========

The JSTP Dispatch is both the message object that gets sent over the network and the event that gets triggered in the destination application. When is sent over the network, the Dispatch is a JSON serialization: within the Engine, it may be represented as a Dispatch object according to the programming language conventions.

A Dispatch is conformed by a series of _Headers_. There are 9 supported Headers, but other, non-normalized Headers may be added. Engines should discard any headers that can't be understood.

Headers
-------

> All headers are case-insensitive.

- [Protocol Header](protocol.md)

- [Method Header](method.md)

  - [GET](method.md#get)

  - [POST](method.md#post)

  - [PUT](method.md#put)

  - [PATCH](method.md#patch)

  - [DELETE](method.md#delete)

  - [BIND](method.md#bind)

  - [RELEASE](method.md#release)

- [Resource Header](resource.md)

- [Timestamp Header](timestamp.md)

- [Token Header](token.md)

- [Host Header](host.md)

- [Body Header](body.md)

- [Endpoint Header](endpoint.md)

- [Exception Header](exception.md)
  
  - [400 Bad Dispatch](exception.md#400-bad-dispatch)

  - [404 Not Found](exception.md#404-not-found)

  - [502 Not Gateway](exception.md#502-not-gateway)

  - [505 JSTP Version Not Supported](exception.md#505-jstp-version-not-supported)

Extensions
----------

_todo_

