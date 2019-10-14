# We avoid `lateinit` in production code

The `lateinit` modifier tells the type-checker that a `var` property will be initialised "by magic" before it is used, and so does not need to be given a nullable type. It prevents the type checker helping you initialise properties correctly. Instead, programming errors in initialisation are reported by exceptions later at runtime.

It is intended for when Kotlin code is interfacing with legacy Java frameworks that use reflection to poke values into fields behind the back of the type checker and visibility modifiers.

Since we [avoid `var` properties](../immutable-domain-models/README.md) and [avoid reflection](../reflection/README.md), there is no need for `lateinit` in our own code.  

We consider any need of `lateinit` an indicator that our design is flawed, and refactor so that it is unnecessary.
