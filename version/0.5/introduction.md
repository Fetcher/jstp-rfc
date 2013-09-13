[Table of Contents](index.md) | [Next: Vocabulary](vocabulary.md)

---

Introduction
============

The JSON Transfer Protocol (JSTP) is an application-level communication protocol for distributed computation systems.

JSTP is designed to:

- Facilitate distributed computing with an abstract, high level API inspired in HTTP
- Standarize interactions among applications connecting with each other playing different roles either as clients or servers.

JSTP also aims to maintain quality standards and conventions of modern web applications:

- Is readily avaible to modern browsers via [WebSockets](http://en.wikipedia.org/wiki/WebSocket).
- JSTP is JSON, which makes it trivial to parse in modern programming languages.
- The method and resource API closely resembles that of [Representational State Transfer (REST)](https://en.wikipedia.org/wiki/Representational_state_transfer), so is very natural to adopt for web developers.
- Responses are sent asynchronously, thus making it fit for parallel processing and optimized for non-blocking, event-driven architectures.
- Messages ( _Dispatches_ ) are symmetrical, so every application manages resources with the same API, whether it is behaving as server or as client.
- Supports subscription ( _binding_ ) to events on remote hosts.
- Supports seamless reflective subscription.
- Arbitrary headers can be added with little to no hassle, making it easy to extend.

Hybrid distributed applications that connect server-side and user agent nodes can benefit greatly from using a single protocol accross the entire architecture.

Notation
--------

### [JSTP URI Scheme](https://github.com/jstp/jstp-uri)

JSTP Dispatches are designed from the bottom up to be as compatible with HTTP as possible, so JSTP resources can be represented as URLs. 

More information can be found in the [JSTP URI Scheme Specification](https://github.com/jstp/jstp-uri)

### HTTP-like syntax

> This notation is not normative and may not be supported in actual implementations.

Dispatches can also be represented in a similar fashion to HTTP Requests. 

The first sample Dispatch may be represented in an HTTP-like notation like this:

```
GET /foods/pizza JSTP/0.4
host: pizza.com.ar
timestamp: 1365647440759
token: 3434h5098asr34h3

message: Let the cheese melt!
```

This notation may be used for clarity since raw JSON can be messy. It can be used, for example, in log files. 

---

[Table of Contents](index.md) | [Next: Vocabulary](vocabulary.md)
