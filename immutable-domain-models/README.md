# Immutable Domain Models

Our domain models are implemented as data classes.

Data classes MUST be immutable.

E.g. we only use `val` for properties:

```kotlin
data class Article(
    val doi: DOI, 
    val title: String, 
    val authors: List<Author>, 
    val contentRef: URI)
```

And do _not_ use `var` for properties, nor define properties to hold mutable types:

```kotlin

// this is STRONGLY DISCOURAGED
data class Article(
    var doi: DOI, 
    var title: String, 
    val authors: MutableList<Author>, 
    var contentRef: URI)
```

Orchestration of our applications is written in an object-oriented style.  Therefore, the design of each component follows a [Functional Core, Imperative Shell](https://www.destroyallsoftware.com/screencasts/catalog/functional-core-imperative-shell) style.
