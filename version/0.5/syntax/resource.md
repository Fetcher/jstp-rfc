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

The first `element` of the Resource must be a Status Code: that is, a JSON `number` that is with one of the defined three JSON `digits` combinations that is used to describe a certain status of the Answer.

The available Status Codes can be found in the [Status Code Definition](status-code.md).

### Transaction ID

The second `element` of the Resource must be the Transaction ID, a unique identifier present in the Source Dispatch as the first `element` of the Token Header.

Answer Dispatches can only be issued to Source Dispatches that provide a Transaction ID in the Token Header. Engines should discard Answer Dispatches with a `null`, `true` or `false` Transaction IDs. 

### Triggering ID

The third `element` of the Resource must be the Triggering ID, a unique identifier present in the Source Dispatch as the second `element` of the Token Header.

Like is the case with Transaction IDs, Answer Dispatches can only be issued to Source Dispatches that provide a Triggering ID in the Token Header. Engines should discard Answer Dispatches with a `null`, `true` or `false` Triggering IDs. 

Samples
-------

Regular Resource pointing towards a book in a catalogue:

```javascript
"resource": ["J.L.Borges", "Ficciones", "Tlön, Uqbar, Orbis Tertius"]
```

Answer Resource with a 200 Ok Status Code:

```javascript
"resource": [200, "d34c6bec-4cf0-49bf-abc0-f980a1ca4a70", "81a0b3b8-1875-4e53-b6a8-7a398b6790a3"]
```

---

[Dispatch](index.md) | [Previous: Method Header](method.md) | [Next: Timestamp Header](timestamp.md)
