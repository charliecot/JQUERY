# Complete jQuery Guide --- Beginner to Advanced

A comprehensive learning reference for jQuery fundamentals, selectors,
DOM traversal, manipulation, events, forms, effects, AJAX, JSON, REST
APIs, authentication, CORS, CSRF, plugins, security, performance,
debugging, and practical projects.

**Audience:** Developers who know basic HTML, CSS, and JavaScript.

> jQuery is a JavaScript library, not a separate programming language.
> It simplifies common browser tasks. Modern JavaScript now provides
> native alternatives for many jQuery features, but jQuery remains
> important when maintaining existing applications and working with
> older frontend codebases.

# Table of Contents

1.  What jQuery Is
2.  Why jQuery Exists
3.  Installation
4.  The jQuery Object and `$`
5.  Document Ready
6.  Selectors
7.  Filtering
8.  DOM Traversal
9.  Content Methods
10. Attributes, Properties, and Data
11. CSS Classes and Styles
12. DOM Manipulation
13. Events
14. Event Object, Bubbling, and Delegation
15. Forms and Validation
16. Effects and Animation
17. AJAX Fundamentals
18. `$.ajax()` Settings
19. GET and POST Helpers
20. JSON and REST APIs
21. File Uploads with FormData
22. HTTP Status Codes and Error Handling
23. CORS, Cookies, JWT, and CSRF
24. jqXHR, Deferreds, and Promises
25. Chaining and Iteration
26. jQuery Utilities
27. Plugin Development
28. REST API CRUD
29. XSS and Secure DOM Updates
30. Performance
31. Debugging
32. jQuery with FastAPI and Django REST Framework
33. jQuery Compared with Modern JavaScript
34. Practical Projects
35. Common Mistakes
36. Quick Reference
37. Exercises
38. Learning Roadmap

------------------------------------------------------------------------

# 1. What jQuery Is

jQuery is a JavaScript library that provides convenient methods for:

-   Selecting HTML elements.
-   Traversing the DOM tree.
-   Reading and changing element content.
-   Changing attributes, properties, and CSS classes.
-   Handling browser events.
-   Creating, inserting, and removing elements.
-   Running simple effects and animations.
-   Sending HTTP requests through AJAX.
-   Processing JSON responses.
-   Extending functionality with plugins.

A library is reusable code written to solve common problems. Instead of
implementing the same operations repeatedly, you call functions provided
by the library.

## 1.1 JavaScript versus jQuery

HTML:

``` html
<h1 id="heading">Welcome</h1>
```

Native JavaScript:

``` javascript
const heading = document.querySelector("#heading");
heading.textContent = "Hello from JavaScript";
```

jQuery:

``` javascript
$("#heading").text("Hello from jQuery");
```

Both change the heading's text.

The expression `$("#heading")` means:

1.  `$` calls the jQuery function.
2.  `"#heading"` is a selector.
3.  jQuery searches the document.
4.  jQuery returns a collection containing the matching DOM element or
    elements.

That returned collection is a **jQuery object**, not a normal DOM
element.

## 1.2 What jQuery does not do

jQuery does not create your backend, connect directly to your database,
replace JavaScript, bypass CORS, authenticate users automatically, or
make an API secure. It runs in the browser and sends HTTP requests to a
backend that must implement the requested endpoints.

## 1.3 When should you use it?

jQuery is useful for maintaining older applications, understanding
existing codebases, working with jQuery-based plugins and templates, and
learning the history of browser scripting. For a new React application,
normally do not use jQuery to manipulate React-managed DOM nodes; React
expects to control its own rendering.

# 2. Why jQuery Exists

Historically, browsers differed in their DOM and event implementations.
jQuery provided a consistent interface for selecting elements, handling
events, manipulating the DOM, and making AJAX requests.

  --------------------------------------------------------------------------------------
  Task                    jQuery                  Native JavaScript
  ----------------------- ----------------------- --------------------------------------
  Select one element      `$("#name")`            `document.querySelector("#name")`

  Select many             `$(".item")`            `document.querySelectorAll(".item")`

  Change text             `.text("Hello")`        `.textContent = "Hello"`

  Add class               `.addClass("active")`   `.classList.add("active")`

  Listen for click        `.on("click", fn)`      `.addEventListener("click", fn)`

  HTTP request            `$.ajax(...)`           `fetch(...)`

  Create element          `$("<p>Hi</p>")`        `document.createElement("p")`
  --------------------------------------------------------------------------------------

Modern JavaScript has standardized many of the features that made jQuery
essential in the past. Still, jQuery is widely encountered in existing
projects.

# 3. Installation

## 3.1 CDN

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>jQuery Demo</title>
</head>
<body>
    <h1 id="heading">Hello</h1>

    <script src="https://code.jquery.com/jquery-3.7.1.min.js"></script>
    <script>
        $("#heading").text("jQuery is working");
    </script>
</body>
</html>
```

The jQuery script must load before your code that uses `$`. Otherwise,
you may see:

``` text
ReferenceError: $ is not defined
```

## 3.2 Local installation

Example structure:

``` text
my-project/
├── index.html
├── css/
│   └── style.css
└── js/
    ├── jquery-3.7.1.min.js
    └── app.js
```

Load in this order:

``` html
<script src="js/jquery-3.7.1.min.js"></script>
<script src="js/app.js"></script>
```

`jquery.js` is usually the readable development build; `jquery.min.js`
is minified to reduce file size.

## 3.3 Verify the installation

``` javascript
console.log(jQuery);
console.log($);
console.log(jQuery.fn.jquery);
```

The last expression prints the version.

# 4. The jQuery Object and `$`

`$` is an alias for `jQuery`. These are equivalent:

``` javascript
$("#message");
jQuery("#message");
```

## 4.1 Access the underlying DOM element

``` javascript
const jqueryObject = $("#message");
const domElement = jqueryObject[0];

// Also:
const anotherElement = jqueryObject.get(0);
```

Convert a DOM element into a jQuery object:

``` javascript
const element = document.querySelector("#message");
const wrapped = $(element);
```

## 4.2 Collections

``` html
<p class="note">First</p>
<p class="note">Second</p>
<p class="note">Third</p>
```

``` javascript
const notes = $(".note");

console.log(notes.length); // 3
console.log(notes[0]);     // First DOM element
console.log(notes[1]);     // Second DOM element
```

Most jQuery methods operate on every matched element:

``` javascript
$(".note").addClass("highlight");
```

## 4.3 Empty selections

``` javascript
const missing = $("#not-present");
console.log(missing.length); // 0
```

Most jQuery methods can be called on an empty collection without
throwing an error. Check existence when the distinction matters:

``` javascript
if ($("#not-present").length > 0) {
    console.log("Found it");
}
```

## 4.4 `$` conflicts

Another library may use `$`. You can use `jQuery` explicitly or call
`jQuery.noConflict()`:

``` javascript
jQuery.noConflict();

jQuery(function ($) {
    $("#message").text("Here $ refers to jQuery");
});
```

# 5. Document Ready

The DOM must exist before code tries to select its elements.

``` javascript
$(document).ready(function () {
    console.log("DOM is ready");
});
```

Short form:

``` javascript
$(function () {
    console.log("DOM is ready");
});
```

Document ready means the HTML has been parsed and the DOM is ready for
manipulation. It does not necessarily wait for every image or external
resource.

Compare with window load:

``` javascript
$(window).on("load", function () {
    console.log("Page resources have loaded");
});
```

Modern equivalent:

``` javascript
document.addEventListener("DOMContentLoaded", function () {
    console.log("DOM ready");
});
```

# 6. Selectors

Selectors identify the elements on which jQuery should operate.

Example HTML:

``` html
<div id="container">
    <h1 class="title">Products</h1>
    <p class="description">Available products</p>
    <ul>
        <li class="item">Keyboard</li>
        <li class="item featured">Mouse</li>
        <li class="item">Monitor</li>
    </ul>
    <input type="text" name="username">
    <button type="button">Submit</button>
