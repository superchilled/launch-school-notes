# HTML Templating with JavaScript

  * A basic way of manipulating HTML content on a web-page using JavaScript, would be to create a string and the  add it to the DOM.
  * If we wanted to include data in the HTML content, we could use string concatenation.

**Example**

```
var fruit = 'apple';

var para = '<div><h1>Favourite Fruit</h1><p>My favourite fruit is ' + fruit + '!!!</p></div>';

var mainSection = document.getElementById('main');

mainSection.appendChild(para);
```

  * There are a number of reasons why you might not want to use this approach:
    * For complex elements you'd need a lot of messy concatenation or nested loops
    * Introducing conditional output complicates matters even further
    * There's no separation of concerns (your HTML and JavaScript are mixed togther in a way that makes either difficult to edit)

## Client-side Templating

  * Client-side templating provides a cleaner and more sustainable way of adding HTML and data to the DOM.
  * There are many client-side templating libraries available, with different levels of conplexity and features:
    * [Mustache](http://mustache.github.io/) is a very simple library which does not provide a way to use logic in your templates
    * [Underscore](https://underscorejs.org/#template) is a much more fully-featured library JavaScript which proivides templating among other things. Its templating allows you to write JavaScript in your templates
    * [Handlebars](http://handlebarsjs.com/) sits somewhere in-between. It keeps JavaScript out of the template, but provides syntax which allows you to add logic, such as conditional logic.

### Handlebars Basics

  * Handlebars doesn't have any other dependencies, such as jQuery or an MVC framework, so can be used on its own just by including the source in your HTML document

#### Handlebars Properties


#### Handlebars Conditionals

#### Handlebars `each`


#### Handlebars `compile` method


#### Handlebars Partials


#### Further Reading:

  * [A Beginner's Guide to Handlebars](https://www.sitepoint.com/a-beginners-guide-to-handlebars/)
