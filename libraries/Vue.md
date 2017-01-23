# Vue

## Vue instance
```javascript
var vm = new Vue({
    el: '#app',
    data: {
        message: 'Hello Vue!'
    }
});
```

```html
<div id="app">{{ message }}</div>
```

## Properties
Vue automatically re-renders the HTML when a `data` property changes.

Exceptions:  
- `array[index] = value;`
  - Use `Vue.set(array, index, value)` instead
- `array.length = newLength;`
  - Use `array.splice(newLength)` instead

## Methods
Callable from templates, never cache
```javascript
var vm = new Vue({
    ...
    methods: {
        reverseMessage: function() {
            this.message = this.message.split('').reverse().join('');
        },
        now: function() {
            return Date.now();
        }
    }
});
```

## Computed properties
Readable from templates, use caching mechanisms
```javascript
var vm = new Vue({
    ...
    computed: {
        reversedMessage: function() {
            return this.message.split('').reverse().join('');
        }
    }
});
```

You can also define properties with getters and setters
```javascript
var vm = new Vue({
    computed: {
        fullName: {
            get: function() {
                return this.firstName + ' ' + this.lastName;
            },
            set: function(newValue) {
                var names = newValue.split(' ');
                this.firstName = names[0];
                this.lastName = names[names.length - 1];
            }
        }
    }
});
```

## Watched properties
Functions get called whenever watched property changes.
```javascript
var vm = new Vue({
    ...
    watch: {
        firstName: function(val) {
            console.log('Thank you for updating your name');
        }
    }
});
```

### Special properties
```javascript
myInstance.$data     // The model
myInstance.$el       // The HTML element
```

### Lifecycle hooks
```javascript
var vm = new Vue({
    ...
    beforeCreate: function() {},
    created: function() {},
    beforeMount: function() {},
    mounted: function() {},
    beforeUpdate: function() {},
    updated: function() {},
    beforeDestroy: function() {},
    destroyed: function() {}
});
```

<img src="https://vuejs.org/images/lifecycle.png" width="500">

## Directives

### Attribute binding
```html
<div v-bind:title="message">
<div :title="message">        <!-- Equivalent to previous -->
```

### Classes
```html
<div v-bind:class="{ active: isActive }">           <!-- Object with true/false values -->

<div v-bind:class="[activeClass, errorClass]">      <!-- Array of classes -->

<div v-bind:class="[{ active: isActive }, errorClass]">  <!-- Mixed -->
```

### Styles
```html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }">

<div v-bind:style="styleObject">

<div v-bind:style="[baseStyles, overridingStyles]">
```

Vue.js adds vendor prefixes automatically if needed.

### Logic directives

#### If

```html
<div v-if="showThis">
<div v-else-if="showThat">
<div v-else>
```

Or applied to multiple elements:

```html
<template v-if="showThis">
    <h1>Title</h1>
    <p>Paragraph 1</p>
    <p>Paragraph 2</p>
</template>
```

To avoid reusing HTML elements between two branches of logic, use the attribute `key`.
```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>
```

#### Show
Always renders the HTML, but hides it with CSS if false.

```html
<div v-show="showThis">
```


#### For
Iterares through objects in an Array

```html
<li v-for="item in items">

<li v-for="item of items">

<li v-for="(item, index) in items">
```
 
Properties in an Object

```html
<div v-for="value in object">

<div v-for="(value, key) in object">

<div v-for="(value, key, index) in object">
```

Or integers
```html
<span v-for="n in 10">{{ n }}</span>
```

Applied to multiple elements:
```html
<template v-for="item in items">
    <li>
```

The `v-for` directive takes precedence over the `v-if` directive in the same tag.
```html
<li v-for="todo in todos" v-if="!todo.isComplete">
```

To force Vue to keep track of items instead of reusing their contents:
```html
<div v-for="item in items" :key="item.id">
```

### Event directives
```html
<button v-on:click="reverseMessage">

<!-- Alternatively: -->
<button @click="reverseMessage">
```

The handler function will receive the event object:
```javascript
methods: {
    reverseMessage: function(event) {
        this.message = this.message.split('').reverse().join('');
    }
}
```

But you can also pass other arguments and the event explicitly:
```html
<button v-on:click="reverseMessage($event, message)">
```

You can also modifiers:
```html
<!-- $event.stopPropagation() -->
<a v-on:click.stop="doThis"></a>

<!-- $event.preventDefault() -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- modifiers can be chained -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- just the modifier -->
<form v-on:submit.prevent></form>

<!-- use capture mode when adding the event listener -->
<div v-on:click.capture="doThis">...</div>

<!-- only trigger handler if event.target is the element itself -->
<!-- i.e. not from a child element -->
<div v-on:click.self="doThat">...</div>

<!-- the click event will be triggered at most once -->
<a v-on:click.once="doThis"></a>
```

#### Keyboard event modifiers
```html
<input v-on:keyup.13="submit">

<!-- Equivalent to -->
<input v-on:keyup.enter="submit">
```

Available key names:  
- enter
- tab
- delete (captures both “Delete” and “Backspace” keys)
- esc
- space
- up
- down
- left
- right

Add custom key names:
```javascript
Vue.config.keyCodes.f1 = 112;
```

Listen for modifier keys:
```html
<input @keyup.alt.67="clear"> <!-- Alt+c -->
<div @click.ctrl="doSomething">Do something</div>
```

Supported modifier keys:  
- alt
- ctrl
- meta
- shift

### Two-way data binding

#### Textual inputs and textareas

```html
<input v-model="message">
<textarea v-model="message">
```

#### Checkboxes

```html
<input type="checkbox" v-model="checked">
```

Also supports an array model, for multiple checkboxes

```html
<input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
<label for="jack">Jack</label>
<input type="checkbox" id="john" value="John" v-model="checkedNames">
<label for="john">John</label>
<input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
<label for="mike">Mike</label>
```

#### Radio buttons
```html
<input type="radio" id="one" value="One" v-model="picked">
<label for="one">One</label>
<br>
<input type="radio" id="two" value="Two" v-model="picked">
<label for="two">Two</label>
```

#### Selects

Supports single selects and multiple selects (with an array model)

```html
<select v-model="selected">
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
```

You can bind to non-textual values with `v-bind:value`.
```html
<option v-bind:value="{ number: 123 }">123</option>
```

#### Modifiers

| Modifier         | Effect                                       |
| ---------------- | -------------------------------------------- |
| `v-model.lazy`   | Sync after `change` instead of `input`       |
| `v-model.number` | Casts the value to a `Number`                |
| `v-model.trim`   | Trims the value                              |


## Filters
Transform output before rendering on screen. Works in template interpolations and `v-bind` expressions.
```javascript
{{ message | capitalize }}

// Registered by:
new Vue({
    // ...
    filters: {
        capitalize: function(value) {
            if (!value) {
                return '';
            }
            value = value.toString();
            return value.charAt(0).toUpperCase() + value.slice(1);
        }
    }
})
```

## Components
Must be registered before the Vue instance call.

```javascript
Vue.component('todo-item', {
    props: ['todo'],          // Allows <todo-item v-bind:todo="This is a todo">
    template: '<li>This is a todo: {{todo}}</li>'
});
```


