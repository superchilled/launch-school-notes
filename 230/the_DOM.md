# The DOM

  * [The Document Object Model (DOM)](#dom)
  * [A Hierarchy of Nodes](#node-hierarchy)
  * [Node Properties](#node_properties)
  * [Determining Node Type](#node-type)
  * [Documentation](#documentation)
  * [Node Traversal](#node-traversal)
  * [Element Attributes](#element-attributes)
  * [Finding DOM Nodes](#finding-nodes)
  * [Traversing Elements](#traversing-elements])
  * [Creating and Moving DOM Nodes](#creating-moving-nodes)
  * [The Browser Object Model (BOM)](#bom)

<a name="dom"></a>
## The Document Object Model (DOM)

  * The Document Object Model (DOM) is an in-memory representation of an HTML document.
  * It provides a way of interacting with a web-page using JavaScript. This fact is leveraged to build modern, interactive user experiences on the web.
  * What the DOM isn't:
    * The HTML code you write. Although this is parsed by the browser and *turned into* the DOM
    * The code you can see in 'View Source' in a browser. This just shows the HTML that makes up the page.
  * What the DOM is:
    * The code you see in a browser's Dev Tools. This is pretty much a visual representation of the DOM.
  * The representation of the DOM shown in Dev Tools may look very similar to HTML code that was written, but there are key differences:
    * The browser can add missing elements/ fix mistakes in the written HTML. These exist in the DOM but not in the source.
    * JavaScript can manipulate the DOM. If you, for example, add some content to an element using JavaScript, this shows in the DOM (visually represented in Dev Tools) but not in the source HTML.
  * AJAX and Templating: this ability of the DOM to be manipulated means we can use AJAX to get data/ content from a source and add it to a webpage, or update part of a page. This can be combined with templating to create interactive web experiences or single page applications
  * JavaScript and the DOM. The DOM isn't part of JavaScript (the language), it is part of the browser; essentially an API for the browser. JavaScript is used to interact with it, but they are separate entities.
  * There are specific [DOM Levels](https://developer.mozilla.org/fr/docs/DOM_Levels) (e.g. 'DOM Level 1') which provide different features. The DOM Level which a particular browser supports indicates which DOM features can be used.

### Sources

  * https://css-tricks.com/dom/
  * http://www.w3.org/TR/DOM-Level-2-Core/introduction.html
  * https://developer.mozilla.org/en-US/docs/DOM/DOM_Reference/Introduction
  * http://en.wikipedia.org/wiki/Document_Object_Model

<a name="node-hierarchy"></a>
## A Hierarchy of Nodes

  * The DOM is a hierarchical tree structure of 'nodes' (imagine each 'branch' of the tree ending in a node):
    * Element nodes represent HTML tags, e.g. `HTML`, `HEAD`, `DIV`, `H1`, `P`, etc.
    * Text nodes represent text that appears in the document
    * Comment nodes represent HTML comments
  * Text nodes can either contain actual text, or they can contain whitespace (such as tabs, newlines, etc.). Text nodes containing whitespace are sometimes referred to as 'empty nodes', but they are still essentially text nodes.
  * Forgetting to count 'empty' text nodes as nodes can lead to bugs. For example, if your code expects to find en element node next to another element node and there is some whitespace between them, the code may end up targetting the wrong node. Although these 'empty' text nodes are not reflected visually in the browser, they **do** exist in the DOM.

<a name="node_properties"></a>
## Node Properties

  * Nodes are DOM API objects. As such they can have properties and methods.
  * All nodes ultimately inherit from `node`
  * `document` is a particular type of node which represents the HTML document. Other nodes are 'child nodes' of `document`
  * `document` has a method called `querySelector()`, which returns the first *Element* node that matches the specified selector (passed in as an argument) and is a child node of the node that the method is called on.
    * Since all nodes in an HTML document are child-nodes of the `document` node, we can use `document.querySelector()` to target any node in the document.

**Example**

```
document.querySelector('p'); // this would return the first paragraph element in the document
```

  * All DOM nodes have certain properties in common, including:

    * `nodeName`: this is a read-only property which holds a string value which corresponds to the 'name' of node.
      * For text nodes (including empty nodes) this value is `'#text'`
      * For comment nodes, this value is `'#comment'`
      * For element nodes, the value is the corresponding tag in *uppercase*, e.g. `'P'`, `'LI'`, `'DIV'`, etc.

    * `nodeType`: this is a read-only property which holds a numerical value corresponding to the type of node, e.g. `1` for element nodes, `3` for text nodes, `8` for comment nodes, `9` for document nodes, etc.
      * The same values can also be obtained by referencing the node constant names, e.g. `Node.ELEMENT_NODE` returns `1`; this can be used for comparison in the form `document.nodeType === Node.DOCUMENT_NODE` (which will return true)

    * `nodeValue`: this property is read/ write, and returns or sets the 'value' of a node. Element nodes only hold `null` values, but text and comment nodes hold string values, which can be retrieved or set.

    * `textContent`: this property is read/ write. When used for retrieval, it returns the concatenatio of the `textContent` of every child node (excluding comments and processing instructions, but ioncluding empty text nodes) of the node it is called on. Setting this property on a node, removes all of its chilren and relaces them with a single text node of the given value.

### Sources

  * https://developer.mozilla.org/en-US/docs/Web/API/Node

<a name="node-type"></a>
## Node Types

  * All DOM objects are Nodes
  * All DOM objects have a type that inherits from `Node`, which means they all have properties and methods they inherit from `Node`.
  * The most common DOM object types you will use are `Element` and `Text`.
    * `Text` nodes hold text
    * `Element` nodes represent HTML elements
  * There are more specific `Element` node types which inherit from `Element`, for example: `HTMLAnchorElement`, `HTMLDivElement`, `HTMLInputElement`.
    * These more specific types have properties and methods specific to them.
    * Most HTML tags have a related Node Element type (though not all, and a few are only supported in certain browsers such as `HTMLTableDataCellElement`). These types are subtypes that inherit from `HTMLElement` (there are other `Element` types such as `SVGElement`)
  * The `EventTarget` DOM object provides the event-handling behaviour that supports interactive web applications. `Node` inherits from `EventTarget`.

### Determining Node Type

There are two commonly used ways of determining a node type:

  1. Using the `toString()` method or the `String()` constructor.
      * This approach works best in interactive console sessions or Dev Tools.
      * Most nodes return a value of the node type's name, but some nodes respond differently (e.g. the `HTMLAnchorElement` node implements `toString()` in a way that returns the value of the `href` attribute of that element). A work-around is to reference the nodes's constructor property to return the constructor object, and then reference that object's name

**Example**

```
var p = document.querySelector('p');
p.toString(); // "[object HTMLParagraphElement]"

var a = document.querySelector('a');
a // <a href="http://domain.com/page">Page</a>
a.toString(); // "http://domain.com/page"

a.constructor.name; // "HTMLAnchorElement"
```

  2. Using the `instanceof` function or `tagName` property.
      * This approach works best within actual program code.
      * `instanceof` checks whether an object has a type that matches or inherits from a named type
        * The downside is that you have to test against a particular element type
      * The `tagName` property returns an element nodes's HTML tag name (in upper case)

**Example**

```
var p = document.querySelector('p');
p instanceof HTMLParagraphElement; // true
p instanceof HTMLAnchorElement; // true
p instanceof Element; // true

p.tagName; // 'P'
```

<a name="node-traversal"></a>
## Node Traversal

  * DOM nodes connect with other DOM nodes via a set of properties that point from one node to another with defined relationships.
  * All element nodes have parent properties, e.g:
    * `childNodes`: returns a *live collection* of all child nodes (a live collection automatically updates to reflect changes in the DOM)
    * `firstChild`: returns `childNodes[0]` or `null`
    * `lastChild`: returns `childNodes[childNodes.length - 1]` or `null`
  * All element nodes also have child properties:
    * `parentNode`: Immediate parent of the node
    * `nextSibling`: `parentNode.childNodes[n + 1]` or `null`
    * `previousSibling`: `parentNode.childNodes[n - 1]` or `null`
  * Whether these properties have an actual value or are `null` depends on the structure of the DOM tree; (e.g. a child node that is the last sibling in a collection of nodes will have `null` for `nextSibling`)

### Walking the Tree

  * 'Walking the Tree' is a term that describes the process of visiting every node that has a child, grandchild, etc relationship with a given node, and doing something with each of them.
  * Walking the tree is carried out using a **recursive** function.

**Example**

```
// walk() calls the function "callback" once for each node
function walk(node, callback) {
  callback(node);                                // do something with node
  var i;                                   
  for (i = 0; i < node.childNodes.length; i++) { // for each child node
    walk(node.childNodes[i], callback);          // recursively call walk()
  }
}

walk(document.body, function(node) {             // log nodeName of every node
  console.log(node.nodeName);
});
```

<a name="element-attributes"></a>
## Element Attributes

  * HTML elements have *attributes*, such as `class`, `id`, `href`, etc..
  * When interacting with DOM element nodes, we can *get* or *set* these attributes on the element in the DOM.
  * All DOM element nodes can use the following methods to work with attributes:
    * `hasAttribute('name')`: checks to see if an element has an attribute (e.g. 'name'); returns `true` or `false`
    * `getAttribute('name')`: retrieves the value of an attribute (e.g. 'name'); returns the value of the attribute as a string
    * `setAttribute('name', 'new-value')`: sets the value of an attribute (e.g. 'name') to the value provided byt he second argument; returns `undefined`

**Example**

```
p.hasAttribute('class'); // true
p.getAttribute('class'); // 'intro'
p.setAttribute('class', 'primary'); // undefined
p; // <p class="primary">...</p>
```

### Attribute Properties

  * Certain attributes on certain types of DOM element node can be interacted with via a property name such as `id`, `name`, `title`, `value`, e.g. using dot notation
  * Not every Element type has these properties, but most have at least the `id` and `className` properties
    * `className` is used as the property name rather than `class` because `class` is a reserved word in JavaScript

**Example**

```
p.className; // 'primary'
p.className = 'secondary'; // undefined
p; // <p class="secondary">...</p>
```

### `classList`

  * Working with the `class` attribute via `getAttribute`, `setAttribute`, or `className` can be inconvenient because elements can quite often have more than one class.
  * Since classes are space-separated within the attribute value, checking for the existence of a particular class, removing or changing a class can be tricky.
  * Most modern browsers provide a better way to work with the `class` arttribute, via the `classList` property
    * `classList` references a special array-like `DOMTokenList` object, with some useful properties and methods:
      * `add('name')`: adds class `'name'` to the class attribute
      * `remove('name')`: removes class `'name'` from the class attribute
      * `toggle('name')`: Adds class `'name'` if it doesn't exist, or removes it if it does
      * `contains('name')`: returns `true` or `false` depending on whether the class attribute contains the class `'name'`
      * `length`: returns the number of classes within the `class` attribute

**Example**

```
p.classList.add('visible');
p; // <p class="secondary visible">...</p>
p.classList.contains('visible'); // true
p.classList.remove('secondary');
p.classList.contains('secondary'); // false
p; // <p class="visible">...</p>
```

### `style`

  * DOM Element nodes also have a `style` property which references as `CSSStyleDeclaration` object.
  * You can use the style property to alter any CSS property for the DOM Element
  * CSS properties are manipulated using the CSS property name. Hyphenated names are camel-cased
  * To remove a CSS property it can be set to `null`

**Example**

```
var h1 = document.querySelector('h1');
h1.style.color = 'red';
h1.style.lineHeight = '3em';
h1.style.color = null;
```

  * It's generally considered better practice to define styles in a stylesheet and apply them to an element by adding or removing classes

<a name="finding-nodes"></a>
## Finding DOM Nodes

  * In HTML/JavaScript applications we often need to find a DOM element based on the value of its `id` attribute
    * We can use the `getElementById` method of `document` to do this. Since `id` values in a document should be unique, we can target specific elements by their `id`
  * If we want to find multiple elements that meet some criteria, we can use a couple of other methods:
    * `document.getElementsByTagName(tagName)`: returns an `HTMLCollection` or `NodeList` of matching elements; e.g. a `tagName` of `'P'` would return all paragraph elements
    * `document.getElementsByClassName(className)`: returns an `HTMLCollection` or `NodeList` of matching elements; e.g. a `className` of `'orange'` would return all elements that had a `class` or `'orange'`

### `HTMLCollection` and `NodeList`

  * `HTMLCollection` and `NodeList` are both index-based array-like objects containing DOM nodes
  * Since they are not JavaScript arrays, they don't supprt `Array` methods such as `forEach()` (though on some browsers, `NodeList` does support `forEach()`)
  * When using `document.getElementsByTagName()`, some browsers will return a `HTMLCollection` object, and some (WebKit) will return a `NodeList`
    * There are some differences between the two object types (e.g. `HTMLCollection` is supposed to only contain Element nodes, whereas `NodeList` can contain any type of node), but you can use them in broadly the same way
    * Other methods, such as `document.getElementsByClassName()` always return an `HTMLCollection` , regardless of browser
    * `HTMLCollection` is a *live collections*, meaning that it automatically reflects any changes in the DOM.
      * For example, if you call `document.getElementsByTagName('P')` to return all paragraph elements, then add a new paragraph element to the DOM, the object returned by `getElementsByTagName()` will now contain the new paragraph
    * `NodeList` is live in some cases, but static others; e.g. `Node.childNodes` and `getElementsByTagName()` return live collections, `document.querySelectorAll()` returns a static `NodeList`

### Using CSS Selectors

  * Modern browsers support an easier way of finding DOM Nodes, using methods which identify nodes via CSS selectors:
    * `document.querySelector(selector)`: returns the first matching element, or `null` if no match
    * `document.querySelectorAll(selector)`: returns a *static* `NodeList` of matching elements
  * Some older browsers, e.g. versions of IE older than IE9, don't support these methods
  * Since specific selector combinations can be used, these methods are very flexible

**Example**

```
document.querySelectorAll('p'); // gets all paragraph elements
document.querySelectorAll('.intro'); // gets all elements with a class of intro
document.querySelectorAll('.intro p'); // gets all paragraph elements nested inside an
// element with a class of 'intro'
```

### Sources

  * https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementById
  * https://developer.mozilla.org/en-US/docs/Web/API/HTMLCollection
  * https://developer.mozilla.org/en-US/docs/Web/API/NodeList
  * https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementsByTagName
  * https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementsByClassName

<a name="traversing-elements]"></a>
## Traversing Elements

  * When traversing elements, using properties such as `nextSibling` and `previousSibling` may not necessarily be the most appropriate, since the sibling of an element could be a text node or a comment node.
  * If you want to work with *just* element nodes, there is another set of DOM node properties that can be used for traversal:
    * Parent Element properties:
      * `children`: returns a *live collection* of all child element nodes
      * `firstElementChild`: returns `children[0]` or `null`
      * `lastElementChild`: returns `children[children.length - 1]` or `null`
      * `childElementCount`: returns `children.length`
    * Child Element properties:
      * `nextElementSibling`: returns `parentNode.children[n + 1]` or `null`
      * `prevElementSibling`: returns `parentNode.children[n - 1]` or `null`

### Text Content

  * When working with properties that target Element nodes, you can still interact with the text content of those nodes via the `textContent` property

**Example**

```
<a href='subscribe.html'>Subscribe</a>
```

```
document.querySelector('a').textContent; // 'Subscribe'
document.querySelector('a').textContent = 'Subscribe now!!!';
```

```
<a href='subscribe.html'>Subscribe now!!!</a>
```

  * Something to be aware of when setting `textContent` is that is removes all child nodes from the element and replaces them with the text that contains the value.

**Example**

```
<a href='subscribe.html'><span class="cta-text">Subscribe</span></a>
```

```
document.querySelector('a').textContent = 'Subscribe now!!!';
```

```
<a href='subscribe.html'>Subscribe now!!!</a>
```

  * We could have instead targetted the span element:

**Example**

```
document.querySelector('a span').textContent = 'Subscribe now!!!';
```

<a name="creating-moving-nodes"></a>
## Creating and Moving DOM Nodes

  * As well as manipulating the DOM to modify existing nodes, it is also possible to create, add, and remove nodes.

### Creating New Nodes

  * This can be done in two ways:
    * Creating a new empty node with the `document.create*` methods:
      * `document.createElement(tagName)`: this creates a new element node; e.g. if `tagName` is `'P'`, a new paragraph element is created
      * `document.createTextNode(text)`: this creates a new text node containing whatever text is supplied as a string argument to the `text` paraemter
    * Cloning an existing node using  `node.cloneNode(deepClone)`. The `cloneNode` method is called on an existing node and returns a cloned version of the node. `deepCLone` should be a boolean, if `true` then `cloneNode()` copies `node` and all of its children, if `false` it merely copies `node`.
      * When cloning a node, the resulting DOM objects that represent them are independent of each other -- changing one does not change the other.

### Adding Nodes to the DOM

  * You can append, insert, or replace nodes by calling a method on a parent node.
    * `parent.appendChild(node)`: appends `node` aat the *end* of `parent.childNodes`
    * `parent.insertBefore(node, targetNode)`: inserts `node` into `parent.childNodes` *before* `targetNode` (i.e. `node` becomes the `previousSibling` of `targetNode`).
    * `parent.replaceChild(node, targetNode)`: removes `targetNode` from `parent.childNodes` amd inserts `node` in its place.
  * Note 1: `docuemnt.appendChild` causes an error; use `document.body.appendChild` instead.
  * Note 2: No node may appear twice in the DOM. If you try to add a node that already exists, it gets removed from the original location. This fact can be leveraged as a useful method of *moving* existing nodes by inserting it where you want.

  * There are some additional methods that are useful for DOM node insertion, where you want to insert a node in a position adjacent to an existing node rather than as a child of a parent node:
    * `element.insertAdjacentElement(position, newElement)`: inserts `newElement` at `position` relative to `element`
    * `element.insertAdjacentText(position, text)`: inserts a Text node containing `text` at `position` relative to `element`
    * For both of these methods, `position` must be one of the following String values:
      * `'beforebegin'`: inserts node *before* `element`
      * `'afterbegin'`: inserts node before the *first child* of `element`
      * `'beforeend'`: inserts node after the *last child* of `element`
      * `'afterend'`: inserts node *after* the element

### Removing Nodes

  * When you remove a node from the DOM it becomes elegible for garbage collection unless you keep a reference to it in a variable.
  * There are a couple of methods which are commonly used to remove nodes from the DOM:
    * `node.remove()`: removes `node` from the DOM
    * `parent.removeChild(node)`: removes `node` from `parent.childNodes`

<a name="bom"></a>
## The Browser Object Model (BOM)

  * The DOM (Document Object Model) reperesents the *document* that is displayed in the browser.
  * It is possible to access other components of the browser environment (beyond the displayed document) using JavaScript.
  * These components are not part of the DOM, but are instead part of the BOM (Browser Object Model).
  * The BOM includes the DOM, but also includes things like:
    * The browser window
    * The browser's history
    * Sensors, such as location sensors
