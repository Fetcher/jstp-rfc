JSTP/0.3 - JSON Transfer Protocol pseudo version 0.3
====================================================

> This document is a clarification, not a recommendation, and should not be used as reference for the development of JSTP Engines. Refer to the [lastest JSTP version](jstp-latest.md) instead.

Changes in version 0.3
----------------------

### 1. The `host` header

Version 0.3 introduced the `host` header, which represents the destination host of the Dispatch. Prior to version 0.3 the host was just a convention and was implied to be the first element in the `resource` header.

The `host` header was a header of type `Array`. Each element represents a destination node. A JSTP Engine can face three possible scenarios when processing a Dispatch:

#### 1.1 The `host` header is empty (or not present)

Whenever there is no destination host, the Engine process the Dispatch on itself instead of forwarding it. This can happen if the Engine was the final destination of a Dispatch sent over the network or if a module within this host created the Dispatch to aim a resource in the same host.

#### 1.2 The `host` header has just one element and resolves to 127.0.0.1 (::1)

> Implied: JSTP relies on DNS to name resolution. The loopback IP address indicates that the target host is itself.

> Currently the JSTP protocol does not explicitly support outbound protocol specification in the Dispatch. It is possible however for an Engine to intepret liberally the host element and parse a pseudo url such as `google.com:33333` using the digits after the colon as the destination host port.

If the only target is the host where the Engine is running, it should remove it and proceed as in section 1.1.

#### 1.3 The `host` header has one or more elements

The Engine should:

1. Resolve the destination address and initialize a connection or get one active from the outbound connection pool.
2. Remove the first entry from the `host` field
3. Send the Dispatch to the target host.

If the first entry in the `host` field resolves to the loopback address and there are more elements afterwards, the recommendation now is to discard that entry and proceed with the second one as stated above.