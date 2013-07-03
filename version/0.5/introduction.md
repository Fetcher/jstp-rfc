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
- Messages ( _Dispatches_ ) are symmetrical, so every application manages resources with the same API, whether be it behaving as server or as client.
- Supports subscription ( _binding_ ) to events on remote hosts.
- Supports seamless reflective subscription.
- Arbitrary headers can be added with little to no hassle, making it easy to extend.

Hybrid distributed applications that connect server-side and user agent nodes can benefit greatly from using a single protocol accross the entire architecture.

Example
-------

JSTP messages (called Dispatches) can adopt three alternative Morphologies.

### Regular Morphology

A regular JSTP looks like this:

```javascript
{
  "protocol":   ["JSTP", "0.4"],      // The protocol and version
  "method":     "GET",                // The method. Headers are case-insensitive but is customary to user uppercase
                                      // in this one after the HTTP convention
  "resource":   ["foods", "pizza"],   // The selected resource which is to be retrieved (since the method is GET)
  "timestamp":  1365647440759,        // The milliseconds since the 1970-01-01 00:00:00.000 (also known as the UNIX timestamp)
  "token":      ["3434h5098asr34h3"], // [optional] An UUID called the Transaction ID, used to track back Answer Dispatches
  "host":       ["pizza.com.ar"],     // [optional] The destination host to which the Dispatch is to be sent
  "body":       {                     // [optional] A message body
    "message": "Let the cheese melt!"       
  }
}
```

### Subscription Morphology

A Subscription Dispatch incorporates the Endpoint Header and has no Resource:

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

### Answer Morphology

A protocol-level, Answer Dispatch:

```javascript
{
  "protocol":   ["JSTP", "0.4"],
  "method":     "ANSWER",
  "resource":   [
      400,                            // The status code. Shamelessly borrowed from HTTP
      "238vs39598gweorit340trwgewrt", // The Transaction ID
      "4589sf34589ar453dth5467ugrte"  // [optional] The Triggering ID. May not be present in 400 or 406 errors
    ],                                
  "token":      ["3434h5098asr34h3"],
  "timestamp":  1365647440759,
  "body": { 
    "message": "Bad Dispatch"         // A payload of the Answer (optional)
  }
}
```

Notation
--------

JSTP Dispatches are designed from the bottom up to be as compatible with HTTP as possible. That's why they can be represented as URLs. 

### [JSTP URI Scheme](uri.md)

The first sample Dispatch may be represented as an URL like this:

    jstp://pizza.com.ar/foods/pizza

This notation is useful to quickly build JSTP Dispatches from user input, as writing down the JSON by hand can be quite a burden.

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
