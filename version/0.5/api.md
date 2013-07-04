[Table of Contents](index.md) | [Previous: Syntax](syntax/index.md) | [Next: Engine](engine.md)

---

API
===

JSTPEngine
----------

For the API, the Engine is described as the `JSTPEngine` class, that JSTP libraries in object oriented languages should implement.

### JSTPEngine#dispatch

The `#dispatch` instance method of the Engine handles the processing of the Dispatch. The function prototype of `JSTPEngine#dispatch` is as follows:

```
JSTPDispatch dispatch( JSTPDispatch dispatch [, void Function callback (JSTPEngine engine, JSTPDispatch answer [, JSTPDispatch triggering]) [, Object context ]])
```

Lets break it down a little:

1. The return value of the `#dispatch` method must be a JSTPDispatch instance representing the same Dispatch that got processed, including all Header values filled in by the Engine, such as the Timestamp and the Protocol Headers. 
2. The name of the method in the Engine instance must be `dispatch`.
3. The first argument must be the partially or completely formed Dispatch to be processed.
4. The optional function callback must accept:
  - [required] A `JSTPEngine` as the first argument. This will be the Engine executing the callback. Useful for both answers and subsequent Dispatches.
  - [required] A `JSTPDispatch` as the second argument. This will be the ANSWER Dispatch if the callback is executed by an Answer Dispatch
  - [required for BIND Subscription Dispatches only] A `JSTPDispatch` as the third argument. This will be the Dispatch that triggered the callback execution. If this callback is missing in a BIND Subscription Dispatch, the Dispatch is aborted with no Answer (since there is no callback to send the Answer). The Engine might throw a `JSTPMissingCallbackException`.
  
  The function callback should be present in every BIND Subscription Dispatch, since otherwise nothing will be bound to the Dispatch Endpoint.
  
  Implementations should discard any return value from the callbacks.
  
  If the Dispatch is a RELEASE Subscription Dispatch, the function callback should be already bound in the Engine otherwise it will have no effect.
5. The optional context provides a binding for the callback when executed by the Engine. Depending on the language details and the applications architrecture the context will be more or less important.

### JSTPEngine#answer

The `#answer` instance method of the Engine allows the emitters to easily issue Answer Dispatches from an original Dispatch. The function prototype for the `JSTPEngine#answer` is as follows:

```
JSTPDispatch answer( JSTPDispatch source, Integer statusCode [, Object body [, void Function callback (JSTPEngine engine, JSTPDispatch answer) [, Object context]]])
```

Breaking it down:

1. The return value will be the generated Answer Dispatch.
2. The first argument must be the Source Dispatch whose Transaction ID will be used as the first item in the Resource Header of the Answer Dispatch.
3. The second argument must be the Integer [status code](syntax/status-code.md) right for the type of Answer.
4. The third argument is optional, and is a callback function to be bound for the Answer to the Answer. Since the Answer Dispatch is a first level citizen Dispatch, it features an unique own Transaction ID and the receiver of the Answer can itself answer to it. 
5. The fourth argument is optional and it is the context in which the callback will be processed.

JSTPDispatch
------------

The object model of the Dispatch is a class called `JSTPDispatch`. The JSTPDispatch class should provide getter and setter methods for each of the Headers.

JSTPCallback
------------

The function callback _(todo)_

---

[Table of Contents](index.md) | [Previous: Syntax](syntax/index.md) | [Next: Engine](engine.md)
