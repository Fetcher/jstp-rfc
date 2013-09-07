[Syntax](index.md) | [Previous: Resource Header](resource.md) | [Next: Token Header](token.md)

---

Timestamp Header
================

The Timestamp Header is _required_ and has the same structure in all the three Morphologies.

The Timestamp Header must be a JSON `number`, representing the milliseconds elapsed since 1970-01-01 00:00:00.000 UTC, also known as the [UNIX Epoch](http://en.wikipedia.org/wiki/Unix_time). The value must represent the time of generation of the Dispatch and should be setted by the Emitters. 

The Timestamp Header is there to provide a temporal dimension to Dispatches, since timing if of significance in distributed applications where messages may transit through various channels, and also because message creation time has proved to be an ubiquously useful datum for modern applications that deal with creation, maintainance and removal of resources.

Engines behaving as Gateways must not alter the timestamp of the Dispatches when forwarding them to the next Host. The timestamp should provide an anchor to retrace a certain call through the network to simplify debugging, profiling and logging.

---

[Syntax](index.md) | [Previous: Resource Header](resource.md) | [Next: Token Header](token.md)