</div>
```

## 6.1 Basic selectors

``` javascript
$("*");          // All elements
$("p");          // All paragraphs
$("#container"); // ID
$(".item");      // Class
$("h1, p, button"); // Group of selectors
```

## 6.2 Relationship selectors

``` javascript
$("#container li");      // Descendant li elements
$("#container > h1");    // Direct child h1
$("h1 + p");             // Immediately following sibling
$("h1 ~ p");             // Later sibling paragraphs
```

-   `A B`: any descendant.
-   `A > B`: direct child.
-   `A + B`: immediately following sibling.
-   `A ~ B`: later sibling with the same parent.

## 6.3 Attribute selectors

``` javascript
$("input[name='username']");
$("input[type='text']");
$("a[href]");
$("a[target='_blank']");

$("[name]");                 // Has attribute
$("[name^='user']");         // Starts with
$("[name$='id']");           // Ends with
$("[name*='account']");      // Contains
```

## 6.4 Form selectors

``` javascript
$(":input");
$(":text");
$(":password");
$(":checkbox");
$(":radio");
$(":submit");
$(":checked");
$(":selected");
$(":disabled");
$(":enabled");
```

These include jQuery selector extensions. Standard CSS selectors such as
`input[type='checkbox']` are often clearer and more portable.

## 6.5 Position and content selectors

``` javascript
$("li:first");
$("li:last");
$("li:even");
$("li:odd");
$("li:eq(2)");
$("li:gt(1)");
$("li:lt(3)");
$("p:contains('available')");
$("div:has(p)");
$("p:empty");
```

`:eq(2)` uses a zero-based index. Some of these are jQuery-specific
pseudo-selectors rather than standard CSS selectors.

## 6.6 Dynamic selector values

Do not concatenate arbitrary user input into selectors without escaping
it. For an ID, a safe option is:

``` javascript
const id = "item:one";
const element = document.getElementById(id);
const wrapped = $(element);
```

If constructing a selector is necessary:

``` javascript
const item = $("#" + $.escapeSelector(id));
```

# 7. Filtering

``` javascript
$(".item").first();
$(".item").last();
$(".item").eq(1);
$(".item").filter(".featured");
$(".item").not(".featured");
$(".item").is(".featured");
$("div").has("p");
$("h1").add("p");
```

Callback filtering:

``` javascript
$(".item").filter(function (index) {
    return index > 0;
});
```

Inside a normal callback, `this` refers to the current DOM element.

`.end()` returns to the previous collection in a chain:

``` javascript
$("ul")
    .find("li")
    .filter(".featured")
    .addClass("highlight")
    .end()
    .addClass("processed");
```

It does not undo changes.

# 8. DOM Traversal

The DOM is a tree. Traversal means moving between related elements.

``` javascript
$(".featured").parent();          // Immediate parent
$(".featured").parents();         // All ancestors
$(".featured").parents(".category");
$("ul").children();               // Direct children
$("#shop").find(".product");      // Descendants
$(".featured").siblings();        // Siblings
$(".featured").next();            // Next sibling
$(".featured").prev();            // Previous sibling
$(".featured").nextAll();
$(".featured").prevAll();
$(".featured").nextUntil(".last");
$(".featured").prevUntil(".first");
$(".featured").closest(".category");
$("#container").contents();       // Includes text nodes
```

Key differences:

-   `.children()` only checks direct children.
-   `.find()` searches descendants at any depth.
-   `.parent()` gets one immediate parent.
-   `.parents()` gets all ancestors.
-   `.closest()` searches upward and returns the first match, including
    the selected element itself.

Example:

``` html
<div id="shop">
    <section class="category">
        <h2>Electronics</h2>
        <ul>
            <li class="product">Keyboard</li>
            <li class="product featured">Mouse</li>
            <li class="product">Monitor</li>
        </ul>
    </section>
</div>
```

From the featured `li`, `.parent()` is `ul`, `.closest(".category")` is
the section, `.next()` is Monitor, and `.prev()` is Keyboard.

# 9. Content Methods

## 9.1 `.text()`

Read text:

``` javascript
const value = $("#message").text();
```

Set text:

``` javascript
$("#message").text("A new message");
```

`.text()` treats the value as text, not HTML.

``` javascript
$("#message").text("<strong>Hello</strong>");
```

The tags are displayed as characters.

## 9.2 `.html()`

Read inner HTML:

``` javascript
const markup = $("#message").html();
```

Set HTML:

``` javascript
$("#message").html("<strong>Hello</strong>");
```

**Security warning:** `.html()` parses markup. Do not place untrusted
user input or untrusted API values into it without a reputable
sanitizer. For plain text, use `.text()`.

Unsafe:

``` javascript
const comment = new URLSearchParams(location.search).get("comment");
$("#comments").html(comment);
```

Safer:

``` javascript
$("#comments").text(comment ?? "");
```

## 9.3 `.val()`

Read and set form control values:

``` javascript
const username = $("#username").val();
$("#username").val("charles");
```

Use `.val()` for inputs, textareas, and select controls.

  Method      Purpose
  ----------- --------------------
  `.text()`   Text content
  `.html()`   HTML markup
  `.val()`    Form control value

# 10. Attributes, Properties, and Data Attributes

HTML:

``` html
<input id="email" type="email" required value="a@example.com">
```

An **attribute** is part of the HTML markup. A DOM **property**
represents the current live state exposed by the browser.

## 10.1 `.attr()`

``` javascript
$("#email").attr("type");
$("#email").attr("placeholder", "Enter email");
$("#email").removeAttr("required");

$("#email").attr({
    placeholder: "Email address",
    autocomplete: "email"
});
```

## 10.2 `.prop()`

Use properties for live states such as checked, selected, and disabled:

``` javascript
$("#terms").prop("checked");
$("#terms").prop("checked", true);
$("#submit").prop("disabled", true);
$("#submit").prop("disabled", false);
```

Example of the difference:

``` javascript
const input = $("#username");

console.log(input.attr("value")); // Initial HTML attribute
console.log(input.prop("value")); // Current live value

input.val("changed");

console.log(input.attr("value")); // Usually remains initial value
console.log(input.prop("value")); // "changed"
```

## 10.3 Data attributes

HTML:

``` html
<button id="delete" data-user-id="42" data-action="delete">Delete</button>
```

Read and set jQuery data:

``` javascript
const userId = $("#delete").data("user-id");
$("#delete").data("confirmed", true);
$("#delete").removeData("confirmed");
```

`.data()` may cache values internally. If you need the current literal
HTML attribute, use `.attr("data-user-id")`.

# 11. CSS Classes and Styles

``` javascript
$("#message").addClass("success");
$("#message").addClass("success visible");
$("#message").removeClass("success");
$("#message").removeClass();
$("#menu").toggleClass("open");
$("#menu").toggleClass("open", true);
$("#menu").toggleClass("open", false);
$("#menu").hasClass("open");
```

Read a computed style:

``` javascript
const color = $("#message").css("color");
```

Set styles:

``` javascript
$("#message").css("color", "red");

$("#message").css({
    color: "white",
    backgroundColor: "green",
    padding: "12px",
    borderRadius: "6px"
});
```

Prefer reusable CSS classes for visual states. Keep presentation in CSS
and behavior in JavaScript.

# 12. DOM Manipulation

## 12.1 Create elements

``` javascript
const paragraph = $("<p>");
paragraph.text("A new paragraph");

const button = $("<button>", {
    type: "button",
    class: "save-button",
    text: "Save"
});
```

Use `.text()` for dynamic untrusted text rather than interpolating it
into an HTML string.

## 12.2 Insert elements

``` javascript
$("#list").append("<li>Last item</li>");
$("#list").prepend("<li>First item</li>");
$("#target").before("<p>Before</p>");
$("#target").after("<p>After</p>");

