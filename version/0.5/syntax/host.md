[Syntax](index.md) | [Previous: Token Header](token.md) | [Next: Body Header](body.md)

---

Host Header
===========

The Host Header is _optional_ in the Regular and Subscription Morphology and _illegal_ in the Answer Morphology.

The Host Header must be a JSON `array` containing one or more Destination Hosts.

If the Host Header `array` is empty, it should be discarded. `null` and empty `array` `elements` within the Host Header are illegal although Engines running in Quirks Mode may choose to ignore them.

### Destination Hosts

Each `element` in the Host Header `array` must be itself a JSON `array` with three `elements`: 

#### Host Address

The first element in a Destination Host must be a Host Address. It must be a JSON `string` representing the some kind of Internet Address—recommended values include [Domain Names](http://tools.ietf.org/html/rfc1034) and IP addresses, both [v4](http://tools.ietf.org/html/rfc791) and [v6](http://www.ietf.org/rfc/rfc2460.txt). Engines should support at least this three addressing protocols. JSTP is designed to run in an IPv4, IPv6 and DNS enabled environment.

#### Port Number

The second element in a Destination Host must be a Port Number.

--- 

**CONTINUE**

---


The host header represents the destination host of the Dispatch. It may resolve to a _loopback_ (such as 127.0.0.1 in IPv4 or ::1 in IPv6). Depending on the values of this header, there are several possible scenarios for the outcome of the Dispatch in the Engine:

Empty host
----------

If the host header is null or an empty string, the Dispatch must be processed in the current Engine. This means that the subscribed resources (local or remote) in the matching Endpoints will be triggered and the Dispatch forwarded to them.

The first host in the list resolves to loopback
-----------------------------------------------

If the host header has only one element and the address resolves to the host where the application is running, the engine should either remove the host header or at least empty it, and afterwars process the Dispatch as stated in the previous section.

If the host header has many several elements and the first address resolves to the host where the application is running, the engine should remove the first item from the list of hosts and proceed as described in the next section.

> Future versions of JSTP may support port selection. Some engines may implement a mechanism to parse a domain such as "localhost:77777" and use 77777 as the target JSTP engine port, but this is not normative.

One or more hosts
-----------------

If the host header has one or more hosts and the first element in the list does not resolve to loopback, the Engine must:

1. Remove the first element from the list of hosts
2. Attempt to connect to the destination host via the standard JSTP protocols (80, 33333) and send the Dispatch to the remote application

It comes naturally that if a series of hosts were specified in the header, each Engine will work as a gateway and forward the Dispatch to the next host until it reaches the final one. This is called the _Automatic Gateway_ and is one of JSTP features.

_Automatic Gateway_ makes the networks work like a series of VPNs, since each host may use a different domain server. This is quite useful when a client uses a public network and wants to access a resource inside a private one. 

One caveat: _Automatic Gateway_ introduces security concerns, so applications may choose to disable this feature. If the engine is configured to not forward Dispatches, it should answer the source engine with an Exception Dispatch with [`502 Not Gateway`](exception.md#502-not-gateway).

Virtual Host
------------

_todo_


If the host item has nor port neither protocol, it is assumed that it is referring to a Virtual Host.

---

[Syntax](index.md) | [Previous: Token Header](token.md) | [Next: Body Header](body.md)
