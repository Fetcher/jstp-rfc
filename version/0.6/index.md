JSON Transfer Protocol: JSTP/0.6 Working Draft
==============================================

This document specifies an independently created communication standard aimed at the Internet community. 

The JSON Transfer Protocol version described here is major version `0` and minor version `6`, referred to as `JSTP/0.6`.

This document and the specified standard are released by the JSTP collaborators under the [Creative Commons Attribution-ShareAlike 3.0 Unported License](http://creativecommons.org/licenses/by-sa/3.0/deed).

Introduction
------------

The JSON Transfer Protocol (JSTP) is an application-level, symmetrical, asynchronous communication protocol for distributed computation systems that works with [JSON](http://www.json.org/) encoded messages. It is designed to be transported over various possible means, including plain [TCP](http://www.ietf.org/rfc/rfc793.txt), [HTTP](http://www.w3.org/Protocols/rfc2616/rfc2616.html) and [WebSocket](http://tools.ietf.org/html/rfc6455).

### Design goals

- Facilitate general purpose, cross platform distributed computing with an abstract, high level API focused on creation, reading, updating and destroying operations.
- Standardize interactions among applications connecting with each other playing different roles either as clients or servers.
- Seamlessly integrate remote and local event driven capabilities.

Key features that the protocol should contemplate, in order of importance:

1. **Symmetry**: JSTP messages should not change depending on whether the sender is behaving as server or client in the transport protocol. 
2. **Event-driven**: JSTP should be structured around a subscription architecture that allows applications to react to the triggering of extensible events.
3. **Actionability**: JSTP messages should represent actionable commands to be performed by the receiving party.
4. **Asynchronicity**: One party may send zero or any amount of JSTP messages to the other and the other may send back or forward zero or any amount of messages.
5. **Reflectiveness**: JSTP messages and subscriptions should be performable locally as they would be over a network.
6. **Ubiquity**: It should be trivial to send JSTP messages across semi-isolated networks using hosts as gateways.
7. **Distributability**: It should be trivial to send the same JSTP message to any amount of parties.

JSTP also aims to maintain quality standards and conventions of modern web applications. It attempts:

- To be readily available to modern browsers via WebSocket and fallback nicely to HTTP polling.
- To be trivial to parse in modern programming platforms. JSON as codification was chosen for that purpose.
- To have a low entry barrier for new adopters. The method and resource API should closely resemble that of [Representational State Transfer (REST)](https://en.wikipedia.org/wiki/Representational_state_transfer) for that purpose.
- To be extensible. As of version `0.5`, arbitrary headers can be added with little to no hassle.

Hybrid distributed applications that connect server side and user agent nodes can benefit greatly from using a single protocol across the entire architecture.

### Related specifications

[JSTP Engine Specification](https://github.com/jstp/jstp-engine): The JSON Transfer Protocol is the core standard in an specification suite along with the [JSTP Engine Specification](https://github.com/jstp/jstp-engine). 

[JSTP URI Scheme](https://github.com/jstp/jstp-uri): An URI addressing specification for describing JSTP resources. Its ultimate aim is to describe JSTP with enough power to be used analogously as HTTP URLs are used by user agents for consuming HTTP resources.

### Requirements

The key words "must", "must not", "required", "shall", "shall not", "should", "should not", "recommended", "may", and "optional" in this document are to be interpreted as described in [Key words for use in RFCs to Indicate Requirement Level _RFC 2119_](http://www.ietf.org/rfc/rfc2119.txt).

An implementation is not compliant if it fails to satisfy one or more of the _must_ or _required_ level requirements for the protocols it implements. An implementation that satisfies all the _must_ or _required_ level and all the _should_ level requirements for its protocols is said to be "unconditionally compliant"; one that satisfies all the _must_ level requirements but not all the _should_ level requirements for its protocols is said to be "conditionally compliant."

### Roadmap

You can find the [version 0.6 Roadmap](https://github.com/jstp/jstp-rfc/issues/32) and the updated status of its implementation as an [issue in the issue tracker](https://github.com/jstp/jstp-rfc/issues/32) for the specification repository.

### Previous versions

- [JSTP/0.5 Specification](../0.5/index.md)
- [JSTP/0.4 Specification](../0.4/index.md) (incomplete)
- [JSTP/0.1 Specification](../0.1/index.md) (incomplete)

> Pseudo versions [0.2](version/pseudo0.2.md) and [0.3](version/pseudo0.3.md) were not described but represent modular updates prior to version 0.4.

Table of Contents
-----------------

1. [Vocabulary](vocabulary.md)
2. [Syntax](syntax/index.md)


Acknowledgements
----------------

### Collaborators

- [Fernando Vía Canel](https://github.com/xaviervia)
- [Luciano Bertenasco](https://github.com/lbertenasco)
- [Matías Domingues](https://github.com/mannias)

### Organizations

- [SouthLogics](http://southlogics.com)

License
-------

[Creative Commons Attribution-ShareAlike 3.0 Unported License](http://creativecommons.org/licenses/by-sa/3.0/legalcode).
    
