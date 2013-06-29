[Table of Contents](index.md) | [Previous: Introduction](introduction.md) | [Next: Vocabulary](vocabulary.md)

---

Changes since previous versions
===============================

Vocabulary
----------

- Introduction of the term "Morphology" appliable to a certain configuration of the Dispatches.
- Introduction of the term "Answer Dispatch" to identify dispatches with the Verb ANSWER and "Answer Resource" to identify the formalized Resource that accompanies such a Dispatch.
- Introduction of the term "Answer Callback" to identify the actual programming rutine to be executed when the Answer is received.
- Introduction of the term "Application Level Response" and "Protocol Level Response" to differentiate regular Dispatches that are issued after a Dispatch is received by a part of the system from Answer Dispatches.
- Formalization of the term "Emitter" to identify the application resource which generates the Dispatch and instructs the JSTP Engine to process it.
- Formalization of the term "Transaction ID" to identify the first item in the Token Header.
- Introduction of the three Morphologies as distinct configurations:
  - The Subscription Morphology: after a Dispatch with the BIND or RELEASE methods and an Endpoint Header
  - The Regular Morphology: after a Dispatch with GET, PUT, POST, PATCH or DELETE as methods
  - The Answer Morphology: after a Dispatch with the ANSWER Method and a regulated Resource Header
- Introduction of the terms "JSTP Transport Protocol" and "JSTP Transport Protocol Layer" to identify the array of possible transports in which JSTP can be sent (now known to be mainly plain TCP and Websockets).
- Introduction of the term "Virtual Host" to identify valid Host Header values that mark a Dispatch to be forwarded, not over a network, but inside the application to a different JSTP engine.
- Introduction of the term "Subscriptions Table" to identify the list of bound endpoints in an Engine.

Syntax
------

- Introduction of the ANSWER method for answering Status Codes to emitters which are preoccupied with the success of the operation.
- Introduction of a formalization of the Answer Resource in the Answer Morphology.
- Formalization of the first item in the Token as an UUID which identifies the Dispatch in terms of Transaction. 
- Introduction of Named Resource Patterns to work like their REST counterparts. This extension includes:
  - Wildcards followed by identifiers: `['...anything']` or `['Book', '*author']`
  - Alias Named Resource Items, such as `[':name']` or `['Article', ':id']`. This are just like the REST ones.
  - And of course the `\` escape character in order to escape literal resources with special characters: `['\:afteracolon:']` or `['\...afterall']`

### URL

- Formalization of the JSTP URI Scheme for easy description of Dispatches. Far for complete but proved to be essential for logging and communication purposes.

Engine
------

### Answer

Instructions to Engines to:

- Identify the Emitters willingness to receive an Answer (by storing a provided Answer Callback, IF there is an Answer Callback).
- Generate the Transaction ID.
- Bind the correct Endpoints for the Answer to be received.
- Upon processing of the Answer, Release the Endpoints.

### Robustness

Instructions to Engines to:

- Always attempt to reconnect lost outgoing connections to remote engines.
- Once reconnected, always forward the Endpoints which are currently bound locally but are aimed at the remote engine.

### Security & Usability

Intruction to Engines to provide a configuration tool for the developer to configure the default behaviours, specifically:

- Automatic gateway functionality
- Implementation Disclosure (for the 5xx Status Codes in the Answer Dispatches).
- Automatic reconnection attempt
- Reconnection attempt timespan

---

[Table of Contents](index.md) | [Previous: Introduction](introduction.md) | [Next: Vocabulary](vocabulary.md)
