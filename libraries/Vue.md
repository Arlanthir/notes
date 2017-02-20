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

You can also use modifiers:
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
    props: ['todo'],            // Allows <todo-item v-bind:todo="This is a todo">
    template: '<li>This is a todo: {{todo}}</li>',
    data: function() {
        return {
            ...
        };            
    }
});
```

**Note**: The original `<todo-item>` tag will be replaced with the new `<li>` tag. Classes/attributes in both tags will be merged. This is very different from what happens in Angular.io.

### Rules:  
- Component data must be a function (object would be shared across instances)
- Component's can't mutate props, they should copy them to a local data property first
  - Alternatively, use a computed property

### Props validation
```javascript
Vue.component('example', {
    props: {
        // basic type check (`null` means accept any type)
        propA: Number,
        // multiple possible types
        propB: [String, Number],
        // a required string
        propC: {
            type: String,
            required: true
        },
        // a number with default value
        propD: {
            type: Number,
            default: 100
        },
        // object/array defaults should be returned from a
        // factory function
        propE: {
            type: Object,
            default: function () {
                return { message: 'hello' }
            }
        },
        // custom validator function
        propF: {
            validator: function (value) {
                return value > 10
            }
        }
    }
})
```

### Custom events
```javascript
this.$emit('myevent', eventValue);
```

To listen to a native event with the same name as a custom event:
```html
<my-component v-on:click.native="onClick">
```

#### Support `v-model`
`v-model` is syntactic sugar for `v-bind:value` and `v-on:input`.  
To support it, a custom component just has to register prop `value` and emit  event `input` with new values.

### Component-component interaction (naive)
```javascript
var bus = new Vue();
bus.$emit('someMessage', value);
bus.$on('someMessage', function(value) { ... });
```

### Content Distribution (AKA transclusion)

Child components can define `slot`s for parent components to add content.

#### Single slot

```javascript
Vue.component('my-component', {
    template: '<div>Hello <slot></slot>!</div>'
});
```

```html
<my-component>World</my-component>
```

#### Named slots

```javascript
Vue.component('my-component', {
    template: '<div><slot name="first"></slot> <slot name="second"></slot>!</div>'
});
```

```html
<my-component>
    <span slot="first">Hello</span>
    <span slot="second">World</span>
</my-component>
```

#### Scoped slots

```javascript
Vue.component('my-component', {
    template: '<div class="child"><slot text="hello from child"></slot></div>'
});
```

```html
<div class="parent">
    <my-component>
        <template scope="props">
            <span>hello from parent</span>
            <span>{{ props.text }}</span>
        </template>
    </my-component>
</div>
```

### Dynamic components (simple routing)

```javascript
var vm = new Vue({
    data: {
        currentView: 'home'
    }
});
```

```html
<keep-alive> <!-- Optional, will keep switched-out components in memory to preserve state -->
    <component v-bind:is="currentView">
        <!-- Will instantiate component <home> -->
    </component>
</keep-alive>
```
### Child reference

```html
<my-component ref="comp"></my-component>
```

```javascript
var myComponent = this.$refs.comp;
```

## Custom directives

```javascript
// Register a global custom directive called v-focus
Vue.directive('focus', {
    // When the bound element is inserted into the DOM...
    inserted: function (el) {
        // Focus the element
        el.focus();
    }
});
```

Available hooks:  
- bind: Directive was bound to its element
- inserted: Component was inserted in parent
- update: Component was updated
- componentUpdated: Component and its children were updated
- unbind: Directive was unbound from element

Hook arguments:
- el
- binding  `v-name:arg.modifiers="value"`
  - name
  - value
  - oldValue
  - expression
  - arg
  - modifiers
- vnode
- oldVnode


## Mixins

Allow code reuse across different components.

```javascript
// define a mixin object
var myMixin = {
    created: function () {
        this.hello();
    },
    methods: {
        hello: function () {
            console.log('hello from mixin!');
        }
    }
}:

// define a component that uses this mixin
var Component = Vue.extend({
    mixins: [myMixin]
});

var component = new Component(); // -> "hello from mixin!"
```

Mixin hooks will be called before component hooks.  
Methods, components, directives, etc, will give priority to the component.


## Single file components

Leverage Webpack or Browserify to transform component files in HTML, JavaScript and CSS files.

`mycomponent.vue`
```vue
<template>
    <p>{{ greeting }} World!</p>
</template>

<script>
module.exports = {                // ES2015 alternative: export default {
    data: function() {            // ES2015 alternative: data() {
        return {
            greeting: 'Hello'
        };
    }
};
</script>

<style scoped>
p {
    font-size: 2em;
    text-align: center;
}
</style>
```

# Build tools: vue-cli with Webpack and vue-loader
```bash
npm install -g vue-cli
vue init <template-name> <project-name> # Interesting templates: webpack, webpack-simple
cd <project-name>
npm install
npm run dev          # Development version with Node.js server
npm run build        # Production version in folder /dist
```

## Manual setup
```bash
npm install --save-dev vue-loader vue-template-compiler
```

webpack.config.js:
```javascript
module: {
    rules: [{
        test: /\.vue$/,
        use: 'vue-loader'
    }]
}
```
