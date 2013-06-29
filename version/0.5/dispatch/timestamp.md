[Dispatch](index.md) | [Previous: Resource Header](resource.md) | [Next: Token Header](token.md)

---

Timestamp Header
================

- Type: Long Integer
- Element: The time when the Dispatch was issued in the UNIX time stamp format
- _Required_

The Timestamp header is a mandatory header that represents the time when the Dispatch was generated. Its value must be the [UNIX timestamp](http://en.wikipedia.org/wiki/Unix_time) in its _long integer_ form. Its rationale is to simplify logging, synchronization and processing. 

Since the Timestamp represents the time of generation, gateways must not change the timestamp. This will allow, among other things, to test application-level latency when the Dispatch is sent over a network.

The Timestamp is meant to be used by applications in order to set the creation or modification time of the affected resources that result of Dispatch processing.
 
---

[Dispatch](index.md) | [Previous: Resource Header](resource.md) | [Next: Token Header](token.md)
