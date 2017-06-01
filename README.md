# RESTful API Design Guidelines

* [Introduction](#introduction)
* [Our Technology Recommendations for APIs](#our-technology-recommendations-for-apis)
* [Pragmatic REST](#pragmatic-rest)
* [Pragmatic REST Principles](#pragmatic-rest-principles)

## Introduction

This document provides guidelines and examples to help developers design simple, consistent and easy-to-use RESTful 
APIs.

## References

* [REST in Practice](http://shop.oreilly.com/product/9780596805838.do)
* [APIs A Strategy Guide](http://shop.oreilly.com/product/0636920021223.do)
* [REST API Design Rulebook](http://shop.oreilly.com/product/0636920021575.do)
* [HTTP: The Definitive Guide](http://shop.oreilly.com/product/9781565925090.do)

## Our Technology Recommendations for APIs

Unless there are specific reasons otherwise, the first choices for any API development team today should be:
* Pragmatic REST, because an API should be easy to provide, learn and consume
* JSON, because it is easy to produce and consume
* OAuth, because it prevents password propagation

## Pragmatic REST

The REST architectural style leverages the HTTP protocol to define an approach to inter-computer communications.
It uses URI's to define “resources” and the standard HTTP verbs (POST, GET, PUT, and DELETE) as synonyms for create, 
read, update, and delete (CRUD).

These guidelines seek a balance between a truly RESTful API and a positive developer experience.
We follow REST principles (but not all of them).

## Pragmatic REST Principles

### URIs matter
RESTful APIs use Uniform Resource Identifiers (URIs) to address resources.
And, a good **resource model** makes an API easy to produce, consume, discover, and extend.

### Parameters matter
Use a standard and easy-to-guess set of optional parameters for each API call.

### Data format matters
Make it straightforward for programmers to understand the data the API expects, what kind of data it will return, and 
how to change that.

### Return codes matter
Don't return 200 (OK) when you should be returning 201 (CREATED) and a
[Location Header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.30) pointing to the location of the 
new resource.

### Versioning matters
Always version your API.

### Everything else should be hidden
Security, rate limiting, routing, and so on can and should be hidden in the HTTP headers.

## Resource Oriented Design

When desiging resource-oriented APIs, follow these steps:
* Determine what types of resources an API provides
* Determine the relationships between resources.
* Decide the resource name schemes based on types and relationships.
* Decide the resource schemas.
* Attach minimum set of methods to resources.

## Resource Modeling
The URI path conveys a REST API’s resource model, with each forward slash separated
path segment corresponding to a unique resource within the model’s hierarchy. For
example, this URI design:

## General guidelines for RESTful APIs

## URI Authority Design

// TODO
This section covers the naming conventions that should be used for the authority portion of a REST API.

### Consistent subdomain names should be used for your client developer portal
If an API provides a developer portal, by convention it should have a subdomain labeled developer. 
For example:
    `https://developer.ato.gov.au`

...

### URIs
* A URI identifies a resource.
* URIs should include nouns, not verbs.
* URI paths that refer to a collection of objects should use a plural noun, for example, `/orders`.

#### Examples

| VERB   | URI               | DESCRIPTION                         |
| -------| ----------------- | ----------------------------------- |
| POST   | /orders           | Creates a new order.                |
| GET    | /orders/{orderId} | Retrieves a specific order.         |
| PUT    | /orders/{orderId} | Updates a specific order.           |
| DELETE | /orders/{orderId} | Deletes a specific order.           |
| GET    | /orders           | Retrieves a list of orders.         |
| PATCH  | /orders/{orderId} | Partially updates a specific order. |


...

### Versioning
// TODO
See: http://semver.org/
