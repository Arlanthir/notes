# Vue

## Vue instance
```javascript
var app = new Vue({
    el: '#app',
    data: {
        message: 'Hello Vue!'
    },
    methods: {
        reverseMessage: function () {
            this.message = this.message.split('').reverse().join('')
        }
    }
})
```

## Vue component
```javascript
Vue.component('todo-item', {
    props: ['todo'], // Allows <todo-item v-bind:todo="This is a todo">
    template: '<li>This is a todo: {{todo}}</li>'
})
```

## Directives
```html
<div v-bind:title="message">
<div v-if="showThis">
<div v-for="todo in todos">
<button type="button" v-on:click="reverseMessage">
<input v-model="message">
```
