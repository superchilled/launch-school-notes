# Making HTTP Requests from JavaScript

  * [Overview](#overview)
  * [Web APIs](#web-apis)
  * [Network Programing in JavaScript](#network-programming-js)
  * [Making Requests with XMLHttpRequest](#making-requests-XHR)
  * [XMLHttpRequest Events](#XHR-Events)
  * [Data Serialization](#data-serialization)
  * [Usage Examples](#usage-examples)
  * [Cross-Domain XMLHttpRequest with CORS](#cross-domain-xhr-cors)

<a name="overview"></a>
## Overview

  * AJAX is a technique used to exchange data between a browser and web server without requiring a complete page reload
  * Modern browsers provide an API called `XMLHttpRequest` (or `XHR`) to send AJAX requests
  * Some modern web applications rely *exclusively* on JavaScript and `XHR` to communicate with the server and use the responses to build and manipulate the DOM. They are known as *single-page application* (SPAs)

  * Sending requests through `XMLHttpRequest` mainly involves the following steps:
    *

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

  * The internet has a number of shared markup languages and data formats for transferring information between computers or systems. One example is HTML, used to create all web pages.
  * HTML is one of many different **media types**, also called **content types** or **MIME types**, supported by modern web browsers.
  * HTTP response header `Content-Type` tells the browser how to interpret the body of the response. For example, if `Content-Type` is set to `text/html`, the body will be interpreted as HTML and the browser will parse the HTML to render it in the viewport.
  * The character set, or `charset`, is often included as a value for the `Content-Type` header.

**Example**

```
Content-Type: text/html; charset=UTF-8
```

  * Other text media types include:
    * `text/plain` for plain text responses
    * `text/css` for css stylesheets
    * `text/javascript`for JavaScript files
  * There are also many non-text media types for things like image files, PDF documents, sound files, ZIP files, etc.
  * Wikipedia has [a list](https://en.wikipedia.org/wiki/Media_type#List_of_common_media_types) of commonly used media types.

### Data Serialization Formats

  * Since APIs are generally used to allow systems to communicate by passing structured data between each other, the content of requests need to use a format and media type that can represent hierarchical data. These formats are known as data serialization formats
  * A **data serialization format** describes a way to represent data in a form that can be more easily or efficiently stored or transferred
  * **XML**, or **extensible markup language** is one such format. It was commonly used by APIs in the past, and is still used by some today.
  * **JSON** or **JavaScript Object Notation** is perhaps the most popular data serialization format used by web APIs today. The syntax used by JSON is based on the way object literals are written in JavaScript, the main difference being that the property or key names are strings.

### Fetching Resources

  * In API terms, a **resource** is the representation of some grouping of data. For example, a blogging API might have resources like posts and comments.
  * Every resource in a web API must have a unique path that can be used to identify and access it. A single resource is often referred to as a **element**
  * To fetch a resource, you issue a `GET` request to url which indicates the location of the resource

**Example**

For a JSON API, the body of a reponse to fetching a single resource might look like this:

```
{
  "id": 1,
  "name": "Karl Lingiah",
  "username": "superchilled"
}
```

  * APIs can also be used to fetch a group of resources. Groups of resources are usually referred to as **collections**. The collection is returned in an array-like structure contaning the individual elements.

**Example**

For a JSON API, the body of a reponse to fetching a group of resources might look like this:

```
[
  {
    "id": 1,
    "name": "Karl Lingiah",
    "username": "superchilled"
  },
  {
    "id": 2,
    "name": "Keri Silver",
    "username": "ksilver"
  },
  {
    "id": 3,
    "name": "Peter Parker",
    "username": "Spiderman"
  }
]
```

  * When deserialized in a programming environment, the collection will be an array containing three objects.

  * As a rule of thumb:
    * If a url or path ends with pluralised word like `/users` or `/products`, then this is for a collection, and the response body will contain multiple elements
    * If a url or path ends with a plural word followed by a slash and an identifier, e.g. `/users/1`, `/products/3`, then this is likely to be for a single element

### Creating Resources

  * As well as fetching resources, APIs often allow you to create new resources.
  * Rather than a `GET` request, you issue a `POST` request to the url for where the resource should be created
  * The API will require some data about the new resource in order to create it. This data is often the result of a form submission, and is passed in the request body.
  * A response for a `POST` request will often have a status of `201 Created` is the request was successful

### Updating Resources

  * Existing resources can also be updated. Rather than create a brand new resource, you can update the value for a particular key, or create a brand new key-value pair.
  * The `PUT` http method is used rather than `POST` for updating resources
  * For updating a resource you need to use a path that indicates a specific resource, e.g. `/products/1`
  * According to the HTTP specification, `PUT` requests require a complete representation of the resource being updated. This means that every key value pair should be specified, even for those that are not having their value changed. If a key-value pair is not specified, then the value for that key *should*, strictly speaking, be set to `null`.
  * Most APIs don't strictly follow this requirement, however, anmd only modify parameters which are specified in the request. This is technically the behaviour of the `PATCH` http method, but most APIs still use `PUT`.

### Deleting Resources

  * As well as fetching, creating, and updating resources, you can also delete them.
  * Deleting resources is carried out using the `DELETE` http method.
  * As with updating, the path should indicate a specific resource to delete.
  * The `204 No Content` status code is often used to respond to `DELETE` requests. This code is generally used when it doesn't make sense to return anything in the response body. When deleting a resource, there is nothing really to return, so `204 No Content` makes sense in this context.

### HTTP Accept Header

  * Some request headers are particularly important when working with Web APIs. One of these is the `Accept` header. This specifies what media types the client will acccept in a response to the request.
    * An `Accept` value of `application/json` means that the client will expect, and treat, any response body as JSON.
    * An `Accept` value of `*/*` means that the client will accept any media type as a reponse.

### HTTP Response Headers

  * Some common response headers that are used by Web APIs are:
    * `Access-Control-Allow-Origin`. This lists the domains that can access a resource using CORS.
    * `Allow`. Indicates which http methods are allowed to be used on a resource. Used with a `405 Method Not Allowed` response to a request with an invalid http method.
    * `Content-Length`. The length of the response body in bytes.
    * `Content-Type`. Describes the media type or format of the response body.
    * `ETag`. Used to identify a specific version of a resource for APIs that support this. Example: `ETag: "6df23dc03f9b54cc"`. The value returned in this header can be sent with future requests to the same url in the `If-None-Match` request header. If the resource hasn't changed in the meantime, the response should be a `304 Not Modified` status. If it has changed, the response should include the entire updated resource, and its new `ETag`. This is used to avoid fetching and processing data that has not been updated since last time it was accessed.
    * `Last-Modified`. The value should be a date-time value indicating the last time the requested resource was modified. This date and time can be included in future requests to the same url in the `If-Modified-Since` header. If the resource hasn't changed in the meantime, the response should be a `304 Not Modified` status. If it has changed, the response should include the entire updated resource, and a new value for `Last-Modified`. This is used to avoid fetching and processing data that has not been updated since last time it was accessed.
    * `WWW-Authenticate`. Indicates the type of authentication required to access a resource. Example: `WWW-Authenticate: Basic`.
    * `X-*` Headers. Giving header names that begin with `X-` is a convention used with non-standard headers. These headers are often API or application-specific. For example, GitHub uses some `X-*` headers to control rate limiting: `X-RateLimit-Limit: 60`, `X-RateLimit-Remaining: 57`, `X-RateLimit-Reset: 1413592144`

### Common Errors

  * HTTP requests don't always complete successfully.
  * A failure can be due to things like:
    * A request being incomplete or containing an invalid value
    * A problem on the server
    * A network connection issue
  * Sometimes errors result in no more information than a status code
    * A status code like `422 Unprocessable Entity` can indicate a validation issue (e.g. missing parameters) with the data that was passed to the API. This mostly occurs when creating or updating a resource.
    * Depending on the API, the response body will sometimes contain additional information about the error, such as which data failed validation.
  * Another common error is trying to access a resource that doesn't exist. This will usually result in a `404 Not Found` status. This could be the result of the resource actually not existing, an incorrect url, or an authenticaton issue (authentication issues might sometimes use `401` or `403` error codes, but for security reasons an API might not want to expose the existence of a resource to someone not authorised to access it)
  * Another cause of errors is sending data in the wrong format for teh expected media type
  * Rate limiting can also be an issue. Many APIs limit the number of requests a user can make within a specified time-period. The status code for this kind of issue varies, but is often `403 Forbidden`.
  * Server errors will generally result in an error code in the `5xx` range. Resolving server errors from the client end isn't usually possible, as you would need access to the server to do so.

<a name="network-programming-js"></a>
## Network Programing in JavaScript

  * The HTTP request-response cycle is the foundation of web applications
  * The standard client-server interaction via request response has been that the client (i.e. browser) requests and entire html page at once from the server and then renders that page in the viewport.
  * The process looks somewhat like this:
    1. User clicks a link in a web page
    2. Browser sends http `GET` request to server
    3. Server returns html for **entire page**
    4. Browser parsers and displays the page
  * A couple of things to note:
    * When a user clicks a link, the browser automatically issues a `GET` request
    * When the browser receives the http response, it automatically parses it and displays it in the viewport

  * As web applications became more interactive, the need to reload the entire page becomes a limiting factor.
  * Developers started using techniques to work around this and only reload part of the page
  * The method for doing this known as **AJAX**: Asynchronous JavaScript And XML. This technique fetches data, typically HTML or XML, and updates *parts* of a page rather performing a full page load.
  * This process typically looks something like this:
    1. User action triggers an event listener
    2. JavaScript event handler send http request to server using `XMLHttpRequest`
    3. Server returns some data (typically a 'chunk' of HTML)
    4. JavaScript callback manipulates the DOM to insert the HTML chunk into the page.
  * A couple of things to note:
    * The web browser doesn't make an automatic http request, instead this request is initiated using JavaScript (typically via an event handler)
    * When the response is received, JavaScript uses the response body to update the page as needed rather than an entire page load occurring

### Single Page Applications

  * In front-end development, it has become increasingly common to make HTTP requests outside of the main HTML page load.
  * Some modern web applications fetch data in a serialized format (often JSON) and create the DOM entirely using JavaScript running in the client browser. These applications are usually referred to as **Single Page Applications** since they often run entirely within a single HTML 'page' (i.e. there are no 'page loads' required after the intial one).

<a name="making-requests-XHR"></a>
## Making Requests with XMLHttpRequest

  * We can use the `XMLHttpRequest` object and its methods to manage HTTP requests in JavaScript.
  * This object is part of the browser API rather than the JavaScript language.
  * To send a request using `XMLHttpRequest`, we must provide the same parameters as usual: method, host, and path (with optional headers and body)

**Example**

```
var myRequest = new XMLHttpRequest(); // instantiates XMLHttpRequest object
request.open('GET', '/somepath'); // sets the http method and URL on the XMLHttpRequest object
request.send(); // sends the request
```

  * Once the request completes, we can check the values of certain properties of the request object (prior to completion the values for these properties is `null`)

**Example**

```
request.responseText;                       // body of response, typically some HTML or JSON
request.status;                             // status code of response, e.g. 200
request.statusText;                         // status text from response, e.g. OK

request.getResponseHeader('Content-Type');  // value for the `Content-Type` response header
```

  * The `send()` method of `XMLHttpRequest` is *asynchronous*, so code execution continues without waiting for it to complete.
  * The `XMLHttpRequest` object uses event listeners to send notifications when the request completes and provides access to the response returned by the server or other remote system.

**Example**

```
request.addEventListener('load', function(event) {
  var request = event.target; // the event passed to the callback function has a
                              // target property pointing to the XMLHttpRequest object
  // rest of callback code here
});
```

  * In the above example, an event listener is added to the request object so that the callback is only executed when the `load` event for that request occurs.

### XMLHttpRequest Methods

  * The following methods can be called on an `XMLHttpRequest` object:
    * `open(method, url)`: opens a connection to `url` using `method`
    * `send(data)`: sends the request, optionally sending `data`
    * `setRequestHeader(header, value)`: sets an http header specified by `header` to the value specified by `value` for the `XMLHttpRequest` object
    * `abort()`: cancels and active request
    * `getResponseHeader(header)`: returns the response's value for a header specified by `header`

### XMLHttpRequest Properties

  * The following properties, among others, exist on `XMLHttpRequest` objects:
    * `timeout`: a writable property that sets the maximum time (in milliseconds) that a request can take to complete
    * `readyState`: a read-only property that indicats what state the request is in
    * `responseText`: a read-only property whose value is the raw text of the response body. Defaults to `null`
    * `response`: a read-only property whose value is the *parsed* content of the response. Defaults to `null`

### Debugging in Chrome

  * When debugging code that uses `XMLHttpRequest` in Chrome Dev Tools, it is useful to check the `Log XMLHttpRequests` option under Settings > Preferences
  * Checking this option means that you can see when a request completes in the JavaScript console

<a name="XHR-Events"></a>
## XMLHttpRequest Events

  * Two main events fire during an `XMLHttpRequest` cycle:
    * `loadstart`: this fires when the request is sent to the server
    * `loadend`: this fires when the response has loaded and all other events have fired. It is the last `XMLHttpRequest` event to fire.
  * Before `loadend` triggers, another event will fire based on whether the request succeeeded or not:
    * `load`: fires when a complete response has loaded
    * `abort`: fires when the request was interrupted before it could complete
    * `error`: fires when an error occured with the request
    * `timeout`: fires when a response wasn't received before the timeout preiod ended
  * One thing to note is that the browser considers a request to be successful as long as it receives a complete response, even if that response has a non-200 status code. `XMLHttpRequest` does not consider the actual status of the response in determining a successful outcome. It is the responsibility of the application code to determine whether a request was 'successful' in status terms by examining the response within a `load` event handler.
  * A couple of other useful `XMLHttpRequest` events are:
    * `readstatechange`: fires when the `readyState` attribute of a document has changed.
    * `progress`: fires to indicate that an operation is in progress.

<a name="data-serialization"></a>
## Data Serialization

  * JavaScript applications that run in a browser must *serialize* data when communicating with remote systems (e.g. APIs). This lets the client and server transfer data in a specified format that can be understood by both.
  * Many applications use query strings and url encoding as a form of data Serialization.
    * A query string consists of one or more `name=value` pairs delimited by the `&` character.
    * Non-alphanumeric characters must be encoded before being passed in a query string, e.g. spaces are encoded as either `%20` or `+`
    * JavaScript provides a built-in function `encodeURIComponent` that lets you encode a name or value using this format.
    * Once you have a properly encoded string, you can append it to a `GET` request's path
    * URL encoding also works with `POST` requests, but you must include the `Content-Type` header with a value of `application/x-www-form-urlencoded`, also the encoded name-value string is placed in the request body rather than appended to the url.
  * `POST` requests use multipart form formats for forms that include file uploads or that use `FormData` objects to collect data.
    * This isn't strictly an encoding format since nothing is really 'encoded', instead each name-value pair is placed in its own separate section of the request body, and a special boundary delimiter, specified as a value of the `Content-Type` request header, is used to separate each 'part'. The final delimiter ends with `--` to mark the end of the multipart content

**Example**

```
POST /path HTTP/1.1
Host: example.test
Content-Length: 267
Content-Type: multipart/form-data; boundary=----WebKitFormBoundarywDbHM6i57QWyAWro
Accept: */*

------WebKitFormBoundarywDbHM6i57QWyAWro
Content-Disposition: form-data; name="title"

Do Androids Dream of Electric Sheep?
------WebKitFormBoundarywDbHM6i57QWyAWro
Content-Disposition: form-data; name="year"

1968
------WebKitFormBoundarywDbHM6i57QWyAWro--
```

  * JSON (JavaScript Object Notation) is a popular data serialization format used by APIs.
    * JSON doesn't provide native support for complex data types like dates and times, instead you must represent such values as strings.
    * A `GET` request can return JSON, but you must use `POST` to send JSON to the server, using a `Content-Type` header of `application/json`, and an optional `charset` (though it is best practice to include it)

**Example**

```
POST /path HTTP/1.1
Host: example.test
Content-Length: 62
Content-Type: application/json; charset=utf-8
Accept: */*

{"title":"Do Androids Dream of Electric Sheep?","year":"1968"}
```

<a name="usage-examples"></a>
## Usage Examples

### Loading HTML via XHR

  * We can have an HTML element like a `<div>`, and instead of having markup already in that div, we can populate the div using html markup that we fetch using JavaScript and `XMLHttpRequest`

**Example**

```
<h1>My cool shop!</h>

<div id='products'></div>

<script>
document.addEventListener('DOMContentLoaded', function() {
  var request = new XMLHttpRequest();
  request.open('GET', 'https://mycoolshop.com/api/products');
  request.send();

  request.addEventListener('load', function(event) {
    var productsDiv = document.getElementById('products');
    productsDiv.innerHTML = request.response; // the response here will be html
  });
});
</script>
```

### Submitting a Form via XHR

  * There are three steps to submitting a form using JavaScript:
    1. Serialize the form data
    2. Send the request using `XMLHttpRequest`
    3. Handle the response

**Example**

```
var request = new XMLHttpRequest();
request.open('POST', 'https://mytodoapp.com/todos');
request.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");

var data = 'task=buy%20milk&date=22-12-2018'

request.send(data);
```

  * In the above example, since this is a post request, we set the `Content-Type` request header to `application/x-www-form-urlencoded` in order to indicate that we are passing data in a urlencoded format.
  * Additionally we pass an argument (the encoded data) to the `send` method in order for that data to be included in the request body.

  * When working with HTML forms, we can use the DOM object `HTMLFormElement`. This has a property called `elements`, which returns a collection of all the form controls contained within the `<form>`.
  * This can be used in conjunction with an event listener that fires when a user submits the form. We can iterate through the form elements and serialize the data in order for it to be sent in a request.

**Example**

```
var form = document.getElementById('form');

form.addEventListener('submit', function(event) {
  // prevent the browser from submitting the form
  event.preventDefault();

  // access the inputs using form.elements and serialize into a string
  var keysAndValues = [];

  for (var i = 0; i < form.elements.length; i++) {
    var element = form.elements[i];
    var key;
    var value;

    if (element.type !== 'submit') {
      key = encodeURIComponent(element.name);
      value = encodeURIComponent(element.value);
      keysAndValues.push(key + '=' + value);
    }
  }

  var data = keysAndValues.join('&');

  // code to submit the data

});
```

  * A process like the example above is laborious and can be error-prone. Modern browsers provide a built-in API to help with this process: `FormData`.

**Example**

```
var form = document.getElementById('form');

form.addEventListener('submit', function(event) {
  // prevent the browser from submitting the form
  event.preventDefault();

  var data = new FormData(form);

  // code to submit the data
});
```

  * Using `FormData` makes serializing the form's data much simpler.
  * One thing to note about `FormData` is that is uses the *multipart* data serialization format. This is the same serialization format used for file inputs in web forms. Since `FormData` can handle file uploads, it makes sense for it to default to this format.

### Loading JSON via XHR

  * Retrieving HTML chunks or fragments from a server and inserting them into an existing webpage works well for applications that primarily sue server-side rendering to generate the user interface.
  * Sometimes it makes more sense to load data in a primitive data structure (e.g. JSON) and render it with client-side code.
  * Modern browser make this process a bit easier by providing native support for fetching JSON data. To do this we can take advantage of the `responseType` property of `XMLHttpRequest` to tell the browser how to handle the data that it receives in the response (i.e. the response body).

**Example**

```
var request = new XMLHttpRequest();
request.open('GET', 'https://api.github.com/repos/rails/rails');
request.responseType = 'json';

request.addEventListener('load', function(event) {
  // request.response will be the result of parsing the JSON response body
  // or null if the body couldn't be parsed or another error occurred

  var data = request.response;
});

request.send();
```

  * Setting the `responseType` to `json` means we don't require an additional step of parsing the response as JSON using `JSON.parse()`. A benefit of doing this means that there is less potential for error in our code (for example if `JSON.parse()` is unable to parse the response as JSON), or at least we have to do less error checking.

### Sending JSON via XHR

  * Sending JSON data to the server is similar to submitting a form, in that the steps to do so are similar:
    1. Serialize the form data **into valid JSON**
    2. Send the request using `XMLHttpRequest` with a `Content-Type: application/json` header
    3. Handle the response

**Example**

```
var request = new XMLHttpRequest();
request.open('POST', 'https://mytodoapp.com/todos');
request.setRequestHeader("Content-Type", "application/json");

var data = { task: 'buy milk', date: '22-12-2018' };
var json = JSON.stringify(data);

request.send(json);
```

  * The difference between this and sending non-JSON data is the way that the data is first defined as a JSON object and then coverted to a String using the `JSON.stringify` method.
  * The other difference is the value set for the `Content-Type` request header.

<a name="cross-domain-xhr-cors"></a>
## Cross-Domain XMLHttpRequest with CORS

### Cross-Origin Requests

  * The combination of a URL's *scheme*, *hostname*, and *port* define the resource's **origin**.
  * A *cross-origin request* occurs when the page tries to access a resource from a different origin.
  * For example, if we are at the following url: `http://mywebsite.com/page1`, then requests to the folowing resources would be considered cross-origin requests:
    * `https://mywebsite.com/page2`: this has a different *scheme* (`https` instead of `http`)
    * `http://mywebsite.com:4000/page1`: this has a differnt *port number* (`4000` instead of the default `80`)
    * `http://anotherwebsite.com/page1`: this has a different *host* (`anotherwebsite` instead of `mywebsite`)
  * Cross-origin requests, in particular cross-domain requests, have security vulnerabilities that can be exploited to launch attacks such as XSS and CSFR attacks.

### Cross-Origin Requests with XHR

  * By default, and `XHR` object can't send **cross-origin** requests.
    * All browsers implement a security feature called the 'same-origin policy'. It prevents `XMLHttpRequest` from making cross-domain requests.
  * To get around the 'same-origin policy', APIs that are made available for public consumption use the 'Cross-Origin Resource Sharing' (CORS) mechanism. This allows cross-origin access to resources.

### CORS

  * Cross-Origin Resource Sharing is a W3C specification that defines how the browser and server must communicate when accessing resources across different origins.
  * Applications use custom HTTP request and response header to implement the mechanism.
    * According to the specification, every `XMLHttpRequest` sent by the browser must have an `Origin` header.
    * The server uses the `Origin` header sent by the browser to determine whether it should send a corresponding header in response.
    * The browser automatically adds the `Origin` header to an `XHR` request.
    * When the server receives the request, it cheks the `Origin` header and determines if it came from and origin that is allowed to see the response.
      * If a response is allowed, the server send the response with a `Access-Control-Allow-Origin` header that contains the same origin.
      * If the server wants to make a resource available to everyone, it can send the same header with a vlaue of `*`.
      * When the browser sees the `Access-Control-Allow-Origin` header, it checks to see if the value contains the correct origin or `*`. If it does, the browser makes the response avaialble to the application, otherwise it raises an error.
      * If the server sends a response *without* the `Access-Control-Allow-Origin` header, the browser will raise an error and not let the script access the response.
  * The Cross-Origin Resource Sharing (CORS) specification fulfills the need forlegitimate Cross-Origin requests. It gives a standard way to access resources from different origins without the security problems associatedwith cross-origin requests.
