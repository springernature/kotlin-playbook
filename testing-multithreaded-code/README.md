# Testing multithreaded code

Design the code so that the number of threads can be varied independently of the code being tested.  

> E.g. make the code entirely reactive, or pass in a threadpool that it uses to spawn parallel tasks.

Test for properties that are _invariant_ w.r.t. the number of threads.  

> E.g. the behaviour when there are two threads, four threads, 13 threads, etc. should be the same as when there is one thread


Here is an example from the OPA project:

```kotlin
val blitzer = Blitzer(1000, 4)

eventStore.store(Started(a))
val invocationCount = AtomicInteger(0)
try {
    blitzer.blitz {
        invocationCount.getAndIncrement()
        projector.currentState()
        eventStore.store(Incremented(a))
    }
} finally {
    blitzer.shutdown()
}

assertThat(invocationCount.get(), equalTo(1000))
assertThat(projector.currentState().single { it.key == a }.value, equalTo(1001))
```
