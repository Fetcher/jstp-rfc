Subscription
============

Subscription refers to the attaching of a callback to an Endpoint pattern on a local or remote JSTP Engine. The subscription is issued via a BIND Dispatch with an Endpoint Header describing the pattern of Dispatches which, upon Processing, will trigger the subscribed callback.

All of callback in JSTP are added via subscription: so, for example, if a local resource wants to hook a callback on an Endpoint, it should be issue a BIND Dispatch with an empty, null, absent or a single loopback element in the Host Header. This is called Reflected Binding or Local Binding. 

> Engine implementations may provide a simple API such a Domain Specific Language in order to simplify Local Binding.

Endpoints
---------

_todo_

### Endpoint hierarchy

_todo_

Subscription pool
-----------------

_todo_

Disconnect Release
------------------

_todo_
