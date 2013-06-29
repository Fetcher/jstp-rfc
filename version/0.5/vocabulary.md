[Table of Contents](index.md) | [Previous: Changes since the previous version](changes.md) | [Next: Syntax](syntax/index.md)

---

Vocabulary
==========

[**Dispatch**](syntax/index.md): A JSTP message. Dispatches are both JSON serializations and the data structures representing them in the applications.

[**Engine**](engine.md): An implementation of a JSTP client-server. Since JSTP is symmetric in design, implementations should be capables of both roles (thus _server_ being a misleading designation). Implementations may however be limited to one role due to the runtime enviroment (for example while in a web browser). Engine can also refer to an running instance of a client-server compliant with this specification.

**Emitter**: The generator of the Dispatch. If the Dispatch passed through a series of Gateways, it refers to the original generator. The Emitter can reside in the same application as the Engine where the Dispatch is being processed.

**Morphology**: A configuration of the Headers of a JSTP Dispatch designed to fulfill an specific purpose with differentiated Engine mechanics. Can be seen as a dialect of subset of JSTP.

**Callback**: A programming routine (typically an object's method) to be called by a JSTP Engine when the Endpoint to which the Callback is Subscribed gets triggered by a Dispatch in process.

**Subscription**: The binding of an Emitter-provided Callback to an Endpoint. The Subscription is created or terminated with Subscription Dispatches.

**Subscription Table**: The list of all current Subscriptions in a certain Engine. 

**Answer Dispatch**: An Engine-generated Dispatch that represents a message to the original Emitter of a Dispatch to which this new one is responding. It is used primarily by Emitters to ensure reception and correct processing of their Dispatches. 

**Transaction ID**: A [UUID](http://en.wikipedia.org/wiki/Universally_unique_identifier) identifying the generated Dispatch. When used, it must be the first element of the Token Header. 

**Application**: the program in whose process the Engine runs. In a broader sense may refer to the whole application topology of a distributed system using JSTP.

**Dispatch processing**: The actual processing of a Dispatch, which usually implies triggering the correponding events.

**Dispatch forwarding**: Whenever an Engine finds that a Dispatch should be sent to another application, the Dispatch is forwarded to be processed in the remote application.

[**Method**](syntax/method.md): The action to be performed on the identified resource. It can be any of the valid method verbs: [GET](syntax/method.md#get), [POST](syntax/method.md#post), [PUT](syntax/method.md#put), [PATCH](syntax/method.md#patch), [DELETE](syntax/method.md#delete), [BIND](syntax/method.md#bind) or [RELEASE](syntax/method.md#release).

[**Resource**](syntax/resource.md): The address of an actionable resource within the target application.

**Host**: The computer in which the engine is running. Not to be confused with the [Host Header](syntax/host.md).

**JSTP Transport Protocol**: The base network protocol that is used to send JSTP Dispatches over a network. Currently the alternatives are TCP sockets or Websockets. JSTP can be transported in other lower-level protocol provided it is full duplex.

**Client**: An Engine starting a network connection to another Engine in a Transport Protocol. And Engine is instructed to do this by the data in the Host Header of a Dispatch (typically resolving the given address through DNS or IP).

**Server**: An Engine attached to a server listening a port for a Transport Protocol. Note that the Engine can behave also as a Client and the Engine is not the Server per se but it is just attached to it.

**Virtual Host**: A target Engine that is described within an application using a domain name like address, but resides in the same application as the Emitter and does not require a network connection to be reached. Such a setup is extremely useful to keep concerns separated, since developers may want greater degree of control over the context in which the Dispatch is processed. Virtual Hosts provide the isolation benefits of different Engines without paying the penalties associated with network usage.

**Inbound**: A JSTP Transport Protocol connection where the current engine has the server role.

**Outbound**: A JSTP Transport Protocol connnection where the current engine has the client role.

**Header**: Any of the top level properties of the Dispatch object. For example: `protocol`, `method`, `body`...

[**Endpoint**](syntax/endpoint.md): A pattern describing a series of possible Method Header and Resource Header combinations. The Endpoint mechanic is deeply inspired in REST API Endpoints. Endpoints are a Method and Resource pair where the method and parts of the Resource may be replaced by wildcards. The JSTP Endpoints are formalized and are used in subscriptions.

**Endpoint triggering**: Whenever a Dispatch is processed the Engine calls the Callbacks provided in the Subscriptions of the endpoints that match the Dispatch.

**Application Level Response**: A Regular Dispatch generated by an Emitter after another Dispatch triggered a Subscription by the same Emitter. The second Dispatch is said to be an Application Level Response to the first one and it is not formalized nor required.

**Protocol Level Response**: An Answer Dispatch.

---

[Table of Contents](index.md) | [Previous: Changes since the previous version](changes.md) | [Next: Syntax](syntax/index.md)
