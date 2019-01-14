# jQuery

  * [jQuery Overview](#overview)
  * [jQuery Events](#events)
  * [jQuery DOM Traversal](#dom-traversal)
  * [Chrome Debugging Tools](#chrome-debugging-tools)

<a name="overview"></a>
## jQuery Overview

  * jQuery is a JavaScript library that provides a convenient and consistent API across most broswers and platfroms
  * You can use vanilla JavaScript and the DOM API to accomplish much of what jQuery provides, but differences between DOM implementations have historically made it difficult to share code between browsers
  * While browser compatibility has improved, many projects still use jQuery
  * jQuery is not a separate language from JavaScript. Under the hood, jquery still uses JavaScript to accomplish what it does; it just uses a layer of abstraction over vanilla JS to provide a uniform interface
  * When interacting with HTML via the DOM, you need to work around bugs and inconsistencies that exist in different browsers; jQuery can do all the hard work for you by providing a simple interface that abstracts away the complexity.

  * At its core, jQuery is a *function* that wraps a collection of DOM elements and convenience methods into an *object*, a 'jQuery object'.
  * You can call the function to select the elements you need to process, and manipulate the object or its elements with methods built in to the object.
  * Since most of jQuery acts on DOM elements, we must wait for the page to load before executing any code. jQuery provides us with a **DOM ready callback** for this very purpose.

**Example**

```
$(document).ready(function() {
  // the DOM is loaded and ready, img tags not ready
});
```

  * In the above example, the callback we pass to the `ready` method executes after the document, and its dependencies, finish loading. Note: `ready` doesn't wait for the browser to load inmages included via `<img>` tags.

  * If you want to delay execution of your code until the the window finishes loading, you can use the `load` method instead, calling it on a `window` jQuery object.

**Example**

```
$(window).load(function() {
  // DOM loaded and ready, img tags loaded and ready
});
```

  * There's a short-hand way of writing a **DOM ready** call-back, by just passing a calback function directly to the jQuery function.

**Example**

```
$(function() {
  // DOM loaded and ready, img tags not ready  
});
```

  * The jQuery function works one of two ways, depending on the argument passed to it.
    * If the argument is a DOM element or astring that identifies a DOM element, it wraps a collection of jQuery objects and returns them
    * If the argument is a function, jQuery uses that function as a callback when the DOM is ready

  * It is common practice to use `$` as an alias for `jQuery` when calling the jQuery function, but they are functionally identical.

**Example**

```
var $content = jQuery('#content');
var $sameContent = $('#content');
```

  * In the example above, both lines of code identify a DOM element via the `#content` id attribute, return that alement as a jQuery object, and assign it to a variable
  * When using an id selector, the returned object represents a single element (since ids are unique within the webpage).
  * For other types of selectors, e.g. class selectors, the returned object may represent many elements (as a collection)
  * The AirBNB style guide recommends prefixing variable names with a `$` if they are intended to reference jQuery objects.
  

<a name="events"></a>
## jQuery Events

<a name="dom-traversal"></a>
## jQuery DOM Traversal

<a name="chrome-debugging-tools"></a>
## Chrome Debugging Tools for Front-End Development
