# Copying Extension Functions

We define extensions to abstract the use of the builtin `copy` method of data classes where that simplifies code, removes duplication, and/or reduces possibility of coding error.

For example, the following code has an error that's easy to miss: the result of the first call to copy is discarded because second call to `copy` should have refered to `e2.bar`, not `e.bar`
  
```kotlin
fun f(e: Example): Example {
    var e2 = e.copy(bar = e.bar + 30)
    ...
    return e2.copy(bar = e.bar + 20)
}
```
  
We therefore avoid calling copy from outside methods or extension functions, when the call to copy transforms fields of a data class.  Instead we define extension functions that delegate to the `copy` method.

The example above would therefore be written as follows, which makes the error impossible:

```kotlin
fun f(e: Example): Example {
    var e2 = e.plusBar(30)
    ...
    return e2.plusBar(20)
}
```

We follow a naming convention for these "copying extensions":

`withThing`: replaces the value of the `thing` property, or if the property `things` is a collection, replaces it with a singleton collection.
`withoutThings`: replaces the value of a nullable `thing` property with null, or if the property `things` is a collection, replaces it with an emoty collection.
`plusThing`: adds a value to the `thing` property or adds an element to the `things` collection.
`minusThing`: subtracts a value from the `thing` property or removes an element from the `things` collection.
