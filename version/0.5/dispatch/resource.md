[Dispatch](index.md) | [Previous: Method Header](method.md) | [Next: Timestamp Header](timestamp.md)

---

Resource Header
===============

Summary
-------

```javascript
"resource": ["books", "The Lord of the Rings", "page", 97]
```

- Type: Array of Scalars
- Elements: An arbitrarily long array containing non-empty scalars that represent the selected Resource.
- _Required_

The Resource header represents the application resource upon which the Dispatch is to operate.

It can represent, for example:

- A file in a filesystem.
- A record in a database.
- A controller in a web application.

> This list features common and easy to understand examples, but far more exotic resources can be represented.

The resources can be accessed with several methods, so it may be possible to create, update, destroy or query resources via JSTP. Since JSTP is symmetrical and asynchronous, the possibilities are more diverse than CRUD (Crete, Read, Update, Delete), but still resources should represent as much as possible the range of actions in the application.

---

[Dispatch](index.md) | [Previous: Method Header](method.md) | [Next: Timestamp Header](timestamp.md)