$("<li>").text("New").appendTo("#list");
$("<li>").text("First").prependTo("#list");
$("<p>Before</p>").insertBefore("#target");
$("<p>After</p>").insertAfter("#target");
```

Safer dynamic insertion:

``` javascript
const item = $("<li>").text(userSuppliedText);
$("#list").append(item);
```

## 12.3 Remove and replace

``` javascript
$("#list").empty();            // Remove children
$(".temporary").remove();      // Remove selected nodes
const saved = $("#panel").detach(); // Remove but preserve jQuery data/events
$("#container").append(saved); // Reinsert later
$("#old").replaceWith("<p>Replacement</p>");
const copy = $("#template").clone();
```

Copy jQuery data and event handlers too:

``` javascript
const copy = $("#template").clone(true, true);
```

Prefer delegated events for dynamic content rather than cloning handlers
unnecessarily.

## 12.4 Wrapping

``` javascript
$(".product").wrap("<div class='wrapper'></div>");
$(".product").wrapAll("<section class='products'></section>");
$("#message").wrapInner("<strong></strong>");
$(".product").unwrap();
```

These change the document structure; inspect the resulting DOM.

# 13. Events

An event is something that happens in the browser: a click, key press,
form submission, input change, pointer movement, or page load.

Preferred general event method:

``` javascript
$("#save").on("click", function () {
    console.log("Clicked");
});
```

Common event names:

  -----------------------------------------------------------------------
  Event                               Meaning
  ----------------------------------- -----------------------------------
  `click`                             Click/activation

  `dblclick`                          Double click

  `mouseenter` / `mouseleave`         Pointer enters/leaves

  `mouseover` / `mouseout`            Pointer enters/leaves, including
                                      descendant transitions

  `mousedown` / `mouseup`             Pointer button pressed/released

  `mousemove`                         Pointer movement

  `keydown` / `keyup`                 Key pressed/released

  `input`                             Value changes during editing

  `change`                            Committed value change, depending
                                      on control

  `submit`                            Form submission

  `focus` / `blur`                    Focus gained/lost

  `focusin` / `focusout`              Focus enters/leaves an element or
                                      descendant

  `scroll`                            Scroll position changes

  `resize`                            Window resize
  -----------------------------------------------------------------------

Older shorthand:

``` javascript
$("#save").click(function () {
    console.log("Clicked");
});
```

Prefer `.on()` for consistency and delegation.

Remove handlers:

``` javascript
$("#save").off("click");
```

Run once:

``` javascript
$("#continue").one("click", function () {
    console.log("Runs once");
});
```

# 14. Event Object, Bubbling, and Delegation

## 14.1 Event object

``` javascript
$("#save").on("click", function (event) {
    console.log(event.type);
    console.log(event.target);
    console.log(event.currentTarget);
    console.log(event.delegateTarget);
});
```

-   `target`: element where the event originated.
-   `currentTarget`: element whose handler is executing.
-   `delegateTarget`: element where a delegated handler was attached.
-   `type`: event name.

In a normal jQuery event callback, `this` refers to the element handling
the event (or the matched delegated element).

``` javascript
$(".item").on("click", function () {
    console.log(this);
    console.log($(this).text());
});
```

Avoid arrow functions when you depend on jQuery's `this`:

``` javascript
// Not suitable if you need jQuery callback this:
$(".item").on("click", () => console.log(this));
```

## 14.2 Prevent default behavior

``` javascript
$("a").on("click", function (event) {
    event.preventDefault();
});
```

This cancels the browser's default action, such as link navigation or
form submission. Use only when your code intentionally replaces that
action.

## 14.3 Stop propagation

``` javascript
$("#inner").on("click", function (event) {
    event.stopPropagation();
});
```

This stops the event bubbling to ancestors. It does not cancel the
default action.

`stopImmediatePropagation()` also prevents later handlers on the same
element from running. Use it sparingly.

## 14.4 Event bubbling

``` html
<div id="parent">
    <button id="child">Click</button>
</div>
```

``` javascript
$("#child").on("click", function () {
    console.log("Child");
});

$("#parent").on("click", function () {
    console.log("Parent");
});
```

Clicking the button normally logs Child and then Parent because the
event bubbles.

## 14.5 Event delegation

Direct binding:

``` javascript
$(".delete-button").on("click", handler);
```

This attaches to buttons that exist at that moment. It does not
automatically attach to future buttons created by AJAX.

Delegation:

``` javascript
$("#product-list").on("click", ".delete-button", function () {
    $(this).closest("li").remove();
});
```

The handler is attached to a stable parent and processes events from
matching descendants, including descendants added later. Prefer the
nearest stable ancestor over `document` when possible.

## 14.6 Namespaces

``` javascript
$("#save").on("click.editor", saveHandler);
$("#save").off(".editor");
```

Namespaces let you remove only handlers belonging to a feature.

Trigger a jQuery event:

``` javascript
$("#save").trigger("click");
```

A triggered jQuery event is not necessarily identical to a real user
interaction. Do not use it as a security boundary.

# 15. Forms and Validation

HTML:

``` html
<form id="register-form">
    <label>Username <input id="username" name="username" required></label>
    <label>Email <input id="email" name="email" type="email" required></label>
    <button type="submit">Register</button>
</form>
<p id="message"></p>
```

Handle submission:

``` javascript
$("#register-form").on("submit", function (event) {
    event.preventDefault();

    const username = $("#username").val();
    const email = $("#email").val();

    console.log({ username, email });
});
```

Checkbox:

``` javascript
const accepted = $("#terms").prop("checked");
```

Radio selection:

``` javascript
const role = $("input[name='role']:checked").val();
```

Select:

``` javascript
const country = $("#country").val();
```

## 15.1 Serialize forms

``` javascript
const queryString = $("#register-form").serialize();
console.log(queryString);
```

Possible output:

``` text
username=charles&email=charles%40example.com
```

`.serialize()` includes successful form controls. Disabled controls,
unchecked checkboxes/radios, and controls without names are generally
omitted.

Array representation:

``` javascript
const values = $("#register-form").serializeArray();
```

Possible result:

``` javascript
[
    { name: "username", value: "charles" },
    { name: "email", value: "charles@example.com" }
]
```

## 15.2 Browser validation

``` html
<input type="email" required>
<input type="password" minlength="8" required>
<input type="number" min="1" max="100">
```

``` javascript
const form = $("#register-form")[0];

if (form.checkValidity()) {
    console.log("Valid");
} else {
    form.reportValidity();
}
```

Client-side validation is for user experience. Always validate on the
backend because attackers can bypass browser JavaScript.

# 16. Effects and Animation

``` javascript
$("#panel").hide();
$("#panel").show();
$("#panel").toggle();

$("#panel").fadeIn();
$("#panel").fadeOut();
$("#panel").fadeToggle();
$("#panel").fadeTo(500, 0.5);

$("#panel").slideDown();
$("#panel").slideUp();
$("#panel").slideToggle();
```

Durations are milliseconds or named speeds such as `"slow"` and
`"fast"`:

``` javascript
$("#panel").fadeIn(400);
```

Callback after completion:

``` javascript
$("#panel").fadeOut(400, function () {
    console.log("Finished");
});
```

Animate CSS properties:

``` javascript
$("#box").animate({
    left: "200px",
    opacity: 0.5,
    width: "300px"
}, 600);
```

For positional animation, the element generally needs positioning:

``` css
#box { position: relative; }
```

Stop an animation:

``` javascript
$("#box").stop();
$("#box").stop(true);
$("#box").stop(true, true);
```

The second argument to `.stop()` controls whether the current animation
jumps to its end state. Effects can be queued:

``` javascript
$("#box")
    .slideUp(300)
    .slideDown(300)
    .fadeOut(300);
```

Consider accessibility and reduced-motion preferences. CSS transitions
may be better for simple visual changes.

# 17. AJAX Fundamentals

AJAX means **Asynchronous JavaScript and XML**. Despite the historical
name, modern applications commonly exchange JSON.

AJAX lets a page communicate with a server without reloading the entire
page.

``` text
User action
    |
jQuery event handler
    |
HTTP request
    |
Backend endpoint
    |
Backend validation / database work
    |
HTTP response, often JSON
    |
JavaScript processes response
    |
