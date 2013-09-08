[Syntax](index.md) | [Previous: Host Header](host.md) | [Next: Endpoint Header](endpoint.md)

---

Body Header
===========

The Body Header is _optional_ and has the same structure in all three Morphologies.

The Body Header `pair` key must be `body` and the `value` can be any type of JSON `value`.

The Body is the payload of the Dispatch and its contents and entirely up to applications to process.

Samples
-------

Array of notes:

```javascript
"body":[
  {"title":"Don't forget the milk","content":"Seriously, the milk!"},
  {"title":"You buy the next lot","content":"Ok I'll buy it"}
]
```

Plain string message:

```javascript
"body":"This message is clearly undisclosed"
```

Language shorcuts object:

```javascript
"body":{
  "se":"Swedish",
  "zh":"Chinese",
  "mr":"Martian"
}
```

---

[Syntax](index.md) | [Previous: Host Header](host.md) | [Next: Endpoint Header](endpoint.md)
