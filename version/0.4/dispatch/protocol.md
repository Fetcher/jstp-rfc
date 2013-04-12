Protocol Header
===============

- Type: Array of Strings.
- Elements: 
  1. "JSTP" - A string stating that the Dispatch uses the JSTP protocol. This field is case insensitive.
  2. <version-number> - A string representing the JSTP version number.
- _Required_

The protocol header represents the JSTP protocol version that the emitting Engine is using, and subsequently the JSTP version that the Dispatch is written in. It consists of an array of strings, being the first "jstp" and the second the version number.

The version number must be used by the receiving Engine to process the Dispatch according to the specifications of the JSTP version. If the engine is unable to process the Dispatch version it must send an Exception Dispatch back to the source.

> The protocol header may be extended with further data to specify JSTP extensions.