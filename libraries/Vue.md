# Vue

*Note: This document is a work in progress, some syntax may be left over from Vue2.*

## Vue application
```javascript
const app = Vue.createApp({
    data() {
        return {
            message: 'Hello Vue!'
        }
    }
});

app.mount('#app');
```

```html
<div id="app">{{ message }}</div>
```

## Properties
Vue automatically re-renders the HTML when a `data` property changes.

Data must be a **plain JavaScript object** (recursively).

To mark a data property as immutable and gain a performance boost (specially memory-wise), use `Object.freeze(...)`.
To modify a frozen object, copy it:
```javascript
let newFrozenObject = Object.freeze(Object.assign({}, oldFrozenObject, changedFields));
```

Exceptions:  
- `object.index = value;`
  `array[index] = value;`
  - Use `Vue.set(object, index, value)` instead
- `array.length = newLength;`
  - Use `array.splice(newLength)` instead

## Methods
Callable from templates, never cache
```javascript
const app = Vue.createApp({
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
const app = Vue.createApp({
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
const app = Vue.createApp({
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
const app = Vue.createApp({
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
const app = Vue.createApp({
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

<!-- <img src="https://vuejs.org/images/lifecycle.png" width="500"> -->
<img src="https://v3.vuejs.org/images/lifecycle.svg" width="500">


## Directives

### Binding

`v-bind:` or its syntax sugar `:` offer one-way data binding.

For two-way data binding, check `v-model:`.


#### Attribute binding
```html
<div v-bind:title="message">
<div :title="message">        <!-- Equivalent to previous -->
```

#### Classes
```html
<div v-bind:class="{ active: isActive }">           <!-- Object with true/false values -->

<div v-bind:class="[activeClass, errorClass]">      <!-- Array of classes -->

<div v-bind:class="[{ active: isActive }, errorClass]">  <!-- Mixed -->
```

#### Styles
```html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }">

<div v-bind:style="styleObject">

<div v-bind:style="[baseStyles, overridingStyles]">
```

Vue.js adds vendor prefixes automatically if needed.

### Logic directives

#### If

```html
<div v-if="showThis"></div>
<div v-else-if="showThat"></div>
<div v-else></div>
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
- <letters> (like `a`, handles both `a` and `A`)

Listen for modifier keys:
```html
<input @keyup.alt.c="clear"> <!-- Alt+c -->
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
const app = Vue.createApp({
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
app.component('todo-item', {
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
app.component('example', {
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

`<input v-model="searchText" />` is syntax sugar for `<input :value="searchText" @input="searchText = $event.target.value" />`.

On components, it's syntax sugar for `<my-component :model-value="searchText" @update:model-value="searchText = $event"></my-component>`.

To support it, a custom component just has to register prop `modelValue` and emit event `update:modelValue` with new values.

```vue
<script setup lang="ts">
const props = withDefaults(defineProps<{
        modelValue?: number
    }>(), {
        modelValue: 0
    }
);

const emit = defineEmits<{
  (e: 'update:modelValue', value: number): void
}>();
</script>

<template>
    <button @click="$emit('update:modelValue', 20)">Change to 20</button>
</template>
```

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
app.component('my-component', {
    template: '<div>Hello <slot></slot>!</div>'
});
```

```html
<my-component>World</my-component>
```

#### Named slots

```javascript
app.component('my-component', {
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
app.component('my-component', {
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
const app = Vue.createApp({
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
app.directive('focus', {
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

<style lang="scss" scoped>
p {
    font-size: 2em;
    text-align: center;
}
</style>
```

# Build tools / Scaffolding

## Vite
```bash
npm init vite@latest
cd <project-name>
npm install
npm run dev        # Run development version
npm run build      # Build production version
```

## Vue CLI
```bash
npm install -g @vue/cli
vue create <project-name>
cd <project-name>
npm run serve       # Run development version
npm run lint        # Lint code
npm run build       # Build production version
npm run test:unit   # Run unit tests
```


# Vue-router

