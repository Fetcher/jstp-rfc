JSTP/0.4 - JSON Transfer Protocol version 0.4
=============================================

> This document is a work in progress. For discussion please refer to the [issue tracker](https://github.com/Fetcher/jstp-rfc/issues) on this repository or the [JSTP related articles in the Fetcher's Dev Blog](http://blog.getfetcher.net/tagged/jstp) .

> Versions [0.2](version/pseudo0.2.md) and [0.3](version/pseudo0.3.md) were not described but represent modular updates prior to version 0.4.

1 Introduction
--------------

JSTP is a communication protocol designed to:

- Facilitate distributed computing with an abstract, high level API inspired in HTTP
- Standarize interactions among applications connecting with different roles either as clients or servers

JSTP is also designed to be up to the paradigm of modern applications:

- JSTP is JSON, which makes it trivial to parse in modern programming languages.
- The API closely resembles REST, so is very natural to adopt for web developers.
- Responses are sent asynchronously, thus making it fit for parallel processing.
- Messages ( _dispatches_ ) are symmetrical, so every node manages resources with the same API, whether is a server or a client node.
- Supports subscription ( _binding_ ) to events on remote hosts.
- Supports seamless reflective message processing.
- Arbitrary headers can be added with little to no hassle, making it easy to extend.

### 1.1 Example

How does a regular JSTP Dispatch looks like?

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

#### 1.1.1 Subscription example

A subscription Dispatch looks slightly different:

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

#### 1.1.2 Exception example

A low level, exception Dispatch:

```javascript
{
  "protocol":   ["JSTP", "0.4"],
  "timestamp":  1365647440759,
  "token":      ["3434h5098asr34h3"],
  "exception": {                      // The exception header contents the information about the error
    "code": 400,                      // The status code. Shamelessly the same as the HTTP ones
    "message": "Bad Dispatch"         // A description the error
  }
}
```

### 1.2 Notation

JSTP Dispatches are designed from the bottom up to be as compatible with current industry standards as possible. That's why they can be represented as URLs. They can also be represented in a similar fashion to HTTP Requests. These notations are not normative and may not be supported in actual implementations.

#### 1.2.1 URL

The first sample Dispatch may be represented as an URL like this:

    jstp://pizza.com.ar/foods/pizza

This notation is useful to quickly build JSTP Dispatches from user input, as writing down the JSON by hand can be quite a burden.

#### 1.2.2 HTTP⁻like syntax

The first sample Dispatch may be represented in an HTTP-like notation like this:

```
GET /foods/pizza JSTP/0.4
host: pizza.com.ar
timestamp: 1365647440759
token: 3434h5098asr34h3

message: Let the cheese melt!
```

This notation may be used for clarity since raw JSON can be messy, for example it can be used in log files. 


2 Terminology
-------------

- Dispatch: A JSTP message. Dispatches are both JSON serializations and the data structures representing them in the applications.


- Engine: An implementation of a JSTP client-server. Since JSTP is symmetric in design, implementations should be capables of both roles (thus _server_ being a misleading designation). Implementations may however be limited to one role due to the runtime enviroment (for example while in a web browser). Engine can also refer to an running instance of a client-server compliant with this specification.

- Application: the program to whose resources the Engine has access. In a broader sense may refer to the whole application topology of a distributed system.

- Emitter: The application from which the current Dispatch in process came. If the Dispatch passed through a series of Gateways, it refers to the original application to create the Dispatch. The emitter application can be the same where the Dispatch is being processed.

- Dispatch processing: The actual processing of a Dispatch, which usually implies triggering the correponding events.

- Dispatch forwarding: Whenever an Engine finds that a Dispatch should be sent to another application, the Dispatch is forwarded to be processed in the remote application.

- Method: The action to be performed on the identified resource.

- Resource: The address of an actionable resource within the target application.

- Host: The computer in which the engine is running.

- Transport protocol: TCP sockets or Websockets. JSTP can be transported in various lower-level protocols.

- Client: An engine making a request to an application identified by its host (typically resolving its address through DNS or just using the IP).

- Inbound: A JSTP connection where the current engine has the server role.

- Outbound: A JSTP connnection where the current engine has the client role.

- Header: Any of the top level properties of the Dispatch object. For example: `protocol`, `method`, `body`...

- Scalar: a non-empty string, boolean or numeral value.

- Endpoint: A description of a method/resource pair where both the method may be replaced by a wildcard and the resource may be replaced partially ot totally by a wilcard. The endpoint is a pattern representing the ocurrence or a series of Dispatches. It is normalized and it is applied in subscriptions.

- Subscription: The binding of an application resource which wants to receive the dispatches processed in this engine or a remote one. 

- Endpoint triggering: Whenever a Dispatch is processed the endpoints that match the Dispatch are triggered. If there are clients (internal or remote) subscribed to any of those endpoints, the Dispatch gets forwarded into them.

- Exception: An exception is a protocol-level error that needs to be reported back to the source of the Dispatch. When an exception occurs, the engine should address an Exception Dispatch back to the source engine.

3 Dispatch
----------

The JSTP Dispatch is both the message object that gets sent over the network and the event that gets triggered in the destination application. When is sent over the network, the Dispatch is a JSON serialization: within the Engine, it may be represented as a Dispatch object according to the programming language conventions.

A Dispatch is conformed by a series of _Headers_. There are 9 supported Headers, but other, non-normalized Headers may be added. Engines should discard any headers that can't be understood.

### 3.1 Headers

> All headers are case-insensitive.

#### 3.1.1 Protocol

- Type: Array of Strings.
- Elements: 
  1. "JSTP" - A string stating that the Dispatch uses the JSTP protocol. This field is case insensitive.
  2. <version-number> - A string representing the JSTP version number.
- _Required_

The protocol header represents the JSTP protocol version that the emitting Engine is using, and subsequently the JSTP version that the Dispatch is written in. It consists of an array of strings, being the first "jstp" and the second the version number.

The version number must be used by the receiving Engine to process the Dispatch according to the specifications of the JSTP version. If the engine is unable to process the Dispatch version it must send an Exception Dispatch back to the source.

> The protocol header may be extended with further data to specify JSTP extensions.

#### 3.1.2 Method

- Type: String
- _Required_

The method header represents the action to be performed on the identified resource.

> JSTP methods are much like HTTP methods, in fact JSTP supports almost all of them. If you are a REST developer, most of this will be familiar.

##### Method types

JSTP Methods can be categorized as _Subscription_ and _Non-Subscription_ methods. Subscription methods include BIND and RELEASE. Non-Subscription methods are the others: GET, POST, PUT, DELETE and PATCH.

Non-Subscription methods deal with creating, querying, updating and destroying data. Any Non-Subscription Dispatch may be followed up by one or more PUT Dispatches sent from the target application back to the source application with the affected Resources in the Dispatch Body, but this is optional and left to the application's configuration.

GET, DELETE and PATCH methods in particular are known as Assuming Methods, in the sense that they assume the selected Resource to exist. An application may issue a protocol-level 404 Not Found Exception Dispatch back to the emitter in the case that the Resource is not available.

> This JSTP specification does not describe the subsequent Dispatches that an application may issue after a successful Non-Subscription Dispatch, but future updates might.  

###### 3.1.2.1 GET

The GET method represents a request of data about the Resource. This is most obvious in an application providing access to a filesystem, where a Resource such as `["books"]` may return the listing of the `books` directory in the applications host.

Another example would be to retrieve a record from a database: a GET Dispatch towards Resource `["articles", 356]` may retrieve the record with id `356` from the `articles` collection in the database.

Unlike HTTP, JSTP GET may include a body: since there is no query string, conditions on the request need to be sent in the body. 

###### 3.1.2.2 POST

The POST method represents the directive to create a new record within the selected Resource. It usually is completed with a Body with data to be inserted in the newly created record.

Following the previous example, a POST Dispatch may be issued to the Resource `["articles"]` with the Body `{"text": "New article"}` and the application may create a record with the given text. 

###### 3.1.2.3 PUT

The PUT method represents the operation of inserting a record in the selected Resource. Unlike POST that selects the resource within which the record should be created, the Resource in a PUT Dispatch represents the exact address where the record should be created in the receiving application. The sent Body should replace any existing records in the Resource address.

A PUT Dispatch may or may not feature a Body.

For example, a PUT Dispatch with Resource `["links", 54]` and Body `{"href":"google.com"}` represents the directive to the application to create or update the record referenced by `["links", 54]` to contain the Body.

###### 3.1.2.4 PATCH

The PATCH method represents the directive to update one or more properties of the selected Resource. 

For example, the following Dispatch:

```javascript
{
  "protocol": ["JSTP", "0.4"],
  "method": "PATCH",
  "resource": ["papers", "physics", "quantum-mechanics"],
  "timestamp": 1363410000,
  "token": 
}
``` 

###### 3.1.2.5 DELETE

_todo_

##### 3.1.2.6 BIND

_todo_

##### 3.1.2.7 RELEASE

_todo_

#### 3.1.3 Resource

- Type: Array of Scalars
- Elements: An arbitrarily long array containing non-empty scalars that represent the selected Resource.
- _Required_

The Resource is to be interpreted as a resource in the application. It can represent, for example:

- A file in a filesystem.
- A record in a database.
- A controller in a web application.

The resources can be accessed with several methods, so it may be possible to create, update, destroy or query resources via JSTP. Since JSTP is symmetrical and asynchronous, the possibilities are more diverse than CRUD (Crete, Read, Update, Delete), but still resources should represent as much as possible the range of actions in the application.

#### 3.1.4 Timestamp

- Type: Long Integer
- Element: The time when the Dispatch was issued in the UNIX time stamp format
- _Required_

The timestamp is a mandatory header. Its rationale is to simplify logging, synchronization and processing. 

Gateways should not change the timestamp. 

> As this draft, Exception Dispatches should not modify the Timestamp so that emitters can identify the faulty Dispatch; relying on the Timestamp is not robust enough and this should most likely be done using the Token header, but a standarization of the Token's elements has not been developed yet.

#### 3.1.5 Token

- Type: Array of Scalars/Null Objects
- Elements: An arbitrarily long array containing strings, numbers (float, integers, etc), boolean values and even null values
- _Optional_

The token header is intended to enable the representation of several items, which include (but are not limited to):

- Session
- Transaction
- Client
- Credentials
- Flags

For example, an application may keep track of the session token to identify the user, or pass around her credentials (such as a public key for asymmetric encryption). An application may also keep track of the transaction ID to identify the Dispatches that got generated after another Dispatch was issues: this subsequent Dispatches will then share the transaction ID of the original one. 

This header is specially useful in the context of a distributed application where Dispatches that represent requests start computation in different applications that issue Dispatches that represent replies to that request.

The specifics of the order, format and usage of the tokens are not defined in this specification.

#### 3.1.6 Host

- Type: Array of Strings
- Elements: An arbitrarily long array of DNS domains or IP addresses
- _Optional_

The host header represents the destination host of the Dispatch. It may resolve to a _loopback_ (such as 127.0.0.1 in IPv4 or ::1 in IPv6). Depending on the values of this header, there are several possible scenarios for the outcome of the Dispatch in the Engine:

##### 3.1.6.1 Empty host

If the host header is null or an empty string, the Dispatch must be processed in the current Engine. This means that the subscribed resources (local or remote) in the matching Endpoints will be triggered and the Dispatch forwarded to them.

##### 3.1.6.2 The first host in the list resolves to loopback

If the host header has only one element and the address resolves to the host where the application is running, the engine should either remove the host header or at least empty it, and afterwars process the Dispatch as stated in the previous section [3.1.6.2].

If the host header has many several elements and the first address resolves to the host where the application is running, the engine should remove the first item from the list of hosts and proceed as described in the next section [3.1.6.3].

> Future versions of JSTP may support port selection. Some engines may implement a mechanism to parse a domain such as "localhost:77777" and use 77777 as the target JSTP engine port, but this is not normative.

##### 3.1.6.3 One or more hosts

If the host header has one or more hosts and the first element in the list does not resolve to loopback, the Engine must:

1. Remove the first element from the list of hosts
2. Attempt to connect to the destination host via the standard JSTP protocols (80, 33333) and send the Dispatch to the remote application

It comes naturally that if a series of hosts were specified in the header, each Engine will work as a gateway and forward the Dispatch to the next host until it reaches the final one. This is called the _Automatic Gateway_ and is one of JSTP features.

_Automatic Gateway_ makes the networks work like a series of VPNs, since each host may use a different domain server. This is quite useful when a client uses a public network and wants to access a resource inside a private one. 

One caveat: _Automatic Gateway_ introduces security concerns, so applications may choose to disable this feature. If the engine is configured to not forward Dispatches, it should answer the source engine with an Exception Dispatch with `502 Not Gateway` as detailed in section 3.1.9.3.

#### 3.1.7 Body

- Type: Any JSON supported type.
- _Optional_

The body is the payload of the Dispatch. Its optional and can be issued with any method.

Its contents may vary: it can be a hash, an array, a scalar and even a null value. 

#### 3.1.8 Endpoint

- Type: Object
- Elements: `method` pattern with a string value and `resource` pattern with an array value.
- _Required_: With `BIND` and `RELEASE`
  _Illegal_: With all other Methods

The endpoint header is a special header that can only be sent with the BIND and RELEASE methods in what is called a Subscription Dispatch. When processed, a Subscription Dispatch binds or unbinds the Endpoint to the source of the Dispatch, so that it gets executed when a Dispatch matching the Endpoint is processed in the current Engine. This is further detailed in section [5] Subscription.

The endpoint structure has a `method` pattern and a `resource` pattern, which are similars to the Method and Resource headers of a regular Dispatch but differ in the syntax, as the properties of the endpoint header accept wildcards.

##### 3.1.8.1 Method pattern

The method pattern represents the pattern to the JSTP Methods that, in combination with the endpoint resource pattern, should trigger the events attached to the endpoint. 

The method pattern accepts as values all the methods and, additionally, the wildcard `*`. The wilcard represents _any method_: an Endpoint with a wildcard method will match any Dispatch whose Resource matches the resource pattern.

###### 3.1.8.1.1 Matching a Subscription Dispatch

When the method pattern is BIND, the Method in the Endpoint header of the future Dispatch that may trigger the event will be ignored: the endpoint will only match the Resource. 

For example, if the following Subscription Dispatch is issued to an Engine:

```javascript
{
  "protocol": ["JSTP", "0.4"],
  "method": "BIND",
  "timestamp": 1365647440759,
  "endpoint": {
    "method": "BIND",
    "resource": ["foods", "*"]
  }
}
```

any of the following Subscription Dispatches will trigger the event:

```javascript
{
  "protocol": ["JSTP", "0.4"],
  "method": "BIND",
  "timestamp": 1365647458760,
  "endpoint": {
    "method":"POST",
    "resource": ["foods", "*"]
  }
}
```

and

```javascript
{
  "protocol": ["JSTP", "0.4"],
  "method": "BIND",
  "timestamp": 1365647458760,
  "endpoint": {
    "method":"GET",
    "resource": ["foods", "*"]
  }
}
```
and

```javascript
{
  "protocol": ["JSTP", "0.4"],
  "method": "BIND",
  "timestamp": 1365647458760,
  "endpoint": {
    "method":"*",
    "resource": ["foods", "*"]
  }
}
```

and even


```javascript
{
  "protocol": ["JSTP", "0.4"],
  "method": "BIND",
  "timestamp": 1365647458760,
  "endpoint": {
    "method":"BIND",
    "resource": ["foods", "*"]
  }
}
```

[So meta](http://xkcd.com/917/).

> While subscribing to a subscription method may seem too _meta_, it is indeed quite useful. For example, an internal application may bind itself in a gateway application into the BIND method in a user session resource in the gateway, so when the user logs in, the user agent binds itself in the gateway and the internal application is subsequently notified and starts processing data that should be sent to the user.

> Future versions of JSTP may restrict the meta-subscription in order to avoid running into avoidable infinite loops. Engine implementations may implement a mechanism to do this, but no Exception type is defined for the eventuality in this specification.

##### 3.1.8.1 Resource pattern

The resource pattern is quite similar to a Resource header, but accepts two special wildcards as elements:

- `*`: The asterisk wildcard may appear alone as an element in the resource pattern and matches any value on that position of the Resource. For example, the resource pattern `["drinks", "*"]` will match both the Resources `["drinks", "water"]` and `["drinks", "beer"]` and any other with "drinks" as first element and a second element. 
- `...`: The ellipsis wildcard (WARNING: not the special punctuation ellipsis character, but the ellipsis formed with three consecutive dots) may appear as the last element in the pattern and represents any amount of elements afterwards. For example, the pattern `["drinks", "..."]` will match both the Resources `["drinks", "soda"]` and `["drinks", "coke", "juice"]`. The ellipsis wildcard should be ignored when not used in the last position.

String elements in the resource pattern will be matched against equal string, case sensitively. 

The backslash character `\` escapes the pattern. For example `\*` will match exactly a `*`. `\...` will match exactly a `...`. In order to match the string `\*` the backslash must be escaped in the pattern, thus making it `\\*`. In the same way the `\...` string can be matched with the pattern `\\...`.

> Future versions of JSTP may support the ellipsis wildcard in different positions.

#### 3.1.9 Exception

- Type: Object
- Elements: `code` integer and `message` string

The exception header represents the description of a protocol level exception that prevented the Dispatch from being processed altogether. A Dispatch featuring an Exception header is called an Exception Dispatch and it should be returned to the source of the Dispatch whenever is possible. 

An Exception Dispatch may or may not have a Method and a Resource depending on the type of the exception. 

The code property represents the code of the exception and the message represents a human-readable description of the exception.

> The code property values have mostly been borrowed from HTTP so to lower the entrance barrier for web developers to JSTP.

##### 3.1.9.1 400 Bad Dispatch

- Code: 400
- Message: Bad Dispatch

The Bad Dispatch exception type should be sent when the processed Dispatch has one of the following problems:

- Some required headers are missing 
- The content of some required headers is missing or malformed
- The engine was unable to parse the JSON serialization

Since in either case the Dispatch is assumed to be unrecoverable, no Method nor Resource should be sent in the Exception Dispatch.

> For the dis _todo_
> For tolerance to malformed Dispatches in the engine implementation please see section [6.2.2] Tolerance and Quirks Mode

##### 3.1.9.2 404 Not Found

_todo_ 

##### 3.1.9.3 502 Not Gateway

_todo_

##### 3.1.9.4 505 JSTP Version Not Supported

_todo_

### 3.2 Extensions

_todo_

4 Forwarding
------------

_todo_

5 Subscription
--------------

_todo_

### 5.1 Endpoints

(pattern for "any amount of this"? such as ["foods", "..."])

### 5.2 Subscription pool

6 Engine
--------

_todo_

### 6.1 Networking

(connection timeout??)
_todo_

#### 6.1.1 Transport protocols

_todo_

##### 6.1.1.1 TCP Sockets

_todo_

##### 6.1.1.2 Websockets

_todo_

#### 6.1.2 Environment

_todo_

### 6.2 Implementation guidelines

_todo_

#### 6.2.1 Version management

_todo_

#### 6.2.2 Tolerance and Quirks mode

(tolerate null on optional headers)
_todo_

7 Changes since previous versions
---------------------------------

(referer out)

(gateway out)

(host-as-first-item out)

(new verbs and syntax for the new verbs)

(endpoind header in)

8 Future
--------

8.1 Known issues

(may be the dispatches are too long: what abount BSON?)

8.1(other methods: options and head)

(subscribe to remote hosts: bind tunnel)

(jsstp: jstp secure)

(support for integers in resources)

9 Acknowledgements
------------------

### 5.1 Collaborators

- [Fernando Vía Canel](https://github.com/xaviervia)
- [Luciano Bertenasco](https://github.com/lbertenasco)
- [Matías Domingues](https://github.com/mannias)

10 License
----------

Copyright (C) 2013 Fetcher & Collaborators

    This program is free software: you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation, either version 3 of the License, or
    (at your option) any later version.

    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

    You should have received a copy of the GNU General Public License
    along with this program.  If not, see <http://www.gnu.org/licenses/>.