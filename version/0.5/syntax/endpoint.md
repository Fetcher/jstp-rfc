[Syntax](index.md) | [Previous: Body Header](body.md) | [Next: Referer Header](referer.md)

---

Endpoint Header
===============

The Endpoint Header is _required_ in the Subscription Morphology and _illegal_ in both the Regular and Answer Morphologies.

Subscription Morphology
-----------------------

The Endpoint Header `pair` key must be `endpoint` and the `value` must be a JSON `object` containing two `pairs`. 

The Endpoint Pattern, composed by both the Method Pattern and the Resource Pattern, represents the combination of Method and Resource Headers that, if present in a Dispatch processed by the Engine to which this Dispatch is aimed, should trigger the Subscription that will result from the processing of the current Dispatch. In other words, the JSTP endpoint to which the Emitter wishes the provided Callback to be bound.

### Method Pattern

One of the `pairs` must be the Method Pattern pair. The JSON `string` key of the Method Pattern must be "method" and the `value` must be a JSON `string` containing either a Literal Method or a Method Wildcard. 

#### Literal Method

A Literal Method is any of the JSTP Methods. It represents the corresponding method.

#### Method Wildcard

A Method Wildcard is the [`*` U+002A ASTERISK](http://www.unicode.org/charts/PDF/U0000.pdf) character and represents any JSTP Method.

### Resource Pattern

The other `pair` must be the Resource Pattern pair. The JSON `string` key of the Resource Pattern must be "resource" and the `value` must be a JSON `array` containing at least one JSON `string`, that must be either a Literal Resource Element or one of the Resource Element Wildcards.

The order of the `elements` in the `array` is relevant since it must be used to check the Resource of the Dispatches processed by the Engine for a match.

In Subscription Dispatches, the Resource Pattern is not matched against the Resource Header because it is illegal; instead, it is matched against the Resource Pattern within the Dispatch Endpoint Header.

#### Literal Resource Element

A Literal Resource Element is any JSON `string` that is not a Resource Element Wildcard. It represents the exact same `string`.

#### Resource Element Wildcards

##### Escape Character

The [`\` U+005C REVERSE SOLIDUS](http://www.unicode.org/charts/PDF/U0000.pdf) is the Escape Character that can be used in the Resource Pattern Elements to specify that an otherwise Wildcard is actually a Literal Resource Element. The Escape Character is only recognized when used as the first character in the `string`.

##### Asterisk Element Wildcard

The Asterisk Element Wildcard is the [`*` U+002A ASTERISK](http://www.unicode.org/charts/PDF/U0000.pdf) character and represents any possible value in the Asterisk Element Wildcard position in the Resource Pattern `array`. 

The Asterisk Element Wildcard is valid in any position in the `array`, except after an Ellipsis Element Wildcard. Engines running Quirks Mode may ignore this and interpret an Asterisk Element Wildcard after an Ellipsis Element Wildcards as only an Ellipsis Element Wildcard, effectively ignoring the Asterisk Element Wildcard.

##### Ellipsis Element Wildcard
The Ellipsis Element Wildcard are three [`.` U+002E FULL STOP](http://www.unicode.org/charts/PDF/U0000.pdf) characters and represent any possible value over zero or more Resource Elements. That is, if only an Ellipsis Element Wildcard is present, any Resource with any amount of elements will be matched. This contrasts with the Asterisk Element Wildcard that only matches elements its own position. 

The Ellipsis Element Wildcard is valid in the begining of the Resource Pattern `array` or after an Asterisk Element Wildcard, a Literal Resource Element or a Named Element Wildcard. It is illegal after another Ellipsis Element Wildcard but Engines running Quirks Mode may ignore this and interpret two consecutives Ellipsis Element Wildcards as only one.

The Ellipsis Element Wildcard may precede a Literal Resource Element and in that case it represents


---

**CONTINUE**

---

- Type: Object
- Elements: `method` pattern with a string value and `resource` pattern with an array value.
- _Required_: With `BIND` and `RELEASE`
  _Illegal_: With all other Methods

The endpoint header is a special header that can only be sent with the BIND and RELEASE methods in what is called a Subscription Dispatch. When processed, a Subscription Dispatch binds or unbinds the Endpoint to the source of the Dispatch, so that it gets executed when a Dispatch matching the Endpoint is processed in the current Engine. This is further detailed in section [5] Subscription.

The endpoint structure has a `method` pattern and a `resource` pattern, which are similars to the Method and Resource headers of a regular Dispatch but differ in the syntax, as the properties of the endpoint header accept wildcards.

Method pattern
--------------

The method pattern represents the pattern to the JSTP Methods that, in combination with the endpoint resource pattern, should trigger the events attached to the endpoint. 

The method pattern accepts as values all the methods and, additionally, the wildcard `*`. The wilcard represents _any method_: an Endpoint with a wildcard method will match any Dispatch whose Resource matches the resource pattern.

### Matching a Subscription Dispatch

When the method pattern is BIND, the Method in the Endpoint header of the future Dispatch that may trigger the event will be ignored: the endpoint will only match the Resource. 

For example, if the following Subscription Dispatch is issued to an Engine:

```javascript
{
  "protocol": ["JSTP", "0.4"],
  "method": "BIND",
  "timestamp": 1365647440759,
  "endpoint": {
    "method": "BIND",
    "resource": ["foods", "*"]
  }
}
```

any of the following Subscription Dispatches will trigger the event:

```javascript
{
  "protocol": ["JSTP", "0.4"],
  "method": "BIND",
  "timestamp": 1365647458760,
  "endpoint": {
    "method":"POST",
    "resource": ["foods", "*"]
  }
}
```

and

```javascript
{
  "protocol": ["JSTP", "0.4"],
  "method": "BIND",
  "timestamp": 1365647458760,
  "endpoint": {
    "method":"GET",
    "resource": ["foods", "*"]
  }
}
```
and

```javascript
{
  "protocol": ["JSTP", "0.4"],
  "method": "BIND",
  "timestamp": 1365647458760,
  "endpoint": {
    "method":"*",
    "resource": ["foods", "*"]
  }
}
```

and even


```javascript
{
  "protocol": ["JSTP", "0.4"],
  "method": "BIND",
  "timestamp": 1365647458760,
  "endpoint": {
    "method":"BIND",
    "resource": ["foods", "*"]
  }
}
```

[So meta](http://xkcd.com/917/).

> While subscribing to a subscription method may seem too _meta_, it is indeed quite useful. For example, an internal application may bind itself in a gateway application into the BIND method in a user session resource in the gateway, so when the user logs in, the user agent binds itself in the gateway and the internal application is subsequently notified and starts processing data that should be sent to the user.

> Future versions of JSTP may restrict the meta-subscription in order to avoid running into avoidable infinite loops. Engine implementations may implement a mechanism to do this, but no Exception type is defined for the eventuality in this specification.

Resource pattern
----------------

The resource pattern is quite similar to a Resource header, but accepts two special wildcards as elements:

- `*`: The asterisk wildcard may appear alone as an element in the resource pattern and matches any value on that position of the Resource. For example, the resource pattern `["drinks", "*"]` will match both the Resources `["drinks", "water"]` and `["drinks", "beer"]` and any other with "drinks" as first element and a second element. 
- `...`: The ellipsis wildcard (WARNING: not the special punctuation ellipsis character, but the ellipsis formed with three consecutive dots) may appear as the last element in the pattern and represents any amount of elements afterwards. For example, the pattern `["drinks", "..."]` will match both the Resources `["drinks", "soda"]` and `["drinks", "coke", "juice"]`. The ellipsis wildcard should be ignored when not used in the last position.

String elements in the resource pattern will be matched against equal string, case sensitively. 

The backslash character `\` escapes the pattern. For example `\*` will match exactly a `*`. `\...` will match exactly a `...`. In order to match the string `\*` the backslash must be escaped in the pattern, thus making it `\\*`. In the same way the `\...` string can be matched with the pattern `\\...`.

> Future versions of JSTP may support the ellipsis wildcard in different positions.

---

[Syntax](index.md) | [Previous: Body Header](body.md) | [Next: Referer Header](referer.md)
