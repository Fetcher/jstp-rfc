JSTP/0.5 - JSON Transfer Protocol version 0.5
=============================================

> This document is a work in progress. For discussion please refer to the [issue tracker](https://github.com/southlogics/jstp-rfc/issues) on this repository.

Introduction
------------

The JSON Transfer Protocol (JSTP) is an application-level, symmetrical, asynchronous communication protocol for distributed computation systems that works with [JSON](http://www.json.org/) encoded messages. It is designed to be transported over various possible means, including plain [TCP](http://www.ietf.org/rfc/rfc793.txt), [HTTP](http://www.w3.org/Protocols/rfc2616/rfc2616.html) and [WebSocket](http://tools.ietf.org/html/rfc6455).

JSTP is designed to:

- Facilitate distributed computing with an abstract, high level API inspired in HTTP
- Standarize interactions among applications connecting with each other playing different roles either as clients or servers.

JSTP also aims to maintain quality standards and conventions of modern web applications:

- Is readily avaible to modern browsers via [WebSockets](http://en.wikipedia.org/wiki/WebSocket).
- JSTP is JSON, which makes it trivial to parse in modern programming languages.
- The method and resource API closely resembles that of [Representational State Transfer (REST)](https://en.wikipedia.org/wiki/Representational_state_transfer), so is very natural to adopt for web developers.
- Responses are sent asynchronously, thus making it fit for parallel processing and optimized for non-blocking, event-driven architectures.
- Messages ( _Dispatches_ ) are symmetrical, so every application manages resources with the same API, whether it is behaving as server or as client.
- Supports subscription ( _binding_ ) to events on remote hosts.
- Supports seamless reflective subscription.
- Arbitrary headers can be added with little to no hassle, making it easy to extend.

Hybrid distributed applications that connect server-side and user agent nodes can benefit greatly from using a single protocol accross the entire architecture.

### Related specifications

[JSTP Engine Specification](https://github.com/jstp/jstp-engine): The JSON Transfer Protocol is the core standard in an two-specification suite completed by the [JSTP Engine Specification](https://github.com/jstp/jstp-engine). The version of both specifications is synced and roadmaps for both are elaborated in the same process.

[JSTP URI Scheme](https://github.com/jstp/jstp-uri): An URI addressing specification for describing JSTP resources. Its ultimate aim is to describe JSTP with enough power to be used analogously as HTTP URLs are used by user agents for consuming HTTP resources.

### Requirements

The key words "must", "must not", "required", "shall", "shall not", "should", "should not", "recommended", "may", and "optional" in this document are to be interpreted as described in [Key words for use in RFCs to Indicate Requirement Level _RFC 2119_](http://www.ietf.org/rfc/rfc2119.txt).

An implementation is not compliant if it fails to satisfy one or more of the _must_ or _required_ level requirements for the protocols it implements. An implementation that satisfies all the _must_ or _required_ level and all the _should_ level requirements for its protocols is said to be "unconditionally compliant"; one that satisfies all the _must_ level requirements but not all the _should_ level requirements for its protocols is said to be "conditionally compliant."

### Roadmap

You can find the [version 0.5 Roadmap](https://github.com/southlogics/jstp-rfc/issues/17) and the updated status of its implementation as an [issue in the issue tracker](https://github.com/southlogics/jstp-rfc/issues/17) for the specification repository.

Table of Contents
-----------------

1. [Vocabulary](vocabulary.md)
2. [Syntax](syntax/index.md) 

Older Versions
--------------

- [JSTP/0.4 Specification](../0.4/index.md)
- [JSTP/0.1 Specification](../0.1/index.md)

> Versions [0.2](version/pseudo0.2.md) and [0.3](version/pseudo0.3.md) were not described but represent modular updates prior to version 0.4.

Acknowledgements
----------------

### Collaborators

- [Fernando Vía Canel](https://github.com/xaviervia)
- [Luciano Bertenasco](https://github.com/lbertenasco)
- [Matías Domingues](https://github.com/mannias)

License
-------

    This program is free software: you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation, either version 3 of the License, or
    (at your option) any later version.

    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

    You should have received a copy of the GNU General Public License
    along with this program.  If not, see <http://www.gnu.org/licenses/>.