## Setup
```javascript
import Vue from 'vue';
import VueRouter from 'vue-router';

import AppComponent from './app-component.vue';
import HelloComponent from './hello-component.vue';
import ByeComponent from './bye-component.vue';

Vue.use(VueRouter);

Vue.component('hello-component', HelloComponent);
Vue.component('bye-component', ByeComponent);

const router = new VueRouter({
    // HTML5 mode
    mode: 'history',
    routes: [
        // Param thing will be in $route.params.thing
        { path: '/hello/:thing', component: HelloComponent },
        { path: '/bye', component: ByeComponent, alias: '/adios' },
        { path: '/', redirect: '/hello/world' },
        { path: '*', component: NotFoundComponent }
    ]
})

let vm = new Vue({
    router,
    el: '#app',
    // ...
});
```

To get notified of changes in params, a component can watch the $route property.

## Linking in templates
```html
<!-- Link -->
<router-link to="/hello/world">Hello</router-link>
<!-- Link without pushing history -->
<router-link :to="/bye">Bye</router-link>

<router-view></router-view>
```

## Linking in code
```javascript
router.push('hello/world');
router.push({path: 'home', params: {thing: 'world'}});
// Link without pushing history
router.replace(...);
```

## Nested routes
```javascript
routes: [
    { path: '/page', component: PageComponent,
        children: [
            { path: '', component: HomeSubpageComponent }
            { path: 'subpage', component: SubpageComponent }
        ]
    }
]
```

## Decoupling route params into component props
```javascript
// Passes the route params as props
{ path: '/hello/:thing', component: HelloComponent, props: true },
// Passes static props
{ path: '/hello', component: HelloComponent, props: { myParam: 'World' } }
```

# State management: vuex

Provides a single data store for components to use.
Enforces rules for data mutation and offers a state tree, as well as debugging features.

## Setup
```javascript
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

// Define global application state
const store = new Vuex.Store({
    state: {
        count: 0
    },
    getters: {
        countPreview(state, getters) {
          return state.count + 1;
        }
    },
    mutations: {
        increment(state, n = 1) {
            state.count += n;
        }
    },
    actions: {
        // Asynchronous mutations
        incrementAsync(context, n = 1) {
            setTimeout(() => {
                context.commit('increment', n);
            }, 1000);
        }
    }
});

let vm = new Vue({
    store,
    el: '#app',
    // ...
});
```

## Display state
```javascript
template: `<div>{{ count }}</div>`,
computed: {
    localComputed() { /* ... */ },
    count () {
        return this.$store.state.count;
    }
}

// Alternatively:
template: `<div>{{ count }}</div>`,
computed: {
    localComputed() { /* ... */ },
    ...mapState(['count'])
    // ...mapGetters ...mapMutations
}
```

## Change state
```javascript
this.$store.commit('increment');
this.$store.dispatch('increment', 2);
```

## Modules
```javascript
const store = new Vuex.Store({
    // ...
    modules: {
        counter: {
            state: {
                count: 0
            },
            // ...
        }
    }
});
```

# Vue Resource (AJAX)

## Setup

```javascript
import Vue from 'vue';
import VueResource from 'vue-resource';

Vue.use(VueResource);

let vm = new Vue({
    http: {
        root: '/api'
    },
    el: '#app',
    // ...
});
```

## Sending requests

```javascript
// Use Vue.http or this.$http
this.$http.post('/someUrl', {foo: 'bar'}).then(response => {
    // Example properties:
    response.status;
    response.statusText;
    response.headers.get('Expires');
    response.body;
    response.json();
}, response => {
    // Error callback
});
```

## Resources
```javascript
var customActions = {
    foo: {method: 'GET', url: 'someItem/foo{/id}'},
    bar: {method: 'POST', url: 'someItem/bar{/id}'}
}

var resource = this.$resource('someItem{/id}', {}, customActions);

// GET someItem/foo/1
resource.foo({id: 1}).then(response => {
    this.item = response.body;
});

// POST someItem/bar/1
resource.bar({id: 1}, {item: this.item}).then(response => {
    // success callback
}, response => {
    // error callback
});

```
