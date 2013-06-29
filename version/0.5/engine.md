Engine
======

_todo_

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

Implementation guidelines
-------------------------

_todo_

### Version management

_todo_

### Tolerance and Quirks mode

(tolerate null on optional headers)
_todo_
