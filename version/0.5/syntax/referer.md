[Syntax](index.md) | [Previous: Endpoint Header](endpoint.md)

---

Referer Header
==============

The Referer Header is _optional_ and has the same structure in all three Morphologies.

The Referer Header `pair` key must be `referer`, but as of the 0.5 version of JSTP, there are no constraints to what type of data is to be used in the `value`: any JSON `value` will do. It is recommended for the Referer Header to be used to convey information about the Emitter.

> The Referer Header maintains consistency with HTTP and should be written verbatim instead of the correct english form "referrer".

---

[Syntax](index.md) | [Previous: Endpoint Header](endpoint.md)