Page updates
```

Asynchronous means the browser can continue handling other work while
waiting for the server. AJAX is asynchronous by default. Avoid
synchronous AJAX (`async: false`) because it blocks the browser's main
thread.

An HTTP request can contain a method, URL, headers, query parameters,
body, and credentials. An HTTP response contains a status code, headers,
and body.

Example:

``` http
GET /api/products/ HTTP/1.1
Host: example.com
Accept: application/json
```

Possible JSON response:

``` json
[
    {"id": 1, "name": "Keyboard", "price": "25.00"},
    {"id": 2, "name": "Mouse", "price": "12.50"}
]
```

# 18. `$.ajax()` in Detail

Basic form:

``` javascript
$.ajax({
    url: "/api/products/",
    method: "GET",
    dataType: "json",

    success: function (data) {
        console.log(data);
    },

    error: function (xhr, status, error) {
        console.error(xhr.status, status, error);
    }
});
```

## 18.1 Important settings

  Setting             Meaning
  ------------------- ------------------------------------------------
  `url`               Request destination
  `method` / `type`   HTTP method
  `data`              Data to send
  `dataType`          Expected response type
  `contentType`       Request body content type
  `headers`           Additional request headers
  `timeout`           Timeout in milliseconds
  `beforeSend`        Runs before request is sent
  `success`           Successful response callback
  `error`             Failed request callback
  `complete`          Runs after success or failure
  `processData`       Whether jQuery converts data to a query string
  `cache`             Controls cache behavior for certain requests
  `xhrFields`         Sets properties on XMLHttpRequest
  `statusCode`        Callbacks for specific status codes

## 18.2 `url` and `method`

``` javascript
$.ajax({
    url: "http://127.0.0.1:8000/api/products/",
    method: "GET"
});
```

Common methods:

-   GET: retrieve data.
-   POST: create a resource or perform an operation.
-   PUT: commonly replace a resource representation.
-   PATCH: partially update a resource.
-   DELETE: delete a resource.

The backend defines exactly what each endpoint supports.

## 18.3 `dataType` versus `contentType`

This distinction is essential:

-   `contentType` describes the request body you send.
-   `dataType` tells jQuery what response format you expect.

JSON POST:

``` javascript
$.ajax({
    url: "/api/products/",
    method: "POST",
    contentType: "application/json; charset=UTF-8",
    dataType: "json",
    data: JSON.stringify({
        name: "Keyboard",
        price: 25
    })
});
```

`JSON.stringify()` converts a JavaScript object into JSON text. A JSON
request body is not the same as passing a raw JavaScript object.

## 18.4 GET query parameters

``` javascript
$.ajax({
    url: "/api/products/",
    method: "GET",
    dataType: "json",
    data: {
        search: "keyboard",
        page: 2
    }
});
```

jQuery serializes the data into a query string such as:

``` text
/api/products/?search=keyboard&page=2
```

## 18.5 Headers and callbacks

``` javascript
$.ajax({
    url: "/api/products/",
    method: "GET",
    headers: {
        "X-Client-Name": "jquery-demo"
    },

    beforeSend: function (xhr) {
        console.log("Sending request");
    },

    success: function (data, textStatus, jqXHR) {
        console.log(data);
        console.log(textStatus);
        console.log(jqXHR.status);
    },

    error: function (jqXHR, textStatus, errorThrown) {
        console.error(jqXHR.status);
        console.error(jqXHR.responseText);
        console.error(textStatus);
        console.error(errorThrown);
    },

    complete: function (jqXHR, textStatus) {
        console.log("Request finished:", textStatus);
    },

    timeout: 10000
});
```

A timeout means the client stopped waiting according to its configured
timeout. It does not prove that the server never received the request.

## 18.6 Status-specific handlers

``` javascript
$.ajax({
    url: "/api/profile/",
    statusCode: {
        401: function () { console.log("Authentication required"); },
        403: function () { console.log("Permission denied"); },
        404: function () { console.log("Not found"); }
    }
});
```

## 18.7 Complete example

``` javascript
$(function () {
    $("#load-products").on("click", function () {
        $("#message").text("Loading...");

        $.ajax({
            url: "http://127.0.0.1:8000/api/products/",
            method: "GET",
            dataType: "json",
            timeout: 10000,

            success: function (products) {
                $("#message").text("Products loaded");
                renderProducts(products);
            },

            error: function (xhr, status, error) {
                $("#message").text("Could not load products");
                console.error({
                    statusCode: xhr.status,
                    status: status,
                    error: error,
                    response: xhr.responseText
                });
            },

            complete: function () {
                console.log("Request completed");
            }
        });
    });

    function renderProducts(products) {
        const list = $("#product-list").empty();

        products.forEach(function (product) {
            list.append(
                $("<li>").text(product.name + " — " + product.price)
            );
        });
    }
});
```

Notice that `.text()` is used for API values. It does not interpret
product names as HTML.

# 19. GET and POST Helpers

GET:

``` javascript
$.get("/api/products/", function (data) {
    console.log(data);
});
```

GET with query data and JSON response:

``` javascript
$.get(
    "/api/products/",
    { page: 2, search: "mouse" },
    function (data) {
        console.log(data);
    },
    "json"
);
```

POST form-style data:

``` javascript
$.post("/api/contact/", {
    name: "Charles",
    message: "Hello"
}, function (response) {
    console.log(response);
});
```

`$.post()` normally serializes ordinary objects as form-style data. It
does not automatically send JSON.

For JSON, use `$.ajax()`:

``` javascript
$.ajax({
    url: "/api/contact/",
    method: "POST",
    contentType: "application/json",
    dataType: "json",
    data: JSON.stringify({
        name: "Charles",
        message: "Hello"
    })
});
```

Request JSON conveniently:

``` javascript
$.getJSON("/api/products/", function (data) {
    console.log(data);
});
```

Use `$.ajax()` when you need full control over headers, body format,
methods, timeouts, and error handling.

# 20. JSON and REST APIs

JSON is a text format for structured data.

``` json
{
    "id": 10,
    "username": "charles",
    "active": true,
    "roles": ["student", "developer"]
}
```

JavaScript object:

``` javascript
const user = {
    username: "charles",
    active: true
};
```

Convert to JSON text:

``` javascript
const jsonText = JSON.stringify(user);
```

Parse JSON text:

``` javascript
const parsed = JSON.parse(jsonText);
```

When `dataType: "json"` is used and the server returns valid JSON,
jQuery parses the response.

## 20.1 Nested JSON

``` json
{
    "id": 1,
    "name": "Band A",
    "albums": [
        {"id": 10, "title": "First Album"},
        {"id": 11, "title": "Second Album"}
    ]
}
```

``` javascript
$.getJSON("/api/bands/1/", function (band) {
    console.log(band.name);

    band.albums.forEach(function (album) {
        console.log(album.title);
    });
});
```

## 20.2 Pagination

A paginated response may look like:

``` json
{
    "count": 23,
    "next": "http://127.0.0.1:8000/api/products/?page=2",
    "previous": null,
    "results": [
        {"id": 1, "name": "Keyboard"}
    ]
}
```

``` javascript
$.getJSON("/api/products/", function (response) {
    renderProducts(response.results);

    if (response.next) {
        console.log("Next page:", response.next);
    }
});
```

Do not assume every API has the same pagination structure. Inspect its
actual response.

# 21. File Uploads with FormData

HTML:

``` html
<form id="cv-form">
    <input type="text" name="title" required>
    <input type="file" name="cv" accept=".pdf,.doc,.docx" required>
    <button type="submit">Upload</button>
