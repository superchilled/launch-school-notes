# Event-Driven and Asynchronous Programming

  * [Asynchronous Execution with setTimeout](#async-execution)
  * [Repeating Execution with setInterval](#repeating-execution)
  * [User Interfaces and Events](#ui-events)
  * [Page Lifecycle Events](#page-lifecycle-events)
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

  * Running code after a timed delay can be useful, but most user interfaces need to respond to actions that a user performs rather than simply the passage of time; for example taking some action after a button is clicked.

<a name="page-lifecycle-events"></a>
## Page Lifecycle Events

<a name="user-events"></a>
## User Events

<a name="event-listeners"></a>
## Adding Event Listeners

<a name="event-object"></a>
## The Event Object

<a name="capturing-bubbling"></a>
## Capturing and Bubbling

<a name="prevent-propogation-and-default"></a>
## Preventing Propagation and Default Behaviors

<a name="event-delegation"></a>
## Event Delegation

<a name="event-loop"></a>
## The Event Loop
