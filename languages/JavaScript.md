# JavaScript

## Basics

- Designed to add dynamic behavior to web pages
- .js files - code
- Can be included inline in .html files

## Minimal Working Example

hello.html:

```html
<body>
    <script>
        alert("Hello");
    </script>
</body>
```

Alternatively:

```html
<body>
    <script src="scripts.js"></script>
</body>
```

## Input/Output

Show popup dialog: `alert("Hello");`

Print directly to the webpage: `document.write("<p>Hello</p>");`

## Functions

```javascript
function f(a, b) {
    alert(a);
    return b;
}
```
```javascript
// Calls function 'f', setting this to thisArg and passing arguments:
f.apply(thisArg, [arg1, arg2, ...]);
f.call(thisArg, arg1, arg2, ...);
```

## Variable Allocation

### Basic Types

```javascript
var a = 2;
var b = "three";
```

### Arrays

```javascript
var myArray = new Array();
myArray.push("element0");
myArray.push("element1");
myArray.push("element2"); // myArray.length == 2;
var element1 = myArray.pop();
var element0 = myArray.shift();

myArray = ["element0", 1, 2, 3, 4, 5];
myArray.splice(2, 3); // Remove 3 elements starting at index 2
// myArray = ["element0", 1, 5];

myArray.sort(function(a, b) { return a-b; });
```

### Objects

```javascript
var myObject = {property1: "value"};
alert(myObject.property1 + " == " + myObject["property1"]);

for (var prop in myObject) {
    alert("myObject." + prop + " = " + myObject[prop]);
}
```

Cecking if variable is defined:
```javascript
// Works even if the variable is not known and
// if the global property undefined has been changed
if (typeof myVar !== 'undefined')

// Alternatively:
// (can only check properties of objects, use window for global vars)
if (window.myVar)
```

### Simulate Classes

```javascript
function MyClass (par) {
    this.myPar = par;
}

MyClass.prototype.alertPar = function() {
    alert(this.myPar);
};

MyClass.staticMethod = function() {
    // do something
};

var c = new MyClass("hello");
c.alertPar();
```

#### Class Inheritance

```javascript
function Mammal(...) {
    ...
}

function Cat(...) {
    Mammal.call(this, ...);
    ...
}
Cat.prototype = new Mammal();
Cat.prototype.constructor = Cat;       // Reset constructor to subclass
Cat.prototype.eat = function() {
    Mammal.prototype.eat.apply(this, arguments); // Call super
    ... // Extend behavior
}
```

## Revealing Module Pattern

```javascript
var myModule = (function() {
    var _privateProperty = 'Hello World';
    var publicProperty = 'I am a public property';

    function _privateMethod() {
        console.log(_privateProperty);
    }

    function publicMethod() {
        _privateMethod();
    }

    return {
        publicMethod: publicMethod,
        publicProperty: publicProperty
    };
}());
```

## Element Manipulation

```javascript
var logo = document.getElementById("logo");
var images = document.querySelectorAll('.image');
logo.innerHTML = "<img src=\"logo.png\">";
```

## Event Handling

**TODO**: Use `addEventListener`

```html
<a href="#" onclick="return clickHandler(element);">Test Link</a>

<script>
function clickHandler(element) {
    element.innerHTML = "Changed Test Link";
    return false; // Cancel click event
}
</script>
```

## Promises

Async code execution without passing callback function arguments.
```javascript
getDataFromServer()
    .then(function(data) {...})
    .catch(function(error) {...});
```

## Error Handling
```javascript
try {
     throw "noooo";
}
catch(err) {
     console.error(err);
}
```

<!--

## Browser differences
```javascript
var event = event || event.originalEvent || window.event;
var target = event.target || event.srcElement;
```

Enter on text field
```javascript
$("#searchField").on("keypress", fieldKeyPress);
if (e.keyCode == 13) {
     // enter was pressed
}
```

-->

## Useful functions

Random int from X (inclusive) to Y (exclusive)
```javascript
Math.floor(Math.random() * (Y-X) + X)
```

<!--

/* Get the coordinates of a mouse event */
function getCoords(e) {
    if (typeof e.originalEvent !== "undefined") {
        e = e.originalEvent;
    }
    var coords = [];
    if (typeof e.targetTouches !== "undefined" && e.targetTouches.length > 0) {
        var thisTouch = e.targetTouches[0];
        coords[0] = thisTouch.clientX;
        coords[1] = thisTouch.clientY;
    } else if (typeof e.changedTouches !== "undefined" && e.changedTouches.length > 0) {
        var thisTouch = e.changedTouches[0];
        coords[0] = thisTouch.clientX;
        coords[1] = thisTouch.clientY;
    } else {
        coords[0] = e.clientX;
        coords[1] = e.clientY;
    }
    return coords;
}

function isClick(coords1, coords2) {
    return (coords1[0] >= coords2[0] - 20 && coords1[0] <= coords2[0] + 20 &&
    coords1[1] >= coords2[1] - 20 && coords1[1] <= coords2[1] + 20);
}

function isNumber() {
    return !isNaN(parseFloat(n)) && isFinite(n);
}

-->

<!--
// Linear interpolation
function lerp(oldv, newv, ratio) {
     return (newv - oldv) * ratio + oldv;
}

function unlerp(start, finish, value) {
     return (value - start) / (finish - start);
}

Upload files using AJAX: http://cmlenz.github.io/jquery-iframe-transport/
On newer browsers (IE10+), look into FormData

-->

## RESTful APIs

**RE**presentational **S**tate **T**ransfer APIs should use the HTTP methods and URIs

`POST` to /cars/ `{make: 'suzuki', color: 'red'}` - Creates a new red Suzuki car in the DB <br>
`GET` /cars/make/suzuki - Gets the list of Suzuki cars <br>
`PUT` /cars/&lt;id&gt; `{color: 'blue'}` - Changes the car's color to blue <br>
`DELETE` /cars/&lt;id&gt; - Deletes the car <br>

**Note**: `PUT` and `DELETE` should be idempotent, i.e. multiple calls should result in the same DB state as one call. `GET` is nullipotent (or safe), it produces no side-effects. `POST` is non-idempotent.

Redirect on Apache .htaccess:

```
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteRule ^prices/cards/([^/]+)$ prices/card.php?name=$1 [NC,L]
</IfModule>
```

Redirect on nginx:

```
location /prices {
     rewrite ^/prices/cards/([^/]+)$ /prices/card.php?name=$1 last;    # or break if location is the same?
}
```

## Debugging

Print message to console (f12): `console.log(message);`

Insert breakpoint in code: `debugger;`

Useful functions:
```javascript
// Alert errors when on mobile, without console viewer
window.onerror = function(msg, url, line) {
     var err = "ERROR: " + url + " #" + line + " : " + msg;
     console.log(err);
     alert(err);
};

function nowTimestamp() {
     var now = new Date();
     return now.getMinutes() + ":" + now.getSeconds() + ":" + now.getMilliseconds();
}

var DEBUG = true;
function log(args) {
     if (DEBUG && typeof console !== 'undefined') {
         console.log.call(console, arguments);
    }
}
```

## Bookmarklets
Should be in the form:

```html
<a href="javascript:(function(){alert('hello');})();">Hello</a>
```
