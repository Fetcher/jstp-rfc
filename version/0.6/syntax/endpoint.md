[Syntax](index.md) | [Previous: Body Header](body.md) | [Next: From Header](from.md)

---

Endpoint Header
===============

The Endpoint Header is _required_ in the Subscription Morphology and _illegal_ in both the Regular and Answer Morphologies.

Subscription Morphology
-----------------------

The Endpoint Header `pair` key must be `endpoint` and the `value` must be a JSON `object` containing two `pairs`. 

The Endpoint Pattern, composed by both the Method Pattern and the Resource Pattern, represents the combination of Method and Resource Headers that, if present in a Dispatch processed by the Engine to which this Dispatch is aimed, should trigger the Subscription that will result from the processing of the current Dispatch. In other words, the JSTP endpoint to which the Emitter wishes the provided Callback to be bound.

### Method Pattern

One of the `pairs` must be the Method Pattern pair. The JSON `string` key of the Method Pattern must be "method" and the `value` must be a JSON `string` containing either a Literal Method or a Method Wildcard. 

#### Literal Method

A Literal Method is any of the JSTP Methods. It represents the corresponding method.

#### Method Wildcard

A Method Wildcard is the [`*` U+002A ASTERISK](http://www.unicode.org/charts/PDF/U0000.pdf) character and represents any JSTP Method.

### Resource Pattern

The other `pair` must be the Resource Pattern pair. The JSON `string` key of the Resource Pattern must be "resource" and the `value` must be a JSON `array` containing at least one JSON `string`, that must be either a Literal Resource Element or one of the Resource Element Wildcards.

The order of the `elements` in the `array` is relevant since it must be used to check the Resource of the Dispatches processed by the Engine for a match.

In Subscription Dispatches, the Resource Pattern is not matched against the Resource Header because it is illegal; instead, it is matched against the Resource Pattern within the Dispatch Endpoint Header.

#### Literal Resource Element

A Literal Resource Element is any JSON `string` that is not a Resource Element Wildcard. It represents the exact same `string`.

#### Resource Element Wildcards

##### Escape Character

The [`\` U+005C REVERSE SOLIDUS](http://www.unicode.org/charts/PDF/U0000.pdf) is the Escape Character that can be used in the Resource Pattern Elements to specify that an otherwise Wildcard is actually a Literal Resource Element. The Escape Character is only recognized when used as the first character in the `string`.

##### Asterisk Element Wildcard

The Asterisk Element Wildcard is the [`*` U+002A ASTERISK](http://www.unicode.org/charts/PDF/U0000.pdf) character and represents any possible value in the Asterisk Element Wildcard position in the Resource Pattern `array`. 

The Asterisk Element Wildcard is valid in any position in the `array`, except after an Ellipsis Element Wildcard. Engines running Quirks Mode may ignore this and interpret an Asterisk Element Wildcard after an Ellipsis Element Wildcards as only an Ellipsis Element Wildcard, effectively ignoring the Asterisk Element Wildcard.

> Non-normative: The Asterisk Element Wildcard behavior is analog to that of the [Perl Regular Expressions `.` char](http://perldoc.perl.org/perlre.html).

##### Ellipsis Element Wildcard

The Ellipsis Element Wildcard are three [`.` U+002E FULL STOP](http://www.unicode.org/charts/PDF/U0000.pdf) characters and represent any possible value over zero or more Resource Elements, starting from the position of the the previously matched element, be it the beginning or the match for another Element. That is, if only an Ellipsis Element Wildcard is present, any Resource with any amount of elements will be matched, and if the Ellipsis Element Wildcard is preceded by another Resource Pattern Element, any amount of Resource Elements after the previous match will be matched and until the next non-Asterisk Element Wildcard match (see the Samples below for clarifications). This contrasts with the Asterisk Element Wildcard that only matches elements its own position. 

The Ellipsis Element Wildcard is valid in the begining of the Resource Pattern `array` or after an Asterisk Element Wildcard, a Literal Resource Element or a Named Element Wildcard. It is illegal after another Ellipsis Element Wildcard but Engines running Quirks Mode may ignore this and interpret two consecutives Ellipsis Element Wildcards as only one.

The Ellipsis Element Wildcard may precede a Literal Resource Element and in that case it represents zero or more elements starting at the position of the Ellipsis Element Wildcard until the Literal Resource Element is found.
 
> Non-normative: The Ellipsis Element Wildcard behavior is analog to that of the [non-greedy Perl Regular Expressions `*` char](http://perldoc.perl.org/perlre.html).

##### Named Element Wildcard

The Named Element Wildcard is composed by the [`:` Ux003A COLON](http://www.unicode.org/charts/PDF/U0000.pdf) followed by one or more uppercase or lowercase letters. An unescaped `:` followed by nothing is illegal although Engines running in Quirks Mode may choose to treat it as an Asterisk Element Wildcard.

The Named Element Wildcard represents any Resource Element like the Asterisk Element Wildcard but instructs the Engine to send Callbacks the content of the matched Resource Element as the value of a key named after the letters in the Named Element Wildcard. Implementation details for this features will be described in the [JSTP/0.5 Engine 1.0](https://github.com/jstp/jstp-engine) specification, although they are non-normative for this specification.

The Named Element Wildcard is valid in any position except between two Ellipsis Element Wildcards. Used after an Ellipsis Element Wildcard and before either an Asterisk Element Wildcard, a Literal Resource Element or as the final `element` of the `array`, it represents the last element of what otherwise would be matched by the Ellipsis Element Wildcard.

Samples
-------

Simple subscription and a matched Dispatch:

```javascript
// Subscription
"endpoint": {
  "method": "*",
  "resource": ["*"]
}

// Matches
"method": "GET",
"resource": ["user"]
```

Named Element Wildcard and a matched Dispatch:

```javascript
// Subscription
"endpoint": {
  "method": "PUT",
  "resource": ["article", ":title"]
}

// Matched
"method": "PUT",
"resource": ["article", "Great new series just released"]

// The result would be 
// "title": "Great new series just released"
```

Ellipsis Element Wildcard alone and matching Dispatches:

```javascript
// Subscription
"endpoint": {
  "method": "GET",
  "resource": ["..."]
}

// Matches
"method": "GET",
"resource": ["book", "The Lord of the Rings"]

// Matches
"method": "GET",
"resource": ["this", "is", "a", "very", "long", "resource"]
```

Ellipsis Element Wildcard between Literals and another Ellipsis Element Wildcard just before the ending:

```javascript
// Subscription
"endpoint": {
  "method": "POST",
  "resource": ["path", "...", "text", "...", ":extension"]
}

// Matches
"method": "POST",
"resource": ["path", "folder", "internal", "text", "value", "txt"]
//  The result would be
// "extension": "txt"

// Matches
"method": "POST",
"resource": ["path", "text", "md"]
// The result would be
// "extension": "md"
```

---

[Syntax](index.md) | [Previous: Body Header](body.md) | [Next: From Header](from.md)
