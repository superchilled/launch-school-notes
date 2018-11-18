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

<a name="documentation"></a>
## Documentation

<a name="node-traversal"></a>
## Node Traversal

<a name="element-attributes"></a>
## Element Attributes

<a name="finding-nodes"></a>
## Finding DOM Nodes

<a name="traversing-elements]"></a>
## Traversing Elements

<a name="creating-moving-nodes"></a>
## Creating and Moving DOM Nodes

<a name="bom"></a>
## The Browser Object Model (BOM)