</form>
<p id="upload-message"></p>
```

jQuery:

``` javascript
$("#cv-form").on("submit", function (event) {
    event.preventDefault();

    const formData = new FormData(this);

    $.ajax({
        url: "http://127.0.0.1:8000/api/upload-cv/",
        method: "POST",
        data: formData,

        // Do not turn FormData into a query string.
        processData: false,

        // Let the browser set multipart boundary automatically.
        contentType: false,

        success: function (response) {
            $("#upload-message").text("Upload successful");
            console.log(response);
        },

        error: function (xhr) {
            $("#upload-message").text("Upload failed");
            console.error(xhr.responseText);
        }
    });
});
```

Why `processData: false`? jQuery normally serializes ordinary objects.
FormData must be transmitted as multipart data.

Why `contentType: false`? The browser generates the multipart content
type including its boundary. Do not manually set `multipart/form-data`
without the correct boundary.

The backend must validate file size, extension, actual content,
filename, storage path, authorization, and download permissions. The
HTML `accept` attribute is only a user-interface hint.

# 22. HTTP Status Codes and Error Handling

  Status   Meaning
  -------- -------------------------------------------------
  200      OK
  201      Created
  202      Accepted
  204      No Content
  301      Moved Permanently
  302      Found / temporary redirect
  304      Not Modified
  400      Bad Request
  401      Authentication required
  403      Forbidden
  404      Not Found
  405      Method Not Allowed
  409      Conflict
  415      Unsupported Media Type
  422      Unprocessable Content; often validation failure
  429      Too Many Requests
  500      Internal Server Error
  502      Bad Gateway
  503      Service Unavailable
  504      Gateway Timeout

Inspect an error:

``` javascript
error: function (xhr, textStatus, errorThrown) {
    console.log(xhr.status);
    console.log(xhr.statusText);
    console.log(xhr.responseText);
    console.log(textStatus);
    console.log(errorThrown);
}
```

A validation response might be:

``` json
{
    "email": ["Enter a valid email address."],
    "password": ["This field is required."]
}
```

``` javascript
error: function (xhr) {
    if (xhr.status === 400 || xhr.status === 422) {
        console.log(xhr.responseJSON);
    }
}
```

`responseJSON` may be undefined if the response is not valid JSON.

Distinguish:

-   HTTP error: server or intermediary returned a status such as 404 or
    500.
-   Network error: browser could not complete the connection, perhaps
    due to DNS, TLS, offline state, or browser policy.
-   Timeout: configured wait period elapsed.
-   Parser error: response could not be interpreted as the expected
    format.

CORS failures often appear to JavaScript as generic network errors; the
browser console provides more detail.

# 23. CORS, Cookies, JWT, and CSRF

This is particularly important when the frontend and backend use
different origins.

## 23.1 Origin

An origin consists of scheme, host, and port. These are different
origins:

``` text
http://localhost:5173
http://127.0.0.1:8000
```

Different ports and even different host spellings matter.

## 23.2 CORS

CORS means Cross-Origin Resource Sharing. It is a browser-enforced
mechanism controlling whether JavaScript on one origin can read a
response from another origin.

A server may return:

``` http
Access-Control-Allow-Origin: http://localhost:5173
```

For credentialed cross-origin requests, the server must allow the exact
origin and return:

``` http
Access-Control-Allow-Credentials: true
```

A wildcard `*` cannot be used for credentialed CORS requests.

CORS is not authentication or authorization. It does not replace CSRF
defenses, input validation, or rate limiting.

## 23.3 Preflight

Some cross-origin requests cause the browser to send an `OPTIONS`
preflight request first. A JSON POST or a request with an
`Authorization` header commonly triggers preflight.

The backend must allow the requested method and headers and handle
`OPTIONS` correctly.

## 23.4 JWT Bearer token

An API may expect:

``` http
Authorization: Bearer ACCESS_TOKEN
```

jQuery:

``` javascript
const accessToken = getAccessToken();

$.ajax({
    url: "http://127.0.0.1:8000/api/profile/",
    method: "GET",
    dataType: "json",
    headers: {
        Authorization: "Bearer " + accessToken
    }
});
```

`getAccessToken()` is an application function you must implement. Never
place a server signing key or secret in frontend code. Anything sent to
a browser can be inspected.

## 23.5 Cookie credentials

For cross-origin cookie requests:

``` javascript
$.ajax({
    url: "https://api.example.com/profile/",
    method: "GET",
    xhrFields: {
        withCredentials: true
    }
});
```

The server must configure credentialed CORS. Cookie `SameSite`,
`Secure`, domain, path, and browser privacy rules also matter.

## 23.6 CSRF

CSRF means Cross-Site Request Forgery. It is especially relevant to
cookie authentication because browsers may automatically attach cookies.

A backend may require a CSRF token header:

``` javascript
$.ajax({
    url: "/api/update-profile/",
    method: "POST",
    headers: {
        "X-CSRFToken": csrfToken
    },
    data: {
        display_name: "Charles"
    }
});
```

The exact token acquisition and header name depend on backend
configuration. For Django, follow its CSRF mechanism. Do not disable
CSRF protection just to make AJAX work.

## 23.7 Token storage

Tokens in `localStorage` are accessible to JavaScript. An XSS
vulnerability can expose them. HttpOnly cookies prevent JavaScript from
reading the cookie value, but cookie authentication needs correct CSRF
defenses and cookie settings. Choose an architecture deliberately.

# 24. jqXHR, Deferreds, and Promises

`$.ajax()` returns a `jqXHR` object with XHR features and a promise-like
interface.

``` javascript
$.ajax({
    url: "/api/products/",
    dataType: "json"
}).done(function (data) {
    console.log(data);
});
```

Failure:

``` javascript
$.ajax({
    url: "/api/products/"
}).fail(function (xhr, status, error) {
    console.error(status, error);
});
```

Always:

``` javascript
$.ajax({
    url: "/api/products/"
})
.done(function (data) {
    console.log(data);
})
.fail(function (xhr) {
    console.error(xhr.status);
})
.always(function () {
    console.log("Request finished");
});
```

Store and abort a request:

``` javascript
const request = $.ajax({
    url: "/api/search/",
    data: { q: "keyboard" }
});

// When the request is no longer needed:
request.abort();
```

You can use either `success`/`error` callbacks in the settings object or
`.done()`/`.fail()` methods. Keep the style consistent in a project.

Modern JavaScript alternative:

``` javascript
async function loadProducts() {
    try {
        const response = await fetch("/api/products/");

        if (!response.ok) {
            throw new Error("HTTP error: " + response.status);
        }

        const products = await response.json();
        console.log(products);
    } catch (error) {
        console.error(error);
    }
}
```

Unlike jQuery AJAX, `fetch()` does not reject merely because the server
returned 404 or 500. Check `response.ok`.

# 25. Chaining and Iteration

Chain methods that return the jQuery collection:

``` javascript
$("#message")
    .text("Saved")
    .addClass("success")
    .show();
```

Getter methods commonly return a value instead of the jQuery collection:

``` javascript
const text = $("#message").text();
```

Iterate through a jQuery collection:

``` javascript
$("li").each(function (index, element) {
    console.log(index);
    console.log(element);
    console.log($(this).text());
});
```

Return `false` from an `.each()` callback to stop iteration.

Map to an array:

``` javascript
const names = $(".product").map(function () {
    return $(this).text();
}).get();
```

# 26. jQuery Utilities

These are utility functions rather than methods on a particular selected
element.

## `$.each()`

``` javascript
const users = ["Alice", "Bob", "Carol"];

$.each(users, function (index, user) {
    console.log(index, user);
});
```

Objects:

``` javascript
const user = { name: "Charles", role: "Developer" };

$.each(user, function (key, value) {
    console.log(key, value);
});
```

## `$.map()`

``` javascript
const doubled = $.map([1, 2, 3], function (number) {
    return number * 2;
});
```

## `$.grep()`

``` javascript
const even = $.grep([1, 2, 3, 4, 5], function (number) {
    return number % 2 === 0;
});
```

## `$.extend()`

``` javascript
const defaults = { theme: "light", pageSize: 10 };
const options = { pageSize: 20 };

