# RESTful API Design Guidelines

* [Introduction](#introduction)
* [Our Technology Recommendations for APIs](#our-technology-recommendations-for-apis)
* [Pragmatic REST](#pragmatic-rest)
* [Pragmatic REST Principles](#pragmatic-rest-principles)
* [Resource Oriented Design](#resource-oriented-design)

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
* Determine the relationships between resources
* Decide the resource name schemes based on types and relationships
* Decide the resource schemas
* Attach minimum set of methods to resources

### Resource Modelling
Resource modelling is an exercise that establishes your API’s key concepts. Before diving directly into the design of
URI paths, it may be helpful to first think about the REST API’s resource model.

### Resource Archetypes
When modeling an API’s resources, we can start with the some basic resource archetypes. Like design patterns, the 
resource archetypes help us consistently communicate the structures and behaviors that are commonly found in REST API 
designs. A REST API is composed of four distinct resource archetypes: **document**, **collection**, **store**, and **controller**.

### Document
A document resource is a singular concept that is akin to an object instance or a database record. A document’s state 
representation typically includes both fields with values and links to other related resources. With its fundamental  
field and link-based structure, the document type is the conceptual base archetype of the other resource archetypes.
In other words, the three other resource archetypes can be viewed as specialisations of the document archetype.

Each URI below identifies a document resource:

    http://api.superannuation.ato.gov.au/forms/form-1a

A document may have child resources that represent its specific subordinate concepts. With its ability to bring many 
different resource types together under a single parent, a document is a logical candidate for a REST API’s root 
resource, which is also known as the "docroot". The example URI below identifies the docroot, which is the Superannuation 
business area's REST API entry point:

    http://api.superannuation.ato.gov.au

### Collection
A collection resource is a server-managed directory of resources. Clients may propose new resources to be added to a 
collection. However, it is up to the collection to choose to create a new resource, or not. A collection resource 
chooses what it wants to contain and also decides the URIs of each contained resource.

Each URI below identifies a collection resource:

    http://api.superannuation.ato.gov.au/forms

### Store
A store is a client-managed resource repository. A store resource lets an API client put resources in, get them back 
out, and decide when to delete them. On their own, stores do not create new resources; therefore a store never 
generates new URIs. Instead, each stored resource has a URI that was chosen by a client when it was initially put into 
the store.

The example interaction below shows a user (with ID 1234) of a client program using a fictional REST API to insert a 
document resource named "receipt-101" in his or her store of deductions:

    PUT /users/1234/deductions/receipt-101

### Controller
A controller resource models a procedural concept. Controller resources are like executable functions, with parameters 
and return values; inputs and outputs. Like a traditional web application’s use of HTML forms, a REST API relies on 
controller resources to perform application-specific actions that cannot be logically mapped to one of the standard 
methods (create, retrieve, update, and delete, also known as CRUD).

Controller names typically appear as the last segment in a URI path, with no child resources to follow them in the 
hierarchy. The example below shows a controller resource (from a fictional REST API) that allows a client to resend an 
alert to a user:

    POST /alerts/245743/resend

## URI Path Design
Each URI path segment, separated by forward slashes (/), represents a design opportunity. Assigning meaningful values 
to each path segment helps to clearly communicate the hierarchical structure of a REST API’s resource model.

* A singular noun should be used for document names
* A plural noun should be used for collection names
* A plural noun should be used for store names
* A verb or verb phrase should be used for controller names

