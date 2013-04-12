[Table of Contents](index.md) | [Next: Terminology](terminology.md)

---

Introduction
============

The JSON Transfer Protocol (JSTP) is an application-level communication protocol for distributed computation systems.

JSTP is designed to:

- Facilitate distributed computing with an abstract, high level API inspired in HTTP
- Standarize interactions among applications connecting with different roles either as clients or servers

JSTP also aims to maintain quality standards and conventions of modern web applications:

- JSTP is JSON, which makes it trivial to parse in modern programming languages.
- The method and resource API closely resembles that of [Representational State Transfer (REST)](https://en.wikipedia.org/wiki/Representational_state_transfer), so is very natural to adopt for web developers.
- Responses are sent asynchronously, thus making it fit for parallel processing and optimized for non-blocking, event-driven architectures.
- Messages ( _Dispatches_ ) are symmetrical, so every application manages resources with the same API, whether be it behaving as server or as client.
- Supports subscription ( _binding_ ) to events on remote hosts.
- Supports seamless reflective subscription.
- Arbitrary headers can be added with little to no hassle, making it easy to extend.

Example
-------

A regular JSTP looks like this:

```javascript
{
  "protocol":   ["JSTP", "0.4"],      // The protocol and version
  "method":     "GET",                // The method. Headers are case-insensitive but is customary to user uppercase
                                      // in this one after the HTTP convention
  "resource":   ["foods", "pizza"],   // The selected resource which is to be retrieved (since the method is GET)
  "timestamp":  1365647440759,        // The milliseconds since the 1970-01-01 00:00:00.000 (also known as the UNIX timestamp)
  "token":      ["3434h5098asr34h3"], // [optional] Just some id which the server will probably use to identify this request
  "host":       ["pizza.com.ar"],     // [optional] The destination host to which the Dispatch is to be sent
  "body":       {                     // [optional] A message body
    "message": "Let the cheese melt!"       
  }
}
```

### Subscription Dispatch example

A Subscription Dispatch looks slightly different:

```javascript
{
  "protocol":   ["JSTP", "0.4"],
  "method":     "BIND",               // I bind myself to this event
  "endpoint": {                       // The endpoint header carries a pattern which represents possible Dispatches
    "method":   "POST",               // The type of dispatches I'd like to subscribe to. It can be replaced with a wilcard (*)
    "resource": ["foods", "*"]        // The resource. The second element is a wildcard representing "anything"
  },
  "timestamp":  1365647440759,
  "token":      ["3434h5098asr34h3"],
  "host":       ["pizza.com.ar"]      // The host is still optional. I can perfectly subscribe for dispatches 
                                      // issued in this very engine  
}
```

### Exception Dispatch example

A protocol-level, Exception Dispatch:

```javascript
{
  "protocol":   ["JSTP", "0.4"],
  "timestamp":  1365647440759,
  "token":      ["3434h5098asr34h3"],
  "exception": {                      // The exception header contents the information about the error
    "code": 400,                      // The status code. Shamelessly the same as the HTTP ones
    "message": "Bad Dispatch"         // A description of the error
  }
}
```

Notation
--------

JSTP Dispatches are designed from the bottom up to be as compatible with HTTP as possible. That's why they can be represented as URLs. They can also be represented in a similar fashion to HTTP Requests. 

> These notations are not normative and may not be supported in actual implementations.

### URL

The first sample Dispatch may be represented as an URL like this:

    jstp://pizza.com.ar/foods/pizza

This notation is useful to quickly build JSTP Dispatches from user input, as writing down the JSON by hand can be quite a burden.

### HTTP-like syntax

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

[Table of Contents](index.md) | [Next: Terminology](terminology.md)
