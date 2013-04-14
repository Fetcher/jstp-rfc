Exception Header
================

- Type: Object
- Elements: `code` integer and `message` string

The exception header represents the description of a protocol level exception that prevented the Dispatch from being processed altogether. A Dispatch featuring an Exception header is called an Exception Dispatch and it should be returned to the source of the Dispatch whenever is possible. 

An Exception Dispatch may or may not have a Method and a Resource depending on the type of the exception. 

The code property represents the code of the exception and the message represents a human-readable description of the exception.

> The code property values have mostly been borrowed from HTTP so to lower the entrance barrier for web developers to JSTP.

400 Bad Dispatch
----------------

- Code: 400
- Message: Bad Dispatch

The Bad Dispatch exception type should be sent when the processed Dispatch has one of the following problems:

- Some required headers are missing 
- The content of some required headers is missing or malformed
- The engine was unable to parse the JSON serialization

Since in either case the Dispatch is assumed to be unrecoverable, no Method nor Resource should be sent in the Exception Dispatch.

> For the dis _todo_
> For tolerance to malformed Dispatches in the engine implementation please see section [6.2.2] Tolerance and Quirks Mode

404 Not Found
-------------

_todo_ 

406 Not Bound
-------------

_todo_

502 Not Gateway
---------------

_todo_

505 JSTP Version Not Supported
------------------------------

_todo_
