Resource Header
===============

- Type: Array of Scalars
- Elements: An arbitrarily long array containing non-empty scalars that represent the selected Resource.
- _Required_

The Resource is to be interpreted as a resource in the application. It can represent, for example:

- A file in a filesystem.
- A record in a database.
- A controller in a web application.

The resources can be accessed with several methods, so it may be possible to create, update, destroy or query resources via JSTP. Since JSTP is symmetrical and asynchronous, the possibilities are more diverse than CRUD (Crete, Read, Update, Delete), but still resources should represent as much as possible the range of actions in the application.
