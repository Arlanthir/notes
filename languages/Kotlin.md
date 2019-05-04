# Kotlin

## Types

## Basic Types

`val` is used to declare immutable values, and it's encouraged.  
`var` is used to declare mutable variables, when you must assign a different value later

```kotlin
val myBoolean = true
val myByte: Byte = 0xf
val myShort: Short = 16
val myInt: Int = 32
val myLong: Long = 64L
val myFloat: Float = 32.0F
val myDouble: Double = 64.0
val myString: String = "hello"
var myString2: String
val myString3 = "world"           // Type inference

val s1 = "I'm $myInt years old"               // String variable interpolation
val s2 = "I'm ${calculateAge()} years old"    // String function interpolation
val s3 = """Multiline
String"""
```

## Arrays
```kotlin
val myArray = arrayOf(4, 5, 7, 3, "Chike", false)
val intArray = arrayOf<Int>(4, 5, 7, 3)
val lambdaArray = Array(5, { i -> i * 2 })       // 0, 2, 4, 6, 8
```

## Conditionals and loops
```kotlin
// Like a switch case
val myNum: Int = when (something){
  1 -> 2
  calculateOtherNumber() -> 4
  4 -> 6
  else -> 0
}

// Parameter is optional
myNum = when {
  shouldBeOne() -> 1
  shouldBeTwo() -> 2
  else -> 3
}
```

## Classes

```kotlin
package com.company.app.package

class MyClass(param1: Int, param2: String, val param3: Int): MySuperClass() {
  val myParam1 = param1
  init {
    println("Initializer block that prints ${param2} but doesn't save it")
  }
  // param3 is auto initialized by constructor
  val lazyParam by lazy {
    // Will be initialized the first time it is accessed
    2
  }
  
  override fun inParent(): Int {
  }
  
  private fun privateFun() {
  }
}
```

### Object Expresions

We can create variations of a class without subclassing it.

```kotlin
fadeIn.setAnimationListener(object : Animation.AnimationListener {
    override fun onAnimationStart(animation: Animation) {
        view.alpha = 1f
    }
    override fun onAnimationEnd(animation: Animation) {}
    override fun onAnimationRepeat(animation: Animation) {}
})
```

### Companion object

Used to add static methods to a class.

```kotlin
companion object {
    fun newInstance() = MyClass() // Creates MyClass.newInstance(), equivalent to calling MyClass()
}
```

## Enums
```kotlin
enum class State {
  EXPANDED,
  EXPANDING,
  COLLAPSING,
  COLLAPSED,
  IDLE,
  INIT
}

State.EXPANDED.name // name is available
```


## Nullable Types
```kotlin
val myInstance: MyClass? = returnMyClassOrNullSomehow()
myInstance?.myMethod()
```

To assert your instance and cast it to a non-nullable type:
```kotlin
val myInstance: MyClass = returnMyClassOrNullSomehow()!!
myInstance.myMethod()

// Or more elegantly:

returnMyClassOrNullSomehow().let {
  it.myMethod()
}
```
