# RxJS

A library that offers mainly:
- Observables: producers of asynchronous values.
- Observers: ways of receiving the data from Observables.

## Simple example

### Creating an Observable from scratch and outputting a single value

**Note**: Observables are cold, they won't actually execute until they are subscribed to.

```typescript
let myData = new Observable<string>(observer => {
    // Do something
    observer.next('resulting value');
    observer.complete();
    // On error
    observer.error(error);
    observer.complete();
});
```

### Subscribing to an Observable's values

```typescript
let subscription = myData.subscribe(data => {
    console.log('received data', data);
}, error => {
    console.error(error);
}, () => {
    console.log('completed');
});
```

### Unsubscribing from an Observable

```typescript
subscription.unsubscribe();
```

## Subjects

**Subject**s are multicast Observables that emit a single value to multiple Observers.  
They are both Observables and Observers.

```typescript
let myData = new Subject<string>();
myData.subscribe(console.log);
myData.next('a value');
myData.next('another value');
```

A Subject is an easy way to create an Observable for multiple values generated somewhere else on the app (instead of running only when the `subscribe` method is called).  
This also means Subjects are good ways of creating long-lived Observables to take advantage of more advanced operators like `debounce`.

