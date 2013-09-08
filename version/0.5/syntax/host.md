[Syntax](index.md) | [Previous: Token Header](token.md) | [Next: Body Header](body.md)

---

Host Header
===========

The Host Header is _optional_ in the Regular and Subscription Morphology and _illegal_ in the Answer Morphology.

The Host Header must be a JSON `array` containing one or more Destination Hosts. It represents a request to the Engine for the Dispatch to not be locally triggered and instead to be sent over a network to the first Destination Host which address is described. Engines must remove the first Destination Host from the list before the Dispatch is actually sent so to avoid redirection loops. 

Engines must not trigger local Subscriptions which endpoints may match the Dispatch if a non-empty Host Header is present. They should forward it to the next Destination Host. Since this recommendation—called the Automatic Gateway feature of JSTP—may lead to security hazards, Engines may be configured to discard Dispatches with a non-empty Host Header and do not forward them. If a Dispatch is not forwarded and it had a Transaction ID/Triggering ID pair set up the Engine should generate an Answer to the Dispatch with a [`506 Gateway Disabled`](status-code.md#506-gateway-disabled) Status Code.

If the Host Header `array` is empty, it should be discarded. `null` and empty `array` `elements` within the Host Header are illegal although Engines running in Quirks Mode may choose to ignore them. 

### Destination Hosts

Each `element` in the Host Header `array` must be itself a JSON `array` with three sub `elements`: 

#### Host Address

The first `element` in a Destination Host must be a Host Address. It must be a JSON `string` representing the some kind of Internet Address—recommended values include [Domain Names](http://tools.ietf.org/html/rfc1034) and IP addresses, both [v4](http://tools.ietf.org/html/rfc791) and [v6](http://www.ietf.org/rfc/rfc2460.txt). Engines should support at least this three addressing protocols. JSTP is designed to run in an IPv4, IPv6 and DNS enabled environment.

#### Port Number

The second `element` in a Destination Host must be either a JSON `number` or a JSON `string` representing the destination's [TCP Port Number](http://tools.ietf.org/html/rfc793).

#### Transport Protocol Label

The third `element` in a Destination Host must be a JSON `string` representing the Transport Protocol intended to be used to communicate with the destination Engine. The Label must be among those listed in this specification.

##### `ws` Label

The `ws` Label represents the [WebSocket Protocol](http://datatracker.ietf.org/doc/rfc6455/).

##### `tcp` Label

The `tcp` Label represents the [Transport Control Protocol](http://www.ietf.org/rfc/rfc793.txt).

Samples
-------

Single Destination Host using WebSockets:

```javascript
"host":[["localhost",8000,"ws"]]
```

Several destinations:

```javascript
"host":[["curiosity.mars","33333","tcp"],["rock.leftside",22,"tcp"]]
```

---

[Syntax](index.md) | [Previous: Token Header](token.md) | [Next: Body Header](body.md)
