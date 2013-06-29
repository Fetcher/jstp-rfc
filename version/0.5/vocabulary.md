[Table of Contents](index.md) | [Previous: Changes since the previous version](changes.md) | [Next: Syntax](syntax/index.md)

---

Vocabulary
==========

[**Dispatch**](syntax/index.md): A JSTP message. Dispatches are both JSON serializations and the data structures representing them in the applications.

[**Engine**](engine.md): An implementation of a JSTP client-server. Since JSTP is symmetric in design, implementations should be capables of both roles (thus _server_ being a misleading designation). Implementations may however be limited to one role due to the runtime enviroment (for example while in a web browser). Engine can also refer to an running instance of a client-server compliant with this specification.

**Morphology**: A configuration of the Headers of a JSTP Dispatch designed to fulfill an specific purpose with differentiated Engine mechanics. Can be seen as a dialect of subset of JSTP.

**Application**: the program to whose resources the Engine has access. In a broader sense may refer to the whole application topology of a distributed system.

**Emitter**: The application from which the current Dispatch in process came. If the Dispatch passed through a series of Gateways, it refers to the original application to create the Dispatch. The emitter application can be the same where the Dispatch is being processed.

**Dispatch processing**: The actual processing of a Dispatch, which usually implies triggering the correponding events.

**Dispatch forwarding**: Whenever an Engine finds that a Dispatch should be sent to another application, the Dispatch is forwarded to be processed in the remote application.

[**Method**](syntax/method.md): The action to be performed on the identified resource. It can be any of the valid method verbs: [GET](syntax/method.md#get), [POST](syntax/method.md#post), [PUT](syntax/method.md#put), [PATCH](syntax/method.md#patch), [DELETE](syntax/method.md#delete), [BIND](syntax/method.md#bind) or [RELEASE](syntax/method.md#release).

[**Resource**](syntax/resource.md): The address of an actionable resource within the target application.

**Host**: The computer in which the engine is running. Not to be confused with the [`host` Header](syntax/host.md).

**Transport protocol**: TCP sockets or Websockets. JSTP can be transported in various lower-level protocols.

**Client**: An engine making a request to an application identified by its host (typically resolving its address through DNS or just using the IP).

**Inbound**: A JSTP connection where the current engine has the server role.

**Outbound**: A JSTP connnection where the current engine has the client role.

**Header**: Any of the top level properties of the Dispatch object. For example: `protocol`, `method`, `body`...

**Scalar**: a non-empty string, boolean or numeral value.

[**Endpoint**](syntax/endpoint.md): A description of a method/resource pair where both the method may be replaced by a wildcard and the resource may be replaced partially ot totally by a wilcard. The endpoint is a pattern representing the ocurrence or a series of Dispatches. It is normalized and it is applied in subscriptions.

**Subscription**: The binding of an application resource which wants to receive the dispatches processed in this engine or a remote one. 

**Endpoint triggering**: Whenever a Dispatch is processed the endpoints that match the Dispatch are triggered. If there are clients (internal or remote) subscribed to any of those endpoints, the Dispatch gets forwarded into them.

**Protocol-level**: A Dispatch that represents a message to the Engine, most oftenly an Exception, although Subscription Dispatches can be seen as being partly Protocol-level.

---

[Table of Contents](index.md) | [Previous: Changes since the previous version](changes.md) | [Next: Syntax](syntax/index.md)