const settings = $.extend({}, defaults, options);
```

Deep merge:

``` javascript
const merged = $.extend(true, {}, defaults, options);
```

Be careful merging untrusted objects into sensitive application objects.

## Other utilities

``` javascript
$.contains(parent, child);
$.inArray("Bob", ["Alice", "Bob", "Carol"]);
$.param({ search: "blue keyboard", page: 2 });
$.escapeSelector("item:one");
```

Modern JavaScript alternatives include `Array.isArray()`,
`String.prototype.trim()`, `indexOf()`, `URLSearchParams`, and
`Object.assign()` in appropriate situations. Older jQuery code may
contain `$.trim()`, `$.isArray()`, or `$.parseJSON()`; prefer native
equivalents in modern code.

# 27. Plugin Development

A plugin adds functionality to jQuery. Common plugin categories include
date pickers, data tables, validation, select enhancements, editors,
galleries, and dialogs.

A plugin usually requires jQuery, the plugin script, sometimes CSS, and
initialization code. Load jQuery before a plugin that depends on it.

## 27.1 Simple plugin

``` javascript
(function ($) {
    $.fn.highlightText = function (color) {
        const selectedColor = color || "yellow";

        return this.each(function () {
            $(this).css("backgroundColor", selectedColor);
        });
    };
})(jQuery);
```

Usage:

``` javascript
$(".notice").highlightText("lightblue");
```

Explanation:

-   `$.fn` is the prototype used by jQuery collections.
-   Adding a method to `$.fn` makes it available on jQuery objects.
-   `this` is the current jQuery collection.
-   `.each()` processes every matched element.
-   `return this` preserves chaining.

## 27.2 Plugin options

``` javascript
(function ($) {
    $.fn.noticeBox = function (options) {
        const defaults = {
            color: "black",
            background: "lightyellow",
            padding: "10px"
        };

        const settings = $.extend({}, defaults, options);

        return this.each(function () {
            $(this).css({
                color: settings.color,
                backgroundColor: settings.background,
                padding: settings.padding
            });
        });
    };
})(jQuery);
```

Usage:

``` javascript
$(".notice").noticeBox({
    color: "navy",
    background: "lightblue"
});
```

# 28. REST API CRUD

A REST-style API might expose:

``` text
GET     /api/products/       List
POST    /api/products/       Create
GET     /api/products/5/     Retrieve one
PUT     /api/products/5/     Replace
PATCH   /api/products/5/     Partially update
DELETE  /api/products/5/     Delete
```

The backend must implement these routes.

## 28.1 GET

``` javascript
function getProducts() {
    return $.ajax({
        url: "http://127.0.0.1:8000/api/products/",
        method: "GET",
        dataType: "json"
    });
}
```

## 28.2 POST

``` javascript
function createProduct(product) {
    return $.ajax({
        url: "http://127.0.0.1:8000/api/products/",
        method: "POST",
        contentType: "application/json",
        dataType: "json",
        data: JSON.stringify(product)
    });
}

createProduct({ name: "Keyboard", price: "45.00" })
    .done(function (created) {
        console.log("Created:", created);
    })
    .fail(function (xhr) {
        console.error(xhr.status, xhr.responseText);
    });
```

## 28.3 PUT

``` javascript
function replaceProduct(id, product) {
    return $.ajax({
        url: "http://127.0.0.1:8000/api/products/" + encodeURIComponent(id) + "/",
        method: "PUT",
        contentType: "application/json",
        dataType: "json",
        data: JSON.stringify(product)
    });
}
```

PUT commonly expects a complete representation, depending on the API
contract.

## 28.4 PATCH

``` javascript
function updateProduct(id, changes) {
    return $.ajax({
        url: "http://127.0.0.1:8000/api/products/" + encodeURIComponent(id) + "/",
        method: "PATCH",
        contentType: "application/json",
        dataType: "json",
        data: JSON.stringify(changes)
    });
}

updateProduct(5, { price: "49.00" });
```

## 28.5 DELETE

``` javascript
function deleteProduct(id) {
    return $.ajax({
        url: "http://127.0.0.1:8000/api/products/" + encodeURIComponent(id) + "/",
        method: "DELETE"
    });
}
```

Some APIs return `204 No Content`; do not require a JSON body if the
endpoint intentionally returns none.

## 28.6 Centralize API configuration

``` javascript
const API_BASE_URL = "http://127.0.0.1:8000/api";

function apiUrl(path) {
    return API_BASE_URL + path;
}
```

For a real application, use environment-specific configuration rather
than scattering URLs throughout scripts.

# 29. XSS and Secure DOM Updates

Cross-Site Scripting (XSS) occurs when attacker-controlled content is
treated as executable content in another user's browser.

Unsafe:

``` javascript
const comment = new URLSearchParams(location.search).get("comment");
$("#comments").html(comment);
```

Safer plain-text insertion:

``` javascript
$("#comments").text(comment ?? "");
```

Or:

``` javascript
const paragraph = $("<p>").text(comment ?? "");
$("#comments").append(paragraph);
```

Do not confuse:

-   Text insertion: treats content as text.
-   HTML sanitization: removes or neutralizes unsafe markup according to
    a policy.
-   Output encoding: depends on the context where data is inserted.
-   URL validation: validates URL schemes and destinations.

If rich HTML is truly required, use a reputable sanitizer configured for
the allowed markup. Do not build a sanitizer from a few regular
expressions.

Avoid blindly putting untrusted values into link `href` attributes. A
URL validator can restrict schemes:

``` javascript
function safeHttpUrl(value) {
    try {
        const url = new URL(value, window.location.origin);

        if (url.protocol !== "http:" && url.protocol !== "https:") {
            return null;
        }

        return url.href;
    } catch {
        return null;
    }
}
```

Do not execute untrusted responses using `dataType: "script"`, `eval()`,
or dynamically executed code.

Client validation is not server validation. Attackers can send requests
without using your page. The backend must validate data and enforce
authorization.

# 30. Performance and Best Practices

## 30.1 Cache repeated selections

Instead of:

``` javascript
$("#message").text("Loading");
$("#message").addClass("loading");
$("#message").show();
```

Use:

``` javascript
const message = $("#message");

message
    .text("Loading")
    .addClass("loading")
    .show();
```

## 30.2 Delegate dynamic events

``` javascript
$("#list").on("click", ".delete", function () {
    $(this).closest("li").remove();
});
```

## 30.3 Limit DOM updates

Create elements and insert them in a controlled manner:

``` javascript
const list = $("#products");
const fragment = $(document.createDocumentFragment());

products.forEach(function (product) {
    fragment.append($("<li>").text(product.name));
});

list.empty().append(fragment);
```

## 30.4 Prevent duplicate event handlers

If initialization can run more than once:

``` javascript
$("#save")
    .off("click.editor")
    .on("click.editor", saveHandler);
```

## 30.5 Debounce search

Without debouncing, each keystroke can send a request.

``` javascript
function debounce(callback, delay) {
    let timerId;

    return function (...args) {
        clearTimeout(timerId);

        timerId = setTimeout(() => {
            callback.apply(this, args);
        }, delay);
    };
}

const searchProducts = debounce(function (query) {
    $.getJSON("/api/products/", { search: query })
        .done(renderProducts)
        .fail(function (xhr) {
            console.error(xhr.status);
        });
}, 300);

$("#search").on("input", function () {
    searchProducts($(this).val());
});
```

## 30.6 Avoid stale search responses

A slower old request can arrive after a newer request. Abort the old
request:

``` javascript
let currentRequest = null;

const searchProducts = debounce(function (query) {
    if (currentRequest) {
        currentRequest.abort();
    }

    currentRequest = $.ajax({
        url: "/api/products/",
        method: "GET",
        dataType: "json",
        data: { search: query }
    })
    .done(renderProducts)
    .fail(function (xhr, status) {
        if (status !== "abort") {
            console.error(xhr.status);
        }
    });
}, 300);
```

Separate HTML structure, CSS, event handling, API communication,
rendering, and validation. Avoid one enormous callback that does
everything.

# 31. Debugging

## `$ is not defined`

Possible causes:

-   jQuery did not load.
-   The script path is wrong.
-   Your application script loaded before jQuery.
-   The CDN request failed.

Inspect the Network tab and script order.

## Selector matches nothing

``` javascript
console.log($("#username").length);
```

Check spelling, timing, document context, and whether the element
actually exists.

## Event does not run

Check whether the selector matched, the handler was registered, the
event name is correct, another element covers the control, or the
element was inserted dynamically after direct binding. Use delegation
for future descendants.

## AJAX fails

Inspect:

1.  Browser Console.
2.  Network tab.
3.  Request URL.
4.  HTTP method.
5.  Request headers.
6.  Request payload.
7.  Response status.
8.  Response body.
9.  Backend logs.
10. CORS settings.

``` javascript
$.ajax({
    url: "/api/products/",
    method: "GET"
}).fail(function (xhr, status, error) {
    console.table({
        httpStatus: xhr.status,
        status: status,
        error: error,
        response: xhr.responseText
    });
});
```

## JSON parser error

Possible causes include HTML instead of JSON, malformed JSON, a login
page, an error page, an empty body when JSON was expected, or incorrect
`dataType`. Inspect the raw response in the Network tab.

## CORS error

Check exact frontend origin, preflight `OPTIONS`, allowed methods,
allowed headers, credentials configuration, and whether error responses
also include the required CORS headers. Do not disable browser security
to work around CORS.

# 32. jQuery with FastAPI and Django REST Framework

jQuery can be the frontend for FastAPI or DRF. The browser only needs a
correctly implemented HTTP endpoint.

## 32.1 FastAPI example

``` python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class ProductCreate(BaseModel):
    name: str
    price: float

