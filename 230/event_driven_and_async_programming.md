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

### Source

  * https://developer.mozilla.org/en-US/docs/Web/Events

<a name="capturing-bubbling"></a>
## Capturing and Bubbling

<a name="prevent-propogation-and-default"></a>
## Preventing Propagation and Default Behaviors

<a name="event-delegation"></a>
## Event Delegation

<a name="event-loop"></a>
## The Event Loop
