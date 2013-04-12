Timestamp Header
================

- Type: Long Integer
- Element: The time when the Dispatch was issued in the UNIX time stamp format
- _Required_

The timestamp is a mandatory header. Its rationale is to simplify logging, synchronization and processing. 

Gateways should not change the timestamp. 

> As this draft, Exception Dispatches should not modify the Timestamp so that emitters can identify the faulty Dispatch; relying on the Timestamp is not robust enough and this should most likely be done using the Token header, but a standarization of the Token's elements has not been developed yet.