products = []

@app.get("/api/products/")
def list_products():
    return products

@app.post("/api/products/", status_code=201)
def create_product(product: ProductCreate):
    saved = {
        "id": len(products) + 1,
        "name": product.name,
        "price": product.price,
    }

    products.append(saved)
    return saved
```

GET:

``` javascript
$.ajax({
    url: "http://127.0.0.1:8000/api/products/",
    method: "GET",
    dataType: "json"
}).done(function (products) {
    console.log(products);
});
```

POST:

``` javascript
$.ajax({
    url: "http://127.0.0.1:8000/api/products/",
    method: "POST",
    contentType: "application/json",
    dataType: "json",
    data: JSON.stringify({
        name: "Keyboard",
        price: 25.5
    })
}).done(function (product) {
    console.log("Created:", product);
});
```

For cross-origin development, configure FastAPI CORS middleware to allow
the exact frontend origin and required methods and headers.

## 32.2 Django REST Framework

A JSON POST:

``` javascript
$.ajax({
    url: "http://127.0.0.1:8000/api/products/",
    method: "POST",
    contentType: "application/json",
    dataType: "json",
    data: JSON.stringify({
        name: "Mouse",
        price: "12.50"
    })
});
```

JWT:

``` javascript
$.ajax({
    url: "http://127.0.0.1:8000/api/products/",
    method: "GET",
    headers: {
        Authorization: "Bearer " + accessToken
    },
    dataType: "json"
});
```

For session authentication, follow Django's CSRF requirements for unsafe
methods.

Debug both sides: browser Network tab and Console on the frontend;
server logs, endpoint execution, validation, permissions, and database
errors on the backend.

# 33. jQuery Compared with Modern JavaScript

  Task           jQuery                Native JavaScript
  -------------- --------------------- -----------------------------------
  Select         `$("#x")`             `document.querySelector("#x")`
  Select all     `$(".x")`             `document.querySelectorAll(".x")`
  Text getter    `$("#x").text()`      `el.textContent`
  Text setter    `.text("Hi")`         `el.textContent = "Hi"`
  HTML getter    `.html()`             `el.innerHTML`
  Value getter   `.val()`              `el.value`
  Add class      `.addClass("a")`      `el.classList.add("a")`
  Remove class   `.removeClass("a")`   `el.classList.remove("a")`
  Toggle class   `.toggleClass("a")`   `el.classList.toggle("a")`
  Click          `.on("click", fn)`    `.addEventListener("click", fn)`
  Remove node    `.remove()`           `el.remove()`
  HTTP           `$.ajax(...)`         `fetch(...)`

Native event example:

``` javascript
const button = document.querySelector("#save");
const message = document.querySelector("#message");

button.addEventListener("click", function () {
    message.textContent = "Saved";
});
```

Native fetch:

``` javascript
fetch("/api/products/")
    .then(function (response) {
        if (!response.ok) {
            throw new Error("HTTP " + response.status);
        }

        return response.json();
    })
    .then(function (products) {
        console.log(products);
    })
    .catch(function (error) {
        console.error(error);
    });
```

# 34. Practical Projects

## Project 1: Interactive task list

HTML:

``` html
<form id="task-form">
    <input id="task-input" name="task" required>
    <button type="submit">Add task</button>
</form>
<ul id="task-list"></ul>

<script src="https://code.jquery.com/jquery-3.7.1.min.js"></script>
<script src="app.js"></script>
```

CSS:

``` css
.completed {
    text-decoration: line-through;
    opacity: 0.6;
}
```

JavaScript:

``` javascript
$(function () {
    $("#task-form").on("submit", function (event) {
        event.preventDefault();

        const input = $("#task-input");
        const taskText = input.val().trim();

        if (!taskText) return;

        const item = $("<li>");
        const label = $("<span>").text(taskText);
        const complete = $("<button>", {
            type: "button",
            class: "complete-task",
            text: "Complete"
        });
        const remove = $("<button>", {
            type: "button",
            class: "delete-task",
            text: "Delete"
        });

        item.append(label, " ", complete, " ", remove);
        $("#task-list").append(item);

        input.val("").trigger("focus");
    });

    $("#task-list").on("click", ".complete-task", function () {
        $(this).closest("li").toggleClass("completed");
    });

    $("#task-list").on("click", ".delete-task", function () {
        $(this).closest("li").remove();
    });
});
```

Concepts: ready, submit, `preventDefault`, `.val`, `.trim`, dynamic
elements, `.text`, `.append`, delegation, `.closest`, `.toggleClass`,
`.remove`.

## Project 2: Search products through an API

HTML:

``` html
<input type="search" id="search" placeholder="Search products">
<p id="status"></p>
<ul id="products"></ul>
```

JavaScript:

``` javascript
$(function () {
    let request = null;

    const search = debounce(function (query) {
        if (request) request.abort();

        $("#status").text("Searching...");

        request = $.ajax({
            url: "http://127.0.0.1:8000/api/products/",
            method: "GET",
            dataType: "json",
            data: { search: query }
        })
        .done(function (response) {
            const products = Array.isArray(response)
                ? response
                : response.results;

            renderProducts(products || []);
            $("#status").text(products.length + " result(s)");
        })
        .fail(function (xhr, status) {
            if (status !== "abort") {
                $("#status").text("Search failed");
                console.error(xhr.status, xhr.responseText);
            }
        });
    }, 300);

    $("#search").on("input", function () {
        search($(this).val().trim());
    });

    function renderProducts(products) {
        const list = $("#products").empty();

        products.forEach(function (product) {
            list.append(
                $("<li>").text(product.name + " — " + product.price)
            );
        });
    }

    function debounce(callback, delay) {
        let timer;

        return function (...args) {
            clearTimeout(timer);
            timer = setTimeout(() => callback.apply(this, args), delay);
        };
    }
});
```

Adapt the URL and response parsing to your API's actual schema.

## Project 3: Create a product

HTML:

``` html
<form id="product-form">
    <input name="name" id="name" placeholder="Product name" required>
    <input name="price" id="price" type="number" min="0" step="0.01" required>
    <button id="submit" type="submit">Create product</button>
