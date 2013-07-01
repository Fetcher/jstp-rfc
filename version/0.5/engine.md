[Table of Contents](index.md) | [Previous: Syntax](syntax/index.md)

---

Engine
======

The JSTP Engine is a running instance of an actual implementation of JSTP. Applications can run several Engines listening in an arbitrary amount of ports in several Transport Protocols. This flexibility is one of the features of JSTP.

An Engine must be able to perform certain basic tasks of a core functionality and offer a at least a minimal API for application resources to consume. Both the basic functionality and the minimal API that are required for Engines are described in this section, which should be taken as a reference by implementation developers.

#### API

For the API, the Engine is described as the `JSTPEngine` class, that JSTP libraries in object oriented languages should implement.

##### Dispatch

The object model of the Dispatch is a class called `JSTPDispatch`. The JSTPDispatch class should provide getter and setter methods for each of the Headers.

Processing
----------

**Dispatch Processing** is the intepretation by the Engine of the directives contained in a certain JSTP Dispatch. During the Processing the Engines (in this order): 

1. Determines the syntactic correctness of the Dispatch that the Emitter provided.
2. Determines whether or not the Dispatch is to be Forwarded, both over a network (and consequently in some of Transport Protocol) or to a Virtual Host.
3. Identifies if the Morphology of the Dispatch is of Subscription type. That being the case, either binds or releases the provided Subscription Callback in the Subscription Context.
4. If the Emitter provided an Answer Callback and emits immediatly a Subscription Dispatch to itself with the same Host Header as the Dispatch in process but aimed at the Answer Dispatch for the Transaction ID of this Dispatch.
5. If there was nothing in the Host Header, triggers the Dispatch. 

#### API

The `#dispatch` instance method of the Engine handles the processing of the Dispatch. The function prototype of `JSTPEngine#dispatch` is as follows:

```c
JSTPDispatch dispatch( JSTPDispatch dispatch [, Function callback [, Object context ]])
```

### 1. Validation

The first step is to validate the Dispatch sent by the Emitter. In strongly typed programming languages Dispatches may be already checked for compliance in the construction of the `JSTPDispatch` object; in more looosely type ones, the Engine may look for missing or malformed Headers.

The [Syntax](syntax/index.md) section of this reference specifies data types and restrictions for each Header.

If the validation fails the Engine must generate immediately an Answer Dispatch with the 400: Bad Dispatch Status Code. If the Dispatch comes from a Local Emitter  for the corresponding Transaction ID. The Engine must alg













---

Forwarding
----------

JSTP Engines are automatically gateways of JSTP Dispatches. Because in many distributed applications different parts of the system lie in different networks connected just by a public gateway or even using different Transport Protocols, JSTP integrates an strategy to make it simple to use Engines as gateways. In practice, applications connected using gateway-enabled JSTP Engines as if they were working in a Virtual Private Network.

Sending Dispatches over a network to a different application is called Forwarding, and can take two forms: Host Forwarding (client mode) and Reverse Forwarding (server mode).


### Host Forwarding

Host Forwarding refers to the ability of JSTP Engines to send the Dispatch over the network to a target Host. Hosts are discovered from the Dispatches Host Header: DNS addresses and IPs are valid values for a Host Header.

When the address of the first host in the Host Header is a remote Host (meaning the address does not resolve to a loopback), the Engine must attempt to send the Dispatch to that Host and must not do the Processing in the current application. A Engine behaving this way is known as a JSTP Host Gateway. Engines may adopt pooling strategies for maintaining a list of active Outbound connections.

The Host Forwarding gateway functionality is opt-out: it can be configured to be disabled in the Engines. This can be useful to enforce execution paths, avoid running into gateway cycles or as a security measure so that Dispatches cannot be issued to an internal server in the network.

### Reverse Forwarding

_todo_

Subscription
------------

Subscription refers to the attaching of a callback to an Endpoint pattern on a local or remote JSTP Engine. The subscription is issued via a BIND Dispatch with an Endpoint Header describing the pattern of Dispatches which, upon Processing, will trigger the subscribed callback.

All of callback in JSTP are added via subscription: so, for example, if a local resource wants to hook a callback on an Endpoint, it should be issue a BIND Dispatch with an empty, null, absent or a single loopback element in the Host Header. This is called Reflected Binding or Local Binding. 

> Engine implementations may provide a simple API such a Domain Specific Language in order to simplify Local Binding.

### Endpoints

_todo_

### Endpoint hierarchy

_todo_

### Subscription pool

_todo_

### Disconnect Release

_todo_

Answer
------

Networking
----------

(connection timeout??)
_todo_

### Transport protocols

_todo_

#### TCP Sockets

_todo_

#### Websockets

_todo_

### Environment

_todo_

Configuration
-------------

_todo_

### Version management

_todo_

### Tolerance and Quirks mode

(tolerate null on optional headers)
_todo_

API Extras
----------

1. Method shorthands

---

[Table of Contents](index.md) | [Previous: Syntax](syntax/index.md)
