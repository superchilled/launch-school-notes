# Making HTTP Requests from JavaScript

  * [Web APIs](#web-apis)
  * [Network Programing in JavaScript](#network-programming-js)
  * [Making Requests with XMLHttpRequest](#making-requests-XHR)
  * [XMLHttpRequest Events](#XHR-Events)
  * [Data Serialization](#data-serialization)
  * [Usage Examples](#usage-examples)
  * [Cross-Domain XMLHttpRequest with CORS](#cross-domain-xhr-cors)

<a name="web-apis"></a>
## Web APIs

### Defining APIs

  * An **API** or **Application Programming Interface** provides a way for computer systems to interact with each other.
  * There are many types of API, for example:
    * Every programming language has a built in API that is used to write programs in that language
    * Mobile devices provide APIs to provide access to location or other sensor data
    * Operating Systems have APIs used by programs to open files, access memory, interact with input and output devices, etc.
  * While the types and use of APIs vary greatly, what they all have in common is **providing functionality for use by another program**
  * **Web APIs** are APIs built with web technologies. They are sometimes called **HTTP APIs** because like the web they operate over HTTP.
  * When discussing APIs it is usual to distinguish between the system to which the API *belongs*, and the external service (or user) that will use the API:
    * An API **provider** is the system that provides the API for other parties to use, (e.g. GitHub is the *provider* of the GitHub API)
    * An API **consumer** is the system that *uses* the API to accomplish some task

### What APIs can Do

  * One of the most common use-cases for web APIs is **sharing data between systems**. At a certain level, all APIs are used to transfer data between systems.
  * Another common use-case is **enabling automation**. For example, if you have two systems that manage different parts of your operation, such as a customer order managment system and an email marketing system, by leveraging the APIs of those systems you can more precisely market to your customers based on their purchase history. Prior to APIs this sort of task was a lot less precise and perhaps accomplished using lots of human input and manual intervention.
  * Web APIs also allow you to **leverage existing services** in order to build new services. For example, if you wanted to build a system that required geo-located mapping, rather than building the functionality yourself you could use the Google Maps API and build on top of it. Using APIs in this way enable developers to build their applications on top of specialized systems, allowing them to focus on their actual objectives and not worry about the complexities of every part of the system.

### Access to APIs

  * **Public APIs** are intended for consumption outside the organization that provides them. Many social media sites, for example, provide public APIs that enable third-party programs to interact with them
  * **Private APIs** are intended only for internal use. For example, the Google search page uses a private API to get a list of search suggestions to display while a user is entering search terms
  * Providers of public APIs can and do dictate the conditions of usage for their API(s). Many require API consumers to have accounts (sometimes as part of a developer program) and to verify that account during requests to the API by including authentication data or parameters.
  * The data accessed via public APIs carries with it ethical and legal responsibilites. Many API providers require developers to agree to **terms and conditions** of use before they are granted access. It is important to understand what is and isn't allowed with respect to API data, in particular the following:
    * What restrictions does the API place on your use of its data?
    * Is the API exposing and data that could be linked back to a specific person?
    * Does the API have rate limits, and if so what are they? (Many APIs limit how many requests can be sent from a single user or application within a given timeframe).

### HTTP and URLs

  * Web APIs are based on the HTTP protocol in the same way that, say, web browsers are.
  * Instead of a human typing a url into the browser address bar (as with a web browser), with a Web API some code makes the request to the url.
  * The interaction between the consumer code and the provider API are still based on the **request-response** cycle. The consumer code sends a request, and the API sends back the response.
  * The request and response contain all the usual elements such as:
    * Request: request line including request method, path and params, http version; request headers; request body
    * Response: response line including http version, response code and description; response headers; response body.
  * When working with web APIs the response body is often in a format like JSON, rather than HTML to be parsed by a browser. In such cases, the `Content-Type` response header will generally have a value like `application/json; charset=UTF-8`
  * Since Web APIs use HTTP, working with them involves using URLs. URLs represent *where* a particular resource is and *how* it can be accessed
  * Some URL paths used in API documentation include identifiers for specific resources. Certain segments of these paths will be placeholders whose value needs to be filled in as part of the request path

**Examples**

```
/api/v1/users/:id/profile
/api/:version/products/:id
```

  * The form of path in the second example can be referred to as *nested*, because the route for `products` is nested underneath the route for `version`
  * The specific placeholder used within the used within a path isn't important as long as it is unique within the path.
  * It is common for the final placeholder to be name `:id`, while other placeholders have names prefixed with the resource they map to

### Media Types

  * The internet has a number of shared markup languages and data formats for transferrig information between computers or systems.
  * 

<a name="network-programming-js"></a>
## Network Programing in JavaScript

<a name="making-requests-XHR"></a>
## Making Requests with XMLHttpRequest

<a name="XHR-Events"></a>
## XMLHttpRequest Events

<a name="data-serialization"></a>
## Data Serialization

<a name="usage-examples"></a>
## Usage Examples

<a name="cross-domain-xhr-cors"></a>
## Cross-Domain XMLHttpRequest with CORS