</form>
<p id="message"></p>
```

JavaScript:

``` javascript
$(function () {
    $("#product-form").on("submit", function (event) {
        event.preventDefault();

        const form = this;

        if (!form.checkValidity()) {
            form.reportValidity();
            return;
        }

        const product = {
            name: $("#name").val().trim(),
            price: Number($("#price").val())
        };

        $("#submit").prop("disabled", true);
        $("#message").text("Creating product...");

        $.ajax({
            url: "http://127.0.0.1:8000/api/products/",
            method: "POST",
            contentType: "application/json",
            dataType: "json",
            data: JSON.stringify(product)
        })
        .done(function (created) {
            $("#message").text("Created product: " + created.name);
            form.reset();
        })
        .fail(function (xhr) {
            $("#message").text("Could not create product");
            console.error(xhr.status, xhr.responseText);
        })
        .always(function () {
            $("#submit").prop("disabled", false);
        });
    });
});
```

The server must validate the product again.

## Project 4: CV upload

Use the FormData pattern in Section 21 and connect it to an endpoint
accepting multipart data. Test with a small allowed file and confirm
backend upload limits.

# 35. Common Mistakes

**1. Loading application code before jQuery**

Wrong:

``` html
<script src="app.js"></script>
<script src="jquery.js"></script>
```

Correct:

``` html
<script src="jquery.js"></script>
<script src="app.js"></script>
```

**2. Using `.text()` to set an input value**

Wrong: `$("#username").text("Charles");`

Correct: `$("#username").val("Charles");`

**3. Using `.val()` on a paragraph**

Wrong: `$("#message").val("Hello");`

Correct: `$("#message").text("Hello");`

**4. Sending an object as JSON without stringifying**

Wrong:

``` javascript
$.ajax({
    url: "/api/products/",
    method: "POST",
    contentType: "application/json",
    data: { name: "Keyboard" }
});
```

Correct:

``` javascript
$.ajax({
    url: "/api/products/",
    method: "POST",
    contentType: "application/json",
    data: JSON.stringify({ name: "Keyboard" })
});
```

**5. Confusing contentType and dataType**

`contentType` describes what you send; `dataType` describes what you
expect back.

**6. Expecting direct handlers to handle future elements**

Use delegated events for dynamically inserted descendants.

**7. Inserting untrusted values with `.html()`**

Use `.text()` for plain text. Sanitize rich HTML with a reputable
sanitizer.

**8. Assuming every AJAX failure is a backend bug**

Inspect URL, Network tab, CORS, method, headers, body, status, response,
and backend logs.

**9. Assuming 200 always means business success**

Inspect the response body and design APIs with meaningful HTTP status
codes.

**10. Using arrow functions when you need jQuery's callback `this`**

Use a normal function callback.

# 36. Quick Reference

## Selectors

``` javascript
$("#id")
$(".class")
$("p")
$("div p")
$("ul > li")
$("input[name='email']")
```

## Filtering

``` javascript
$(".item").first()
$(".item").last()
$(".item").eq(2)
$(".item").filter(".active")
$(".item").not(".active")
$(".item").is(".active")
```

## Traversal

``` javascript
$(this).parent()
$(this).parents()
$(this).children()
$(this).find("li")
$(this).siblings()
$(this).next()
$(this).prev()
$(this).closest("form")
```

## Content

``` javascript
$("#x").text()
$("#x").text("Hello")
$("#x").html()
$("#input").val()
$("#input").val("Hello")
```

## Attributes and properties

``` javascript
$("#x").attr("title")
$("#x").attr("title", "Tooltip")
$("#x").removeAttr("title")
$("#x").prop("disabled", true)
$("#x").prop("checked")
$("#x").data("user-id")
```

## Classes and styles

``` javascript
$("#x").addClass("active")
$("#x").removeClass("active")
$("#x").toggleClass("active")
$("#x").hasClass("active")
$("#x").css("color")
$("#x").css("color", "red")
```

## DOM

``` javascript
$("#list").append("<li>New</li>")
$("#list").prepend("<li>First</li>")
$("#x").before("<p>Before</p>")
$("#x").after("<p>After</p>")
$("#x").empty()
$("#x").remove()
$("#x").detach()
$("#x").clone()
```

## Events

``` javascript
$("#x").on("click", handler)
$("#x").one("click", handler)
$("#x").off("click")
$("#container").on("click", ".child", handler)
event.preventDefault()
event.stopPropagation()
```

## Forms

``` javascript
$("#input").val()
$("#form").serialize()
$("#form").serializeArray()
$("#checkbox").prop("checked")
$("input[name='role']:checked").val()
```

## Effects

``` javascript
$("#x").show()
$("#x").hide()
$("#x").toggle()
$("#x").fadeIn()
$("#x").fadeOut()
$("#x").slideDown()
$("#x").slideUp()
$("#x").animate({ opacity: 0.5 }, 500)
$("#x").stop()
```

## AJAX

``` javascript
$.ajax({
    url: "/api/items/",
    method: "GET",
    dataType: "json"
})
.done(function (data) {
    console.log(data);
})
.fail(function (xhr) {
    console.error(xhr.status, xhr.responseText);
});
```

## Utilities

``` javascript
$.each(array, callback)
$.map(array, callback)
$.grep(array, callback)
$.extend({}, defaults, options)
$.param(object)
$.contains(parent, child)
$.escapeSelector(value)
```

# 37. Practice Exercises

Try each exercise before looking up a solution.

## Beginner

1.  Select an element by ID and change its text.
2.  Select all paragraphs and give them a class.
3.  Select every second list item.
4.  Read an input value.
5.  Change a button's disabled property.
6.  Toggle a class when a button is clicked.
7.  Hide and show a panel.
8.  Read a selected radio value.
9.  Add a list item.
10. Remove a selected list item.

## Intermediate

11. Use `.closest()` to find a table row from a button.
12. Use `.find()` to locate inputs inside a form.
13. Use `.filter()` to select elements with a class.
14. Prevent normal form submission and handle it with JavaScript.
15. Use `.serialize()` to inspect form data.
16. Add delegated handlers for dynamic buttons.
17. Use `.one()` for a one-time handler.
18. Add an event namespace and remove only that handler.
19. Create a plugin that adds a class.
20. Build a task list with complete and delete controls.

## AJAX

21. Request JSON using `$.getJSON()`.
22. Request a paginated API and render its first page.
23. Search an API using query parameters.
24. Create a resource using JSON POST.
25. Partially update a resource using PATCH.
26. Delete a resource using DELETE.
27. Display a loading indicator and remove it in `.always()`.
28. Handle 401, 403, 404, 422, and 500 differently.
29. Upload a file using FormData.
30. Add a timeout and inspect timeout errors.

## Advanced

31. Build a debounced search field.
32. Abort the previous request when a new search starts.
33. Prevent stale results from replacing newer results.
34. Build a reusable API request helper.
35. Write a plugin with configurable options.
36. Render nested API data safely.
37. Add JWT authorization headers.
38. Configure a backend for a specific frontend origin.
39. Implement a CSRF-aware cookie-authenticated POST.
40. Refactor a large script into rendering, events, and API functions.

# 38. Learning Roadmap

## Stage 1 --- Core foundation

Master `$`, document ready, selectors, `.text()`, `.html()`, `.val()`,
`.attr()`, `.prop()`, and class methods.

**Build:** A page where buttons change headings, messages, and form
values.

## Stage 2 --- DOM and events

Master traversal, insertion/removal, `.on()`, `.off()`, `.one()`,
bubbling, delegation, form submission, and validation.

**Build:** A task list or shopping cart interface.

## Stage 3 --- AJAX

Master HTTP methods, URLs, query parameters, headers, request bodies,
`dataType`, `contentType`, `$.ajax()`, helpers, callbacks, JSON,
FormData, and status codes.

**Build:** A frontend that consumes your own FastAPI or Django REST API.

## Stage 4 --- Advanced browser integration

Master CORS, JWT headers, cookies, CSRF, debouncing, request abortion,
race conditions, plugins, and safe DOM rendering.

**Build:** A searchable product catalogue with CRUD, authentication,
pagination, and file upload.

## Stage 5 --- Read and modernize existing code

Learn the native JavaScript equivalent of each jQuery feature. This
helps you maintain older projects and choose modern APIs when jQuery is
unnecessary.

------------------------------------------------------------------------

# Final Advice

The most important part of jQuery is not memorizing hundreds of methods.
Understand the relationship between:

1.  **DOM:** the document and its elements.
2.  **Selectors:** how you find elements.
3.  **Traversal:** how you move through the document tree.
4.  **Manipulation:** how you read and change the page.
5.  **Events:** how the page responds to user actions.
6.  **AJAX:** how the browser communicates with a backend.
7.  **HTTP:** how methods, headers, status codes, and bodies work.
8.  **Security:** how to handle untrusted data, authentication, CORS,
    and CSRF.

For API learning, the most valuable exercise is to connect a jQuery page
to your own FastAPI or Django REST API. Build in stages: list data,
retrieve one record, create, update, delete, search, paginate,
authenticate, and upload a file. Inspect every request in the browser
Network tab and compare it with backend logs.

**Remember:** jQuery simplifies browser-side JavaScript. It does not
replace the DOM, HTTP, or the backend's responsibility for validation,
authentication, authorization, and security.
