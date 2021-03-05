# Groovy

## Basics

- Based in Java but with modern features.
- Allows scripts without classes (by transforming them into classes under the hood).
- Used in the *Configuration as Code* paradigm in tools such as Jenkins and Nexus.

## Minimal Working Example

script.groovy:
```groovy
println 'Hello World!'
```

## Conditionals & Loops

### each

Iterates over a Collection, keeping the current element in the implicit parameter `it`:

```groovy
list.each({
    println it
})
```

Equivalent to the shortcut:

```groovy
list.each {
    println it
}
```

To use the index as well:

```groovy
list.eachWithIndex { it, i ->
    println "$i: $it"
}
```

### Elvis operator

Shortcut (`a ?: b`) for the Ternary Operator recipe `a ? a : b`.

```groovy
def result = name ?: "Unknown"
// Shortcut for: def result = name != null ? name : "Unknown"
```

## Variables

Variables (and functions) can be explicitly typed (encouraged) or inferred (convenient).

```groovy
int i = 1
def j = 1
def stringNoInterpolation = 'Hello'
def stringWithInterpolation = "$stringNoInterpolation World!"
```

## Functions

```groovy
def hello() {
    println('Hello World!')
}
```

You can omit parentheses of function calls:
```groovy
println 'Hello'
sum a, b
```

You can omit the return keyword:

```groovy
int sum(int a, int b) {
    a + b
}
```

### Closures

A closure in Groovy is an open, anonymous, block of code that can take arguments, return a value and be assigned to a variable.

```
def sum = { a, b -> a + b }
```

When no parameters are explicitly defined (using `->`), a closure always defines one implicit parameter called `it`:

```groovy
def greeting = { "Hello, $it!" }
```


## Input/Output

### Console I/O

```groovy
println("Hello World")
```

## Best practices / idioms

https://groovy-lang.org/style-guide.html
