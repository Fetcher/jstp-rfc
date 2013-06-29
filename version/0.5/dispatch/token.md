Token Header
============

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