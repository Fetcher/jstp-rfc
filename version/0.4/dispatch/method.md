Method Header
=============

- Type: String
- _Required_

The method header represents the action to be performed on the identified resource.

> JSTP methods are much like HTTP methods, in fact JSTP supports almost all of them. If you are a REST developer, most of this will be familiar.

Method types
------------

JSTP Methods can be categorized as _Subscription_ and _Non-Subscription_ methods. Subscription methods include BIND and RELEASE. Non-Subscription methods are the others: GET, POST, PUT, DELETE and PATCH.

Non-Subscription methods deal with creating, querying, updating and destroying data. Any Non-Subscription Dispatch may be followed up by one or more PUT Dispatches sent from the target application back to the source application with the affected Resources in the Dispatch Body, but this is optional and left to the application's configuration.

GET, DELETE and PATCH methods in particular are known as Assuming Methods, in the sense that they assume the selected Resource to exist. An application may issue a protocol-level 404 Not Found Exception Dispatch back to the emitter in the case that the Resource is not available.

> This JSTP specification does not describe the subsequent Dispatches that an application may issue after a successful Non-Subscription Dispatch, but future updates might.  

### GET

The GET method represents a request of data about the Resource. This is most obvious in an application providing access to a filesystem, where a Resource such as `["books"]` may return the listing of the `books` directory in the applications host.

Another example would be to retrieve a record from a database: a GET Dispatch towards Resource `["articles", 356]` may retrieve the record with id `356` from the `articles` collection in the database.

Unlike HTTP, JSTP GET may include a body: since there is no query string, conditions on the request need to be sent in the body. 

### POST

The POST method represents the directive to create a new record within the selected Resource. It usually is completed with a Body with data to be inserted in the newly created record.

Following the previous example, a POST Dispatch may be issued to the Resource `["articles"]` with the Body `{"text": "New article"}` and the application may create a record with the given text. 

### PUT

The PUT method represents the operation of inserting a record in the selected Resource. Unlike POST that selects the resource within which the record should be created, the Resource in a PUT Dispatch represents the exact address where the record should be created in the receiving application. The sent Body should replace any existing records in the Resource address.

A PUT Dispatch may or may not feature a Body.

For example, a PUT Dispatch with Resource `["links", 54]` and Body `{"href":"google.com"}` represents the directive to the application to create or update the record referenced by `["links", 54]` to contain the Body.

### PATCH

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

### DELETE

_todo_

### BIND

_todo_

### RELEASE

_todo_
