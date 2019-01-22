# jQuery

  * [jQuery Overview](#overview)
  * [jQuery Events](#events)
  * [jQuery DOM Traversal](#dom-traversal)
  * [Chrome Debugging Tools](#chrome-debugging-tools)
  * [jQuery Animations](#jquery-animations)
  * [HTML Data Attributes](html-data-attributes)

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

  * Traversing the DOM is one of the most common actions taken using jQuery.
  * jQuery has a number of useful methods to help with DOM Traversal

### `find`

  * The `find` method searches the descendant elements of the element on which it is called and filters all the descendants based on an argument passed to it (usually a selector)
  * It is similar to the `children` method, except that `children` only travels down a single level in the DOM tree whereas `find` will traverse all levels.
  * [API Reference](https://api.jquery.com/find/)

**Example**

```
$('body').find('li');
```

  * The above example finds all list items in the body of the web page.

### `parent`

  * The `parent` method gets the parent of each element in the set it is called on. Optionally filtered by a selector
  * [API Reference](https://api.jquery.com/parent/)

**Example**

```
$('body').find('li').parent();
```

  * The above example would find the immediate parent of the all the list items in the body of the web page

### `closest`

  * The `closest` method finds the first ancestor element that matches the filter (usually a selector)
  * [API Reference](https://api.jquery.com/closest/)

**Example**

```
$('p').closest('div');
```

  * The above example finds the first `<div>` element that is an ancestor of each `<p>` element on the page.

### Other Useful Traversal Methods

  * `nextAll` selects all the sibling elements that follow the element on which it is called, optionally filtering on a selector. [API Reference](https://api.jquery.com/nextAll/)
  * `prevAll` is similar to `nextAll` but in the opposite direction; i.e. it selects all sibling elements *before* the element it was called on, optionally filtering on a selector. [API Reference](https://api.jquery.com/prevAll/)
  * `siblings` gets *all* the siblings (both before and after) the element on which it is called, optionally filtering on a selector. [API Reference](https://api.jquery.com/siblings/)
  * There are also a number of other traversal mehtods listed in the [jQuery API reference](http://api.jquery.com/category/traversing/)

<a name="chrome-debugging-tools"></a>
## Chrome Debugging Tools for Front-End Development

  * Chrome DevTools can be used for debugging purposes when developing in the front-end, or with jQuery. Other browsers provide similar tools.
  * The various features available are listed in the [Chrome DevTools User Guide](https://developers.google.com/web/tools/chrome-devtools/)

<a name="jquery-animations"></a>
## jQuery Animations

  * As well as taking care of cross-browser support for DOM methods and managing events, jQuery also provides a well-organised and succinct way of animating CSS properties, using effects to fade-in, slide-out, and toggle elements, among other things.
  * These animations methods are convenience methods that wrap other jQuery code so that we don't have to write an animation function each time.
  * These methods are listed in the Effects section of the [jQuery API documentation](http://api.jquery.com/category/effects/)
  * These methods generally all work in the same way: you can call them on an element, either with or without arguments (which can be used to provide additional information about the required animation) and jQuery will handle the required changing of the relevant CSS properties using an animation loop.

**Example**

```
var $p = $('p');
$p.fadeOut();
```

  * The above example will fade out all the paragraph elements on the page.
  * Methods can take a callback function which is executed when the animation completes.

**Example**

```
$p.fadeIn(250, function() {
  $(this).addClass('visible');
});
```

  * Methods accept arguments either positionally or as an options object.

**Example**

```
$p.slideToggle(400, 'linear', function() {
  console.log('Sliding complete!');
});

$p.slideToggle({
  duration: 400,
  easing: 'linear',
  complete: function() {
    console.log('Sliding complete!');
  }
});
```

  * As well as the specific convenience methods, jQuery also has a general-purpose `animate` method.
  * This method takes an object of the css properties to animate, and then either positional arguments fro duration and callback, or a second object with these arguments as options.

**Example**

```
$p.animate({
  left: 500,
  top: 250
}, 400, function() {
  $(this).text('All done!');
});

$p.animate({
  left: 500,
  top: 250
}, {
  duration: 1000,
  complete: function() {
    $(this).text('All done!');
  }
});
```

  * Most CSS properties can be animated using the `animate` method, though complex properties like colours cannot.
  * All properties that use a measurement unit use pixels by default unless a different units (e.g. `em`, `%`) is specified.

### Chaining and Delaying Animations

  * If you chain jQuery animation methods together, they will run sequentially. That is to say, the first animation in the chain will complete before the next one starts, and so on.

**Example**

```
$p.slideUp(250).fadeIn();
```

  * You can also add a delay before an animation or between chained animations using the `delay` method

**Example**

```
$p.slideUp(250).delay(500).slideDown(250);
```

### Stopping Animations

  * There's a couple of ways of stopping animations.
  * Sometimes you want to stop all current animations before running a new one. Here you can use the `stop` method before the animation method call. [jQuery API documentation](https://api.jquery.com/stop/)

**Example**

```
$p.stop().fadeToggle(200);
```

  * The above example will stop the *current* running animation on the `$p` element.
  * The `stop` method takes two optional boolean arguments, both of which default to `false`. The first, if set to `true` will remove all queued animations as well as stopping the current one. The second argument, if set to `true` will jump to the end of the frame of the current animation (rather than stoppig it where it currently is).

  * Another way of stopping animations is the `finish` method.
  * This method stops the currently currently-running animation, removes all queued animations, and jumps to the last frame for all queued animations. It's the equivalent of calling `stop(true, true)` on all the queued animations. [jQuery API documentation](https://api.jquery.com/finish/)

**Example**

```
// Element will immediately be visible and will be in position 50, 50

$p.fadeIn(200).animate({
  left: 50,
  top: 50
}).finish();
```

### Disabling Animations

  * There may be situations where you want to disable all animations completely. For example, you may be testing or debugging some other part of the application functionality and having the animations running is gettig in the way or making that more difficult.
  * You can turn off effects globally by setting `$.fx.off` to `true`. [jQuery API documentation](https://api.jquery.com/jquery.fx.off/]

**Example**

```
$.fx.off = true;
```

<a name="html-data-attributes"></a>
## HTML Data Attributes
