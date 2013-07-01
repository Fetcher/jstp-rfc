[Syntax](index.md) | [Previous: Timestamp Header](timestamp.md) | [Next: Host Header](host.md)

---

Token Header
============

- Type: Array of Scalars
- Elements: The first element, if present, must be a [UUID](http://en.wikipedia.org/wiki/Universally_unique_identifier) used by Engines as the Transaction ID
- _optional_ but usually included

Since version 0.5, the Token Header is generated automatically by the Engine and any value inserted before Processing will be discarded. If a Callback was provided, the Engine will generate the Transaction ID for the Dispatch and add it as the first item in the Token Header: otherwise, it will be left blank.

This removes a previous ambiguity with respect to the usage of the Token Header by applications. The Token Header is now Protocol Level and applications are advised to use the Body instead.

---

[Syntax](index.md) | [Previous: Timestamp Header](timestamp.md) | [Next: Host Header](host.md)
