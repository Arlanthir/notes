# Kotlin

## Types

## Basic Types
```kotlin
val myString: String = "hello"    // Immutable, encouraged
var myString2: String             // Mutable
val myString3 = "world"           // Type inference
val myByte: Byte = 0xf
val myShort: Short = 16
val myInt: Int = 32
val myLong: Long = 64L
val myFloat: Float = 32.0F
val myDouble: Double = 64.0
```

## Classes

```kotlin
package com.company.app.package

class MyClass(param1: Int, param2: String) {
}
```

### Companion object

Used to add static methods to a class.

```kotlin
companion object {
    fun newInstance() = MyClass() // Creates MyClass.newInstance(), equivalent to calling MyClass()
}
```

### Nullable Types
```kotlin
val myInstance: MyClass? = returnMyClassOrNullSomehow()
myInstance?.myMethod()
```

To assert your instance and cast it to a non-nullable type:
```kotlin
val myInstance: MyClass = returnMyClassOrNullSomehow()!!
myInstance.myMethod()
```
