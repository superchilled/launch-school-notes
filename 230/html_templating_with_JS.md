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

  * Handlebars uses string replacement that allows you to write the names of properties as placeholders within your HTML and have the placeholders replaced with the actual propery values.
  * Handlebars syntax encloses the property names in two opening and closing curly braces `{{ }}`.

**Example**

```
<li>
  <h3>{{name}}</h3>
  <dl>
    <dt>Quantity:</dt>
    <dd>{{quantity}}</dd>
    <dt>Price:</dt>
    <dd>${{price}}</dd>
  </dl>
</li>
```

  * These property placeholders can be placed anywhere in the HTML

#### Handlebars Conditionals

  * Handlebars also has the ability to check properties for whether they are truthy values and use this in comobination with some conditional syntax in order to output part of a template based on the condition.
  * A Handlebars conditional is `false` if the property value is `false`, `undefined`, `null`, `''`, `0` or an empty array.
  * The syntax to use a conditional in a Hnadlebars template is a hash sign (`#`) followed by `if`.

**Example**

```
<li>
  <h3>{{name}}</h3>
  <dl>
    <dt>Quantity:</dt>
    <dd>{{quantity}}</dd>
    <dt>Price:</dt>
    <dd>
      ${{price}}
      {{#if on_sale}}
      <strong>SALE!</strong>
      {{/if}}
    </dd>
  </dl>
</li>
```

  * The `#` sign tells Handlebars to perform an action at this point in the template before rendering it.
  * `if` is one of a number of helpers available in Handlebars. The `if` helper has an `{{else}}` section that can be used as part of the conditional block, but `else` doesn't require the `#` sign before it.

#### Handlebars `each`

  * Another useful Handlebars helper id the `each` block. This allows you to iterate through a collection of items and perform repetitive tasks, such as outputting all the items using a single layout.

**Example**

```
{{#each items}}
<li>
 <h3>{{name}}</h3>
 <dl>
   <dt>Quantity:</dt>
   <dd>{{quantity}}</dd>
   <dt>Price:</dt>
   <dd>
     ${{price}}
     {{#if on_sale}}
     <strong>SALE!</strong>
     {{/if}}
   </dd>
 </dl>
</li>
{{/each}}
```

#### Handlebars `compile` method

  * In order to use this Handlebars functionality (properties and helpers) in a web page, we have to create templates, compile them, compile them, and then call them within the page.
  * Templates can be created in HTML using a `<script>` element with a unique `id` and a `type` of `text/x-handlebars-template`

**Example**

```
<script id='productTemplate' type='text/x-handlebars'>
<ul>
  {{#each product}}
  <li>{{name}}</li>
  {{/each}}
</ul>
</script>
```

  * The `Handlebars.compile` method converts the HTML to a template function, which can be assigned to a variable. The function can be passed an object, and it will return an HTML string with all of the properties filled in.

**Example**

```
<script id='productTemplate' type='text/x-handlebars'>
  {{#each items}}
  <li>{{name}} -- {{quantity}} -- £{{price}}</li>
  {{/each}}
</script>
<script>
var products = [{
  name: 'Banana',
  quantity: 14,
  price: 0.79
}, {
  name: 'Apple',
  quantity: 3,
  price: 0.55
}];

var productTemplate = Handlebars.compile($('#productTemplate').html());
// here we use jQuery'e html() method to read the contents of the script element

$('ul').html(productTemplate({ items: products }));
</script>
```

  * In the above example we call the `productTemplate` Handlebars function (i.e. the compiled template), and pass in an object with the property name from the template, and the value being our `products` array.

#### Handlebars Partials

  * Partials are templates that can be called within other templates.

**Example**

```
<script id='productTemplate' type='text/x-handlebars'>
  <li>{{name}} -- {{quantity}} -- £{{price}}</li>
</script>
<script id='productsList' type='text/x-handlebars'>
  {{#each items}}
  {{> productTemplate}}
  {{/each}}
</script>
```

  * In the above example, we have split our template into two templates `productTemplate` and `productsList`. In this way, `productTemplate` can be used to render a single product, or called within the `productsList` template to ouput multiple products.
  * The `>` sign tells Handlebars to look for a partial with the name `productTemplate`
  * In order to use a partial, it needs to be 'registered' as a partial, after being compiled like any other Handlebars template.

**Example**

```
// Compile both templates for use later
var productsList = Handlebars.compile($('#productsList').html());
var productTemplate = Handlebars.compile($('#productTemplate').html());

// Also register the product template as a partial
Handlebars.registerPartial('productTemplate', $('#productTemplate').html());
```

#### Further Reading:

  * [A Beginner's Guide to Handlebars](https://www.sitepoint.com/a-beginners-guide-to-handlebars/)
  * [Handlebars.js Documentation](http://handlebarsjs.com/)
