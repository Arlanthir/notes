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
```html
<div v-bind:title="message">
<div :title="message">                              <!-- Equivalent to previous -->

<div v-bind:class="{ active: isActive }">           <!-- Object with true/false values -->
<div v-bind:class="[activeClass, errorClass]">      <!-- Array of classes -->
<div v-bind:class="[{ active: isActive }, errorClass]">  <!-- Mixed -->

<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }">   <!-- Vue adds vendor prefixes -->
<div v-bind:style="styleObject">
<div v-bind:style="[baseStyles, overridingStyles]">


<div v-if="showThis">
<div v-for="todo in todos">

<button v-on:click="reverseMessage">
<button @click="reverseMessage">                    <!-- Equivalent to previous -->
<form v-on:submit.prevent="onSubmit">

<input v-model="message">                           <!-- Data binding -->
```

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

## Vue component
Must be registered before the Vue instance call.

```javascript
Vue.component('todo-item', {
    props: ['todo'],          // Allows <todo-item v-bind:todo="This is a todo">
    template: '<li>This is a todo: {{todo}}</li>'
});
```


