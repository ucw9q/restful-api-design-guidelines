# RESTful API Design Guidelines

* [Introduction](#introduction)
* [Pragmatic REST](#pragmatic-rest)
* [Pragmatic REST Principles](#pragmatic-rest-principles)
* [Resource Oriented Design](#resource-oriented-design)
* [Resource Modelling](#resource-modelling)
* [URI Path Design](#uri-path-design)
* [URI Query Design](#uri-query-design)
* [Request Methods](#request-methods)
* [Response Status Codes](#response-status-codes)
* [Versioning](#versioning)
* [Security](#security)

## Introduction

This document provides guidelines and examples to help developers design simple, consistent and easy-to-use RESTful 
APIs. Much of the material is based on common design problems found while developing RESTful web services. The problems 
include resource and representation design, URIs, usage of HTTP, caching, concurrency control, partial updates, batch 
processing, transactions, versioning and security.

### References

* [REST in Practice](http://shop.oreilly.com/product/9780596805838.do)
* [APIs A Strategy Guide](http://shop.oreilly.com/product/0636920021223.do)
* [REST API Design Rulebook](http://shop.oreilly.com/product/0636920021575.do)
* [RESTful Web Services Cookbook](http://shop.oreilly.com/product/9780596801694.do)
* [HTTP: The Definitive Guide](http://shop.oreilly.com/product/9781565925090.do)

### Our Technology Recommendations for APIs

The first choices for any API development team today should be:
* Pragmatic REST, because an API should be easy to provide, learn and consume
* JSON, because it is easy to produce and consume
* OAuth, because it prevents password propagation

We describe these technologies as a first choice, not as an only choice. For example, you might decide to include a 
query language, like [GraphQL](https://developer.github.com/v4/), with your API.

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
[Location](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.30) header pointing to the location of the 
new resource.

### Versioning matters
Always version your API. By default, all requests receive the latest version of the REST API. 
We encourage you to explicitly request a particular version via the Accept Header.

### Everything else should be hidden
Security, rate limiting, routing, and so on can and should be hidden in the HTTP headers.

## Resource Oriented Design

When desiging resource-oriented APIs, follow these steps:
* Determine what types of resources an API provides
* Determine the relationships between resources
* Decide the resource name schemes based on types and relationships
* Decide the resource schemas
* Attach minimum set of methods to resources

## Resource Modelling
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
```
https://api.superannuation.ato.gov.au/forms/form-1a
https://api.superannuation.ato.gov.au/fact-sheets/super-and-tax
```

A document may have child resources that represent its specific subordinate concepts. With its ability to bring many 
different resource types together under a single parent, a document is a logical candidate for a REST API’s root 
resource, which is also known as the "docroot". The example URI below identifies the docroot, which is the Superannuation 
business area's REST API entry point:
```
https://api.superannuation.ato.gov.au
```

### Collection
A collection resource is a server-managed directory of resources. Clients may propose new resources to be added to a 
collection. However, it is up to the collection to choose to create a new resource, or not. A collection resource 
chooses what it wants to contain and also decides the URIs of each contained resource.

Each URI below identifies a collection resource:
```
https://api.superannuation.ato.gov.au/forms
https://api.superannuation.ato.gov.au/fact-sheets
```

### Store
A store is a client-managed resource repository. A store resource lets an API client put resources in, get them back 
out, and decide when to delete them. On their own, stores do not create new resources; therefore a store never 
generates new URIs. Instead, each stored resource has a URI that was chosen by a client when it was initially put into 
the store.

The example interaction below shows a user (with ID 1234) of a client program using a fictional REST API to insert a 
document resource named "receipt-101" in his or her store of deductions:
```
PUT /users/1234/deductions/receipt-101
```

### Controller
A controller resource models a procedural concept. Controller resources are like executable functions, with parameters 
and return values; inputs and outputs. Like a traditional web application’s use of HTML forms, a REST API relies on 
controller resources to perform application-specific actions that cannot be logically mapped to one of the standard 
methods (create, retrieve, update, and delete, also known as CRUD).

Controller names typically appear as the last segment in a URI path, with no child resources to follow them in the 
hierarchy. The example below shows a controller resource (from a fictional REST API) that allows a client to resend an 
alert to a user:
```
POST /alerts/4321/resend
```

## URI Path Design

Each URI path segment, separated by forward slashes (/), represents a design opportunity. Assigning meaningful values 
to each path segment helps to clearly communicate the hierarchical structure of a REST API’s resource model.

Guidelines:
* A singular noun should be used for document names.
* A plural noun should be used for collection names.
* A plural noun should be used for store names.
* A verb or verb phrase should be used for controller names.

## URI Query Design
A URI’s query comes after the path and before the optional fragment:
```
URI = scheme "://" authority "/" path [ "?" query ] [ "#" fragment ]
```

As a component of a URI, the query contributes to the unique identification of a resource.

The query component can provide clients with additional interaction capabilities such as ad hoc searching and filtering.
Therefore, unlike the other elements of a URI, the query part may be transparent to a REST API’s client.

Guidelines:
* The query component of a URI may be used to filter collections or stores.
* The query component of a URI should be used to paginate collection or store results.

## Request Methods

Each HTTP method has specific, well-defined semantics within the context of a REST API’s resource model. The purpose of 
**GET** is to retrieve a representation of a resource’s state. **HEAD** is used to retrieve the metadata associated 
with the resource’s state. **POST** should be used to create a new resource within a collection and execute 
controllers. **PUT** should be used to add a new resource to a store or update a resource. **DELETE** removes a 
resource from its parent. 

Guidelines:
* GET must be used to retrieve a representation of a resource.
* GET and POST must not be used to tunnel other request methods.
* HEAD should be used to retrieve response headers.
* POST must be used to create a new resource in a collection.
* POST must be used to execute controllers.

* PUT must be used to both insert (the client can decide the URI) and update a stored resource.
* PUT must be used to update mutable resources.

* DELETE must be used to remove a resource from its parent.
* OPTIONS should be used to retrieve metadata that describes a resource’s available interactions.

**Note:** The recommended usage of the HTTP’s POST method for each of the four resource archetypes:

|     | Document | Collection                      | Store | Controller           |
| ----| ---------| ------------------------------- | ----- | -------------------- |
| POST| _error_  | Create a new contained resource |_error_| Execute the function |
 
## Response Status Codes

HTTP defines forty standard status codes that can be used to convey the results of a client’s request. The status codes 
are divided into the five categories:

| Category           | Description                                                                                    |     
| -------------------| ---------------------------------------------------------------------------------------------- |
| 1xx: Informational | Communicates transfer protocol-level information.                                              |
| 2xx: Success       | Indicates that the client’s request was accepted successfully.                                 |
| 3xx: Redirection   | Indicates that the client must take some additional action in order to complete their request. |
| 4xx: Client Error  | This category of error status codes points the finger at clients.                              |
| 5xx: Server Error  | The server takes responsibility for these error status codes.                                  |

Guidelines:
* 200 (“OK”) should be used to indicate nonspecific success.
* 200 (“OK”) must not be used to communicate errors in the response body.
* 201 (“Created”) must be used to indicate successful resource creation. The new URI should be returned in the 
response’s [Location](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.30) header.
* 202 (“Accepted”) must be used to indicate successful start of an asynchronous action (Controller resources may send 
202 responses, but other resource types should not).
* 301 (“Moved Permanently”) should be used to relocate resources. The new URI should be returned in the response’s 
[Location](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.30) header.
* 302 (“Found”) should not be used.
* 303 (“See Other”) should be used to refer the client to a different URI.
* 304 (“Not Modified”) should be used to preserve bandwidth.
* 307 (“Temporary Redirect”) should be used to tell clients to resubmit the request to another URI.
* 400 (“Bad Request”) may be used to indicate nonspecific failure
* 401 (“Unauthorized”) must be used when there is a problem with the client’s credentials.
* 403 (“Forbidden”) should be used to forbid access regardless of authorisation state.
* 404 (“Not Found”) must be used when a client’s URI cannot be mapped to a resource.
* 405 (“Method Not Allowed”) must be used when the HTTP method is not supported.
* 406 (“Not Acceptable”) must be used when the requested media type cannot be served.
* 409 (“Conflict”) should be used to indicate a violation of resource state.
* 412 (“Precondition Failed”) should be used to support conditional operations.
* 415 (“Unsupported Media Type”) must be used when the media type of a request’s payload cannot be processed.
* 500 (“Internal Server Error”) should be used to indicate API malfunction.

## Versioning

A REST API is composed of an assembly of interlinked resources: its resource model. The version of each resource is 
conveyed through its representational form and state.

Guidelines:
* Schemas should be used to manage representational form versions.
* Entity tags should be used to manage representational state versions

## Security

Many RESTful APIs expose resources that are associated with a specific client and/or user. For example, a REST API’s 
documents may contain private information and its controllers may expose operations intended to be executed by a 
restricted audience.

Guidelines:
* API management solutions may be used to protect resources. See: https://github.com/Mashape/kong
* OAuth may be used to protect resources. See: https://getkong.org/plugins/

### Additional Resources
* [The GitHub API v3](https://developer.github.com/v3/)
* [Google Cloud Platform: API Design Guide](https://cloud.google.com/apis/design/)
* [Best Practices for Designing a Pragmatic RESTful API](http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api)

