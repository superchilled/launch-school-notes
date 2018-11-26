# Event-Driven and Asynchronous Programming

  * [Asynchronous Execution with setTimeout](#async-execution)
  * [Repeating Execution with setInterval](#repeating-execution)
  * [User Interfaces and Events](#ui-events)
  * [The Page Lifecycle](#page-lifecycle)
  * [User Events](#user-events)
  * [Adding Event Listeners](#event-listeners)
  * [The Event Object](#event-object)
  * [Capturing and Bubbling](#capturing-bubbling)
  * [Preventing Propagation and Default Behaviors](#prevent-propogation-and-default)
  * [Event Delegation](#event-delegation)
  * [The Event Loop](#event-loop)

<a name="async-execution"></a>
## Asynchronous Execution with setTimeout

  * We can think of some code we encounter as running **sequentially**; that is each line executes in turn, the next line must wait until the current line completes.
  * Even if code doesn't run sequentially (for example a function is called multiple times and so the body of that function is executed each time it is called), it still runs **synchronously**; that is it may not strictly in the order of the lines as they appear in the code file, but each line still has to wait for another line to complete execution before it can run.
  * It is possible to run code that partly runs now, and then pauses for a certain period of time, before completing execution at a later point. This is called **asynchronous** code. This code can run, then pause, and while it is paused other code can run.
  * One example of asynchronous code execution in JavaScript is using the `setTimeout()` function.
    * This function takes two arguments: a callback function and a specified wait time (in milliseconds).
    * It sets a timer and waits until the given time delay elapses before invoking the callback function.

**Example**

```
setTimeout(function() {
  console.log('Hello');
}, 3000);

console.log('World');
```

This code would log the following to the console:

```
World
Hello
```

  * Working with asynchronous code measns you must reason with btoh *what* the code does and *when* it does it.
  * This is something to be aware of when workig with the DOM, since some DOM APIs are asynchronous.

<a name="repeating-execution"></a>
## Repeating Execution with setInterval

  * Whereas `setTimeout()` invokes a callback function once, after a specified delay, `setInterval()` invokes a callback function over and over at specific intervals until told to stop.
  * We can halt `setInterval()` using `clearInterval()` as long as we assign the return of `setInterval()` to a variable (that can then be passed as an argument to `clearInterval()`)

**Example**

```
var myInterval = setInterval(function() {
  console.log('hey!');
}, 1000);

// 'hey!' is logged every second

// later on in the program (perhaps in response to a condition being met or a user event)
clearInterval(myInterval);
```

<a name="ui-events"></a>
## User Interfaces and Events

  * Running code after a timed delay can be useful, but most user interfaces need to respond to actions that a user performs rather than simply the passage of time; e.g. taking some action after a button is clicked.
  * An **DOM event** is an *object* of the DOM API that represents some sort of occurence within the DOM.
    * It contains information about what happened and where it happened (e.g. 'this link was clicked' or 'the mouse entered this div').
    * The browser can trigger events:
      * As the page loads
      * When a user interacts with the page
      * When the browser performs some action required by the program

### Event-Driven User Interfaces

  * User interfaces are inherently *event-driven*. An interface draws itself on screen and then does nothing until a user interacts with it. Such interfaces require the program to register some behaviour that the event will trigger when it occurs. In this context we can think of the code behind a user interface having two main tasks:
    1. Set up the user interface and display it
    2. Handle events resulting from user or browser actions
 * In the browser, step 1 is typically achieved with HTML (and CSS). Step 2 is carried out through the use of *event listeners*.
  * The *event loop* runs continually in the background, waiting for an event to occur. When an event occurs, the event loop processes it then goes back to waiting.
  * We can get our program to listen for events using the `EventTarget.addEventListener()` method, which is part of the DOM Event Model.

**Example**

```
document.addEventListener('click', function(event) {
  // Do something with the DOM
});
```

  * In the above example we are calling `addEventListener()` on the `document` DOM object. The first argument to the method `click` is the event type we want to listen for (there are many different types of event), and the second argument to the method is a callback function that gets executed when the event occurs (the callback function can be passed the `event` object as an argument).

### Sources

  * https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Events
  * https://developer.mozilla.org/en-US/docs/Web/API/EventTarget
  * https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener

<a name="page-lifecycle"></a>
## The Page Lifecycle

  * When a web page loads, a lot of stuff needs to happen before the page is 'ready' to interact with
  * A simplified breakdown of what happens might look something like this:
    1. HTML Code is received from the server
    2. HTML parsed and JavaScript is evaluated
    3. DOM constructed from parsed HTML
    4. Page displayed on screen
    5. Embeded assets (e.g. images) are loaded
  * It's important that we understand this process, because if we want to, say, add an event listener to a DOM element on the page, that DOM element needs to exist in the DOM before we can add the event listener.
  * There's a couple of different events that we can use to help us. These events fire in response to what is happening as part of this page lifecycle rather than as a result of user interaction:
    *
    * `load` is fired when a resource (i.e. a webpage) and all its dependent resources (e.g. embeddedimages) have finished loading. It is fired on the `window` object.
    * `DOMContentLoaded` is fired when the initial HTML document has been completely loaded and parsed, without waiting for stylesheets, images, subframes, etc to finish loading. It is fired on the `document` object.
    * If we add these two events to our Page Lifecycle breakdown, it might look something like this:
      1. HTML Code is received from the server
      2. HTML parsed and JavaScript is evaluated
      3. DOM constructed from parsed HTML
        * `DOMContentLoaded` event fires on `document`
      4. Page displayed on screen
      5. Embeded assets (e.g. images) are loaded
        * `load` event fires on `window`

  * If we want to work with the DOM in some way, such as adding an event listener to an element which is part of the DOM, we don't need to wait for the `load` event, we can listen instead for the `DOMContentLoaded` event to fire and then we can start to work with the DOM.

**Example**

```
document.addEventListener('DOMContentLoaded', function(event) {
  document.getElementsByTagName('A').addEventListener('click', function(event) {
    // do something when links are clicked
  });
});
```

  * In the above example, we wait until the DOM is loaded, then get all of the `<a>` elements and add a `'click'` event listener to those elements.

### Sources

  * https://developer.mozilla.org/en-US/docs/Web/Events/load
  * https://developer.mozilla.org/en-US/docs/Web/Events/DOMContentLoaded

<a name="user-events"></a>
## User Events

  * The majority of the time an interactive web application will respond specifically to user actions.
  * There are many different types of events that can be triggered by user actions, depending on the inpout device, the application, and the user.

**Some Common User Event Types and Example Events**

| Event Type | Example Events |
|---|---|
| Keyboard | `keydown`, `keyup`, `keypress` |
| Mouse | `mouseenter`, `mouseleave`, `mousedown`, `mouseup`, `click` |
| Touch | `touchdown`, `touchup`, `touchmove` |
| Window | `scroll`, `resize` |
| Form | `submit` |

  * There are many are events types and events listed in the [Event Reference](https://developer.mozilla.org/en-US/docs/Web/Events) section of the MDN documentation

<a name="event-listeners"></a>
## Adding Event Listeners

  * **Event Listeners** are can be used by JavaScript code in order to call functions in response to an event firing.
  * This JavaScript code is generally referred to as an **Event Handler**, since it 'handles' or deals with the event)
  * There are four steps needed to set up an Event Handler:
    1. **Identify the event you need to handle**. This will be a user action, a lifecycle event, or some other event that is part of the DOM Event Model
    2. **Identify the element that will receive the event**. Depending on the event, this could be a button, an input field, some other element on the page, the entire `document`, or the `window` itself.
    3. **Define a function to call when this event occurs**. The function will a single argument, the `Event` object.
    4. **Register the function as an Event Listener**. This is the implementation of the first three steps into a working system.

  * The `GlobalEventHandlers` mixin provides an alternative way to register a function as an event listener for an element.

**Example**

```
function myFunc() {
  // do something
};

var button = document.getElementById('alert');

// Using addEventListener()
button.addEventListener('click', myFunc);

// Using GlobalEventHandlers
button.onclick = myFunc;
```

### Sources

  * https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener
  * https://developer.mozilla.org/en-US/docs/Web/API/GlobalEventHandlers

<a name="event-object"></a>
## The Event Object

  * The callback function passed to `addEventListener` can take the `Event` object as an argument.

**Example**

```
document.addEventListener('click', function(event){
  // do some stuff
});
```

  * This `Event` object can provide us with more information about the event that occurred.
  * Some useful properties that appear in `Event` objects include:
    * `type`: the name of the event type, e.g. `'click'`
    * `currentTarget`: The object to which the event listener is attached, e.g. `document`
    * `target`: The object that fired the event, e.g. in the case of a click event it would be the element that was clicked by the user (this might not be the same as the element to which the listener is attached, but is should be *nested* within that element)
    * In addition to these standard properties, the `Event` object can have additional properties depending on the type of event.

### Mouse Events

  * `button`: a read-only property that indicates which mouse button was pressed
  * `clientX`: the horizontal position of the mouse when the event occurred
  * `clientY`: the vertical position of the mouse when the event occurred
    * Both `clientX` and `clientY` return values relative to the visible area of the page, i.e the viewport.The values are given in pixels from the upper-left corner of the browser's viewport

### Keyboard Events

  * `which`: The ASCII key code (a number) that identifies the key pressed. (This property is deprecated but may still be seen in existing code)
  * `key`: The string value of the pressed key. (older browsers do not support this property)
  * `shiftKey`: A Boolean value indicating whether the user pressed the shift key during the event
  * `altKey`: A Boolean value indicating whether the user pressed the alt (or option) key during the event
  * `ctrlKey`: A Boolean value indicating whether the user pressed the control key during the event
  * `metaKey`: A Boolean value indicating whether the user pressed the meta (or command) key during the event

  * Some Keyboard events do not fire on all keys. For example, `keypress` events arent# generated by `Shift`, `Control`, etc. (for these keys you need to listen for `keyup` or `keydown`, not `keypress`)

### Sources

  * https://developer.mozilla.org/en-US/docs/Web/Events
  * https://medium.com/launch-school/javascript-lets-talk-about-events-572ecce968d0

<a name="capturing-bubbling"></a>
## Capturing and Bubbling

  * Adding event handlers to individual elements that may be the source of events (e.g. a specific `<a>` element) may work ok for a small application with only a few elements to interact with. This approach does have downsides, however:
    * You can't add an event listener to an element until it exists in the DOM, so you must wait until the `DOMContentLoaded` event fires
    * If a new element is added to the page programatically *after* `DOMContentLoaded` fires, you need to manualy add an event handler to that element
    * If your application has lots of elements that can be interacted with (like hundreds of links, for example) then adding individual evant listeners for each one can slow the application down, and lead to complicated, difficult to maintain code.
  * A technique called **event delegation** provides a solution for these problems.
  * In order to use event delegation, it's important to understand how capturing and bubbling work.

### Nested Elements and Handling Events

  * The DOM can contain elements nested inside each other
  * If an event listener is added to an 'outer' element, events created by interacting with elements nested inside that element can be handled by that 'outer' event listener.

**Example**

```
<div id="elem1">
  <div id="elem2">
    <div id="elem3">
      <div id="elem4">
      </div>
    </div>
  </div>
</div>
<script>
  var elem1 = document.querySelector('#elem1');

  elem1.addEventListener('click', function(event) {
      alert('hello!');
  });
</script>
```

  * In the above example, if we click anywhere on the page, a `'click'` event will be created. If that event is created by clicking inside any of the four `<div>` elements, the event listener added to the outermost `<div>` will handle the event.
  * This technique is useful for when you have a number of elements that may potentially be interacted with nested inside another element. It is common to add an event listener to the `document` DOM element to handle events fired by any elements inside the `document`.

### `target`, `currentTarget` and `this`

  * `target` and `currentTarget` are properties of the `Event` object
  * `target` refers to the element on which the event occurred (e.g. the element that was clicked in the case of a `click` event)
  * `currentTarget` refers to the element on which the event listener was added
  * `this` in within an event handler refers to `currentTarget`, i.e. the element to which the event listener was added

### The Capturing and Bubbling phases

  * **Capturing** and **Bubbling** are phases that an event goes through after it initially fires.
  * Capturing phase: When an event fires, it first gets dispatched to the global `window` object, then the `document` object, then each nested DOM element until it reaches the element on which the event was orginally fired.
  * Bubbling Phase: from the element on which the event was originally fired, the event follows the same route as in the capturing phase but in reverse, until it reaches `window`
  * As the event moves through the capturing and bubbling phases it checks of there are any listeners for it on each DOM element it passes.
  * Since both phases run, the event gets dispatched to each DOM element twice: once during the capturing phase, and once during the bubbling phase.
    * The actual event listener only gets called/ fired in one of these phases; by default the listener is set to fire during the bubbling phase.
    * A third (optional) argument can be passed to the `addEventListener()` method to set the value of the `useCapture` parameter; if set to `true` then the listener fires on the capturing phase instead of the bubbling phase.

<a name="prevent-propogation-and-default"></a>
## Preventing Propagation and Default Behaviors

### Stopping Propogation

  * During the capturing and bubbling phases, the event gets dispatched from `window` down to the `target` DOM element and back up to `window`. If we have multiple event listeners for the same event type on different DOM elements along this path, then all of those event listeners will fire.

**Example**

```
<div id="elem1">
  <div id="elem2">
    <div id="elem3">
      <div id="elem4">
      </div>
    </div>
  </div>
</div>
<script>
  var elem1 = document.querySelector('#elem1');
  var elem4 = document.querySelector('#elem4');

  elem1.addEventListener('click', function(event) {
      alert('hello!');
  });

  elem4.addEventListener('click', function(event) {
      alert('goodbye!');
  });
</script>
```

  * Given the above code example, if we click on the innermost `<div>` (`elem4`), the event will be dipatched along the following path:
    * Capturing: `window` > `document` > `elem1` > `elem2` > `elem3` > `elem4`
    * Bubbling: `elem4` > `elem3` > `elem2` > `elem1` > `document` > `window`
    * The same event will cause two event listeners to fire during the bubbling phase: first the one attached to `elem4`, then the one attached to `elem1`; this will result in two alerts: the first one saying `'goodbye!'`, and the scond `'hello!'`
  * Much of the time you will only want a single event listener to fire as the result of an event. In order to achieve this, we can call the `stopPropogation` method of `Event` on the event object within the event handler in order to stop the event object from continuing on its path along the capturing and bubbling phases.

**Example**

```
<div id="elem1">
  <div id="elem2">
    <div id="elem3">
      <div id="elem4">
      </div>
    </div>
  </div>
</div>
<script>
  var elem1 = document.querySelector('#elem1');
  var elem4 = document.querySelector('#elem4');

  elem1.addEventListener('click', function(event) {
      alert('hello!');
  });

  elem4.addEventListener('click', function(event) {
      event.stopPropogation();
      alert('goodbye!');
  });
</script>
```

  * Given the above code example, if we click on the innermost `<div>` (`elem4`), the event will be dipatched along the following path:
    * Capturing: `window` > `document` > `elem1` > `elem2` > `elem3` > `elem4`
    * Bubbling: `elem4`
    * The same event will cause the event listener attached to `elem4` to fire. The callback in the event handler calls `event.stopPropogation()` which stops the event from continuing up through the bubling phase.

### Preventing Default Behaviours

  * Another thing we often want to do when handling events is to prevent the default behaviour for the interaction which caused the event.
  * A common example is when an `<a>` element is clicked, rather than follow the default behaviour for a hyperlink (which is to send a `GET` request to the path defined by the `href` attribute), we might want to do something else.
  * To achieve this we can use the `preventDefault()` method of `Event`, which tells the browser not to execute the default behaviour for that event.

**Example**

```
<a href='http://google.com'></a>
<script>
var link = document.querySelector('#elem1');

link.addEventListener('click', function(event) {
  event.preventDefault();
  alert('hello!');
});
</script>
```

  * In the code example above, when the link is clicked, rather than going to `google.com` the event handler will tell the browser not to follow this default behaviour, so the browser stays on the same page and then executes the rest of the callback function (in this case alerting `'hello!'`).

  * It is good practice to call `preventDefault` and/ or `stopPropogation` as soon as possible in an event handler.
    * This emphasises the presence of those methods to anyone reading the code
    * It ensures those methods will execute before any errors occur (not running these methods before an error occurs can lead to unexpected behaviour which is hard to debug).

<a name="event-delegation"></a>
## Event Delegation

  * Event delegation takes advantage of event bubbling to deal with the problems which can occur if we add individual event handlers to mutliple DOM elements
  * One way of doing this is add an event handler to a common parent of the elements you want to target, for example the `document` object

**Example**

```
document.addEventListener('click', function(event) {
  var tag = event.target.tagName;

  if (tag === 'A') {
    event.preventDefault();
    alert('hi!');
  }  
});
```

  * In the above example, when any element in `document` is clicked, the click event fires the event listener. If the element that fired the event is an `<a>` element (i.e. the `tagName` is `'A'`) then the alert is created.

  * The trade-off to event delegation is that the listener may becaom complicated if it must handle multiple situations. Looking at the example, imagine of the listener had to perform a number of different actions, each depending on what type of element was clicked (a link, a button, an image, a div, etc)
  * For small projects it is often best to bing event handlers directly to the element which will be interacted with.
  * For larger projects with lots of elements and a larger number of possible interactions, it perhaps makes sense to delegate the event to a common parent element, but care must be taken when selecting which element to use for this delegation to prevent handlers from become too complex
  * Some JavaScript libraries, such as `jQuery`, provide functionality that makes delegation comparatively easy while avoiding the complexity trade-off.

<a name="event-loop"></a>
## The Event Loop

  * The Event Loop is one part of JavaScript's concurrency model
  * The JavaScript run-time is *single-threaded*, which basically means it can do one thing at a time.
    * When a function is invoked, it gets added to the call-stack, and when it returns is removed (function calls are added and removed in a LIFO order)

### Sources

  * https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop
  * https://blog.carbonfive.com/2013/10/27/the-javascript-event-loop-explained/
  * https://www.youtube.com/watch?v=8aGhZQkoFbQ
