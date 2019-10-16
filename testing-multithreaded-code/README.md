# Testing multithreaded code

Design the code so that the number of threads can be varied independently of the code being tested.  

> E.g. make the code entirely reactive, or pass in a threadpool that it uses to spawn parallel tasks.

Test for properties that are [_invariant_](https://en.wikipedia.org/wiki/Invariant_(mathematics)#Invariants_in_computer_science) with respect to the number of threads.  

> E.g. the behaviour when there are two threads, four threads, 13 threads, etc. should be the same as when there is one thread

Be careful that synchronisation in the test does not end up serialising threads through the code being tested.  E.g. use lock-free data structures in the test code, rather than locks or synchronized blocks.

Make sure you see the test of the invariant fail, before making it pass.

## Example

Here is an example from the OPA project that tests that a projection produces the same result when updated by four threads as it does when updated by a single thread (which is checked by other tests in its test suite):

```kotlin
val blitzer = Blitzer(1000, 4)

eventStore.store(Started(a))
val invocationCount = AtomicInteger(0)
try {
    blitzer.blitz {
        invocationCount.getAndIncrement()
        projection.currentState()
        eventStore.store(Incremented(a))
    }
} finally {
    blitzer.shutdown()
}

assertThat(invocationCount.get(), equalTo(1000))
assertThat(projection.currentState().single { it.key == a }.value, equalTo(1001))
```
