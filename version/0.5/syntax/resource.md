[Dispatch](index.md) | [Previous: Method Header](method.md) | [Next: Timestamp Header](timestamp.md)

---

Resource Header
===============

The Resource Header represents the address of the selected resource.

Regular Morphology
------------------

The Resource Header is required in the Regular Morphology. It represents the resource on to which perform the action or query the data.

The Resource Header must be a JSON `array` of one or more `elements`. 

It can represent, for example:

- A file in a filesystem.
- A record in a database.
- A controller in a web application.

The resources can be accessed with the various JSTP Methods, so JSTP can describe the creation, update, destruction or querying of resources. 

Subscription Morphology
-------------------------------

In the Subscription Morphology the Resource Header _must not_ be used. Subscription Dispatches target endpoints instead of resources.

If a Resource Header is present in a Subscription Dispatch, an Engine running in Strict Mode must issue a [400 Bad Dispatch](status-code.md#400-bad-dispatch).

Answer Morphology
------------------------

The Resource Header is required in the Answer Morphology and has a predefined element structure.

The Resource Header must be a JSON `array` of three `elements`. 

### [Status Code](status-code.md)

The first `element` of the Resource must be a Status Code: that is, one of the defined three JSON `digits` combinations that is used to describe a certain status of the Answer.

The available Status Codes can be found in the [Status Code Definition](status-code.md).

### Transaction ID

The second `element` of the Resource must be the Transaction ID, a unique identifier present in the Source Dispatch as the first `element` of the Token Header.

Answer Dispatches can only be issued to Source Dispatches that provide a Transaction ID in the Token Header. Engines should discard Answer Dispatches with a `null`, `true` or `false` Transaction IDs. 

---
CONTINUE
---

### Triggering ID

The third `element` of the Resource must be the Triggering ID, a unique identifier present in the Source Dispatch as the second `element` of the Token Header.

---

[Dispatch](index.md) | [Previous: Method Header](method.md) | [Next: Timestamp Header](timestamp.md)
