[Dispatch](index.md) | [Next: Method Header](method.md)

---

Protocol Header
===============

The Protocol Header is _required_ and has the same structure in all three Morphologies.

The Protocol Header must be a JSON `array`. Every `element` of the `array` must be of `string` type. 

#### Protocol Declaration

The first `element` must be the "JSTP" `string`, and should be written in upper case although the Protocol Declaration is case insensitive. This element is _required_.

#### Version Declaration

The second `element` must be the version number corresponding to the JSTP version that the Dispatch Emitter is using. It must be a valid version number of a published JSTP specification. The version number should be used by Engines to process the Dispatch according to the specifications of the declared JSTP version. If the Engine is unable to process a Dispatch of the declared version it must send back a [505 JSTP Version Not Supported](https://github.com/southlogics/jstp-rfc/blob/master/version/0.5/syntax/status-code.md#505-jstp-version-not-supported) Exception Dispatch to the Emitter. An Engine running in [Quirks Mode](https://github.com/southlogics/jstp-rfc/blob/master/version/0.5/vocabulary.md#quirks-mode) may chose to ignore the Version Declaration and process the Dispatch anyway but this may give space for inconsistencies to appear. This element is _required_.

#### Implementation Specifics Declaration

An arbitrary number of `string elements` may be added afterwards by the Emitter in order to declare specifics about the implementations of any plugins that the Emitter is expecting for the Engine or a Remote Engine to use. Engines may ignore unrecognized Implementation Specifics Declarations or may ignore any `element` after the second element. This element is _optional_.

Samples
-------

Plain 0.5 Protocol Header:

```javascript
"protocol": ["JSTP", "0.4"]
```

Protocol Header with Implementation Specifics Declarations:

```javascript
"protocol": ["JSTP", "0.5", "node-jstp-1.2.4"]
```

---

[Dispatch](index.md) | [Next: Method Header](method.md)
