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

### The jQuery Function

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

  * There's a short-hand way of writing a **DOM ready** call-back, by just passing a callback function directly to the jQuery function.

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

### jQuery Objects

  * In the example above, both lines of code identify a DOM element via the `#content` id attribute, return that element as a jQuery object, and assign it to a variable
  * When using an id selector, the returned object represents a single element (since ids are unique within the webpage).
  * For other types of selectors, e.g. class selectors, the returned object may represent many elements (as a collection)
  * The AirBNB style guide recommends prefixing variable names with a `$` if they are intended to reference jQuery objects.
  * If you are unsure whether a variable or property references a jQuery object, you can check the `jquery` property of the object.

**Example**

```
console.log($content.jquery);
```

  * If the property exists and is a version number that matches the jQuery vcersion, you know you're dealing with an object produced by jQuery.

### Working with jQuery Objects

  * Once you obtain an object from jQuery, you can perform a variety of tasks with it, such as getting or setting the value of a css property.
  * Some jQuery methods, such as `css`, have both getter and setter forms.

**Example**

```
$content.css('font-size'); // getter
$content.css('font-size', '18px'); // setter
```

  * Note that the second argument of the setter form is a string that includes an appropriate measurement unit.

  * Other jQuery methods, such as `width` and `height`, have a single form that is used for both getting and setting.

**Example**

```
$content.width(); // getter
$content.width(400); // setter
$content.width('20em'); // setter specifying a measurement unit
```

  * Note: `width` and `height` getters return a numeric value (the amount of pixels). The setters can take either a numeric value (assumed to be pixels), or a string that includes a measurement unit.

### Setting Multiple Properties

  * Rather than using repeated method calls to set different properties you can either chain calls, or pass an object (with key/ value pairs) as an argument.

**Example**

```
// multiple calls
$content.css('font-size', '18px');
$content.css('color', '#b00b00');

// chaining
$content.css('font-size', '18px').css('color', '#b00b00');

// object argument
$content.css({
  'font-size': '18px',
  color: '#b00b00'
});
```

  * Note in the example above that `'font-size'` is quoted as a string, but `color` is not. This is because `font-size` contains a hyphen that jQuery would interpret as subtraction. You can alternatively use the camel-cased version `fontSize`.
  * The same thing applies to other hyphenated properties: `background-color` and `backgroundColor`, `border-width` and `borderWidth`.

### Working with multiple elements

  * When used with selectors other than the ID selector, the return value of jQuery is a collection.

**Example**

```
var $lis = $('li');
```

  * In the above example, `$lis` represent all the list item elements on the page.
  * You can check on the number of elements in a collection by calling `length` on it

**Example**

```
$lis.length; // 3
```

  * Working with a collection means that you can affect all the elements at once.

**Example**

```
$lis.css('background-color', 'red'); // changes the background-color of all list elements
```

### jQuery Selectors and Convenience Methods

  * You can use most CSS selectors 'as-is' to access elements with jQuery
  * jQuery also provides a number of selectors that are unique to jQuery
  * All of the available selectors are listed in the [jQuery API](http://api.jquery.com/category/selectors/) documentation

  * jQuery also provides a number of convenience methods attached to the jQuery object, such as `$.isArray()`, `$.isFunction()`, etc. for checking some object types. `$.merge()` for concatenating two array, or `$.map()` for creating new arrays.
  * One particularly important method of the jQuery object is `$.ajax()` which lets you make AJAX requests in a relatively straightforward fashion that is supported cross-browser.
  * A number of these utility methods are listed [here](https://api.jquery.com/category/utilities/) in the docs.


<a name="events"></a>
## jQuery Events

  * Most JavaScript and jQuery code executes when a user interacts with a site.
  * The most common way to respond to user actions is to bind events with an event listener and then handle them when the event occurs
  * An event listener is a function that gets executed when a particular event occurs.
  * jQuery uses the `$.on()` method to bind event listeners to jQuery objects.

**Example**

```
$(function() {
  $('a').on('click', function() {
    // do something when link is clicked
  });
});
```

  * In the above example, the `$('a')` call returns all the `a` elements on the page. The `on()` then binds an event handler to those elements which responds when one of them is 'clicked'. The body of the callback function can be defined to do something as a result of the click.
  * We can pass the `event` object to the callback function as an argument. The event can then be referenced within the calback.

**Example**

```
$('a').on('click', function(event) {
  event.preventDefault();
});
```

  * In the above example we are calling `preventDefault()` on the event in order to prevent the link from executing its default behaviour. We can also use `stopPropogation()` here to stop the event from bubbling further.

### Convenience Methods

  * jQuery has convenience methods for many events. These methods have the same name as the event types, take a callback function as an argument, and allow for less verbose code.

**Example**

```
$('a').click(function(e) {
  e.preventDefault();
  // rest of callback code
});

$('form').submit(function(e) {
  e.preventDefault();
  // rest of callback code
});
```

### Further Reading

  * This [Medium article](https://medium.com/@sak1986/event-binding-in-jquery-daf902be7c58) provides some more detail on what happens when you bind an event using jQuery.

<a name="dom-traversal"></a>
## jQuery DOM Traversal

<a name="chrome-debugging-tools"></a>
## Chrome Debugging Tools for Front-End Development
