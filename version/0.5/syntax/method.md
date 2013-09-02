[Dispatch](index.md) | [Previous: Protocol Header](protocol.md) | [Next: Resource Header](resource.md)

---

Method Header
=============

The Method Header is _required_ and has the same structure in all three Morphologies.

The Method Header must be a JSON `string`. The `string` content must be one of the eight valid [Methods](#methods) in either upper or lower case.

The method header represents the action to be performed on the identified resource.

> JSTP methods are much like HTTP methods, in fact JSTP supports almost all of them. If you are a REST developer, most of this will be familiar.

Methods
-------

The JSTP Method determines the Dispatch Morphology. In that sense, methods can be categorized as belonging to the [_Regular Morphology_](#regular-morphology), the [_Subscription Morphology_](#subscription-morphology) or the [_Answer Morphology_](#answer-morphology).

### Regular Morphology

Regular Morphology Methods (_Regular Methods_) deal with creating, querying, updating and destroying data. 

#### GET

The GET method represents the request of data about the Resource. This is most obvious in an application providing access to a filesystem, where a Resource such as `["books"]` may return the listing of a `books` directory in the applications host.

Another example would be to retrieve a record from a database: a GET Dispatch towards Resource `["articles", 356]` may retrieve the record with id `356` from the `articles` collection in the database.

Unlike HTTP, JSTP GET may include a body: since there is no query string, conditions on the request need to be sent in the body. 

--- 
CONTINUE
---

### POST

The POST method represents the directive to create a new record within the selected Resource. It usually is completed with a Body with data to be inserted in the newly created record.

Following the previous example, a POST Dispatch may be issued to the Resource `["articles"]` with the Body `{"text": "New article"}` and the application may create a record with the given text. 

### PUT

The PUT method represents the operation of inserting a record in the selected Resource. Unlike POST that selects the resource within which the record should be created, the Resource in a PUT Dispatch represents the exact address where the record should be created in the receiving application. The sent Body should replace any existing records in the Resource address.

A PUT Dispatch may or may not feature a Body.

For example, a PUT Dispatch with Resource `["links", 54]` and Body `{"href":"google.com"}` represents the directive to the application to create or update the record referenced by `["links", 54]` to contain the Body.

### PATCH

The PATCH method represents the directive to update one or more properties of the selected Resource. 

For example, upon processing the following Dispatch:

```javascript
{
  "protocol": ["JSTP", "0.4"],
  "method": "PATCH",
  "resource": ["papers", "physics", "quantum-mechanics"],
  "timestamp": 1363410000,
  "token": ["4sF45fRtra"],
  "body": {
    "higgs-boson-existence": true
  }
}
``` 

an application should update the `["papers", "physics", "quantum-mechanics"]` so that `higgs-boson-existence` is now equal to `true`.

### DELETE

The DELETE method represents the directive to destroy the selected Resource. 

### BIND

The BIND method represents the protocol-level directive to bind the emitting application to be notified upon the Processing of a Dispatch with a certain Method/Resource combination, as defined by the Endpoint pattern in the BIND Dispatch Header.

BIND Dispatches must carry an [Endpoint Header](endpoint.md) containing the pattern to which the Emitter is to be bound. The Emitter can be the application running the Engine or some Remote application.

BIND Dispatches may have a Body, since a previously bound Endpoint could have been configured to be triggered by the BIND Dispatch and may use the Body for some purpose. When used this way, the BIND method represents a session initialization for the Resources matched by the Endpoint, and its RELEASE counterpart, the finalization of the session.

> For further details refer to the [Subscription](../engine.md#subscription) section.

### RELEASE

The RELEASE method represents the protocol-level directive to unbind the emitting appliation from the Endpoint. 

RELEASE Dispatches must carry an [Endpoint Header](endpoint.md) with the pattern to be unbound from. After a RELEASE Dispatch is processed, the Emitter will no longer be triggered when a Dispatch matching the Endpoint pattern is processed.

The RELEASE Dispatch Endpoint may match no existing Subscription. If that's the case, the Engine should send an [406 Unbound Endpoint Exception](exception.md#406-unbound-endpoint) Dispatch back to the Emitter. 

RELEASE Dispatches may carry a Body by the same rationale as BIND Dispatches.

> When a client disconnects from an Engine, the Engine will automatically create and process a Body-less RELEASE Dispatch for each of its active Subscriptions. This will prevent the subscriptions to be triggered when the client is not available. For further details refer to the [Subscription](../subscription.md) section.

### ANSWER

The ANSWER method represents the Answer to another Dispatch, identified by the Transaction ID as the second item in the Resource:

The Resource Header is formalized for Answer Dispatches, and must be construction with a [Status Code](status-code.md) as the first item, the Transaction ID of the Source Dispatch as the second item and, if there is one, the Triggering ID of the callback making the Answer as the third item. Engines should answer with a [400 Bad Dispatch](status-code.md#400-bad-dispatch) if the Answer Dispatch Resource is otherwise structured and with a [406 Not Acceptable](status-code.md#406-not-acceptable) if the Transaction ID or the Triggering ID are invalid.

> The Triggering ID shall not be present in 400 Bad Dispatch and 406 Not Acceptable Answers, because they are not processed but just sent directly to the provided Callback, if any.

---

[Dispatch](index.md) | [Previous: Protocol Header](protocol.md) | [Next: Resource Header](resource.md)
