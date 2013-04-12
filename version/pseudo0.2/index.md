JSTP/0.2 - JSON Transfer Protocol pseudo version 0.2
====================================================

> This document is a clarification, not a recommendation, and should not be used as reference for the development of JSTP Engines. Refer to the [latest JSTP version](jstp-latest.md) instead.

Changes in version 0.2
----------------------

### The `gateway` header

Version 0.2 introduced the `gateway` header, which provided directives to the Engine about Dispatch forwarding. There were three possible values:

- `normal`: If the Dispatch contained a valid Host as the first element in the `resource` field, the Dispatch had to be forwarded to the target host.
- `reverse`: If the Dispatch contained an `token` header which first element represented a valid inbound connection into the Engine, the Dispatch had to be sent over that inbound connection and to the client represented by the `token`. 
- `none`: The Dispatch is not to be forwarded to any other host.

The main aim of the changes introduced in v/0.2 was to make explicit the gateway functionality that was implemented in practice in the early Ruby implementation, and to stub the subsequent subscription able architecture. The `gateway` header has been completely overriden in version 0.4 and is no longer part of the protocol.