# New JavaScript Features

  * [JavaScript Versions](#js-versions)
  * [Build Tools](#build-tools)
  * [Babel Guide to ES2015](#guide-to-es2015)
  * [Let and Const](#let-const)
  * [Arrow Functions](#arrow-functions)
  * [Promises](#promises)

<a name="js-versions"></a>
## JavaScript Versions

  * JavaScript has been known by a number of different names since it was first created; it started life as LiveScript, before becoming JavaScript.
  * In 1996, Netscape submitted JavaScript to ECMA Internatinal for standardization, this eventuallky resulted in a version of the language known as ECMAScript. Al JavaScript implementations since then have been implementations of the ECMAScript standard, but the name JavaScript has stuck for historical/ marketing reasons. These days, ECMAScript is generally used to refer to the standard, whereas JavaScript is used when talking about the language in practice.
  * For many years the standard didn't really change, and real world implementations differed significantly from the standard. Around 2012 there was a push to stop supporting old versions of IE, and so writing code in the ECMAScript 5 (ES5) style become much more feasible.
  * ECMAScript 6th Edition (ES6) was then written and renamed ECMAScript 2015 (ES2015) as ECMA moved to a yerly model for standard names rather than using version numbers.
  * ECMA field proposals for uocoming versions (ES7/ ES2016, ES8/ ES2017, ES9/ ES2018, etc), before adding them to the standard

  * [Source](#https://benmccormick.org/2015/09/14/es5-es6-es2016-es-next-whats-going-on-with-javascript-versioning)

<a name="build-tools"></a>
## Version Support and Build Tools

  * Generally, ES5 code is supported natively in all browsers.
  * Some ES6 features are supported by newer browsers, but you cannot really on all browsers supporting these features.
  * If you want to use features from more recent JavaScript versions cross-browser, then you need to use a *build tool* to transfrom the ES6+ code into an older version of JavaScript. This process is know as *transpiling*, and the tools used to do it are *transpilers*.
  * A popular tool for transpiling ES6+ to ES5 is [Babel](https://babeljs.io/)
  * Babel is often used within a larger set of build tools, which often include a task-runner such as Gulp, Grunt, or Webpack. Most of these tools are written in JavaScript and part of the Node.js ecosystem
  * [BabelREPL](https://babeljs.io/repl) can be used as an online editor/ transpiler to become familiar with how Babel works

<a name="guide-to-es2015"></a>
## Babel Guide to ES2015

  * The [Babel Guide to ES6](https://babeljs.io/docs/en/learn/) covers most of the new features available in ECMA2015, including the following:

### Arrows and Lexical This

  * Arrows are a function shorthand using the `=>` syntax.
  * Unlike standard function syntax, arrows share the same lexical `this` as their surrounding scope, in other words arrow functions do not lose their execution context when nested in another function
  * [Source](https://babeljs.io/docs/en/learn/#arrows-and-lexical-this)

**Example**

```
// ES6
var evens = [1, 2, 3, 4, 5, 6].filter(v => v % 2 == 0); // [2, 4, 6]

// Transpiled to ES5
var evens = [1, 2, 3, 4, 5, 6].filter(function (v) {
  return v % 2 == 0;
});
```

### Classes

  * Classes are syntactical sugar over the prototype-based OO pattern
  * ES6 classes support protoype-based inheritance, super calls, instance and static methods and constructors.
  * [Source](https://babeljs.io/docs/en/learn/#classes)

### Enhanced Object Literals

  * Object literals are extended in ES6 to support setting the prototype at construction
  * [Source](https://babeljs.io/docs/en/learn/#enhanced-object-literals)

### Template Strings

  * Template strings provide syntactical sugar for constructing strings by supporting interpolation
  * Template strings are delimited by using back-ticks `` ` ``
  * [Source](https://babeljs.io/docs/en/learn/#template-strings)

**Example**

```
var name = "Bob";
var time = "today";
`Hello ${name}, how are you ${time}?`
```

### Destructuring

  * Destructuring allows binding using pattern matching, with support for matching arrays and objects.
  * Destructuring is fail-soft, producing `undefined` when values are not found.
  * [Source](https://babeljs.io/docs/en/learn/#destructuring)

**Example**

```
var [a, b, c] = [1, 2];
a; // 1
b; // 2
c; // undefined
```

### Default + Rest + Spread

  * Callee-evaluated default parameter values. Turn an array into consecutive arguments in a function call. Bind trailing parameters to an array. Rest replaces the need for `arguments` and addresses common cases more directly.
  * [Source](https://babeljs.io/docs/en/learn/#default-rest-spread)

**Example**

```
function f(x, y=12) {
  // y is 12 if not passed (or passed as undefined)
  return x + y;
}
f(3) == 15

function f(x, ...y) {
  // y is an Array
  return x * y.length;
}
f(3, "hello", true) == 6

function f(x, y, z) {
  return x + y + z;
}
// Pass each elem of array as argument
f(...[1,2,3]) == 6
```

### Let + Const

  * Block-scoped binding constructs. `let` is similar to `var` except that `var` is function-scoped and `let` is block-scoped. `const` is for single-assignment
  * [Source](https://babeljs.io/docs/en/learn/#let-const)

### Iterators + For..Of

  * Iterator objects enable custom iteration.
  * [Source](https://babeljs.io/docs/en/learn/#iterators-forof)

### Promises

  * Promises are a library for asynchronous programming. They are a first-class representation of a value that may be made available in the future.
  * [Source](https://babeljs.io/docs/en/learn/#promises)

<a name="let-const"></a>
## Let and Const

  * `let` and `const` are new ways in ES6 of declaring variables.
  * Both `let` and `const` define *block level* variables, which means the variable exists only within the block it is defined in and not the entire enclosing function
  * `let`, like `var`, allows variables to be reassigned to new values. Variables declared with `const` *cannot* be reassigned, though objects they reference can still be mutated.

### Further Reading

  * [ES6 let vs const variables](http://wesbos.com/let-vs-const/)
  * [MDN docs for let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
  * [MDN docs for const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)

<a name="promises"></a>
## Promises

  * Promises are a new feature of JavaScript adopted by many new browser APIs such as [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API), and nearly all frameworks and libraries
  * They are another solution to the problem solved by callbacks when working asynchronously in JavaScript; they are effectively a way to register behaviour that needs to run *after* an event.
  * Unlike callbacks, promises are objects that represent an *eventual value*; this allows for a variety of powerful abstractions to be built.
  * Promises have other benefits, such as providing behaviour that is cumbersome to create with event listeners

### Further Reading

  * [Promises MDN docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
  * [JavaScript Promises: an Introduction at Google Web Fundamentals](https://developers.google.com/web/fundamentals/primers/promises)
