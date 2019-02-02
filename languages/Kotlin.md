# Kotlin

## Types

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

## Classes

### Companion object

Used to add static methods to a class.

```kotlin
companion object {
    fun newInstance() = MyClass() // Creates MyClass.newInstance(), equivalent to calling MyClass()
}
```
