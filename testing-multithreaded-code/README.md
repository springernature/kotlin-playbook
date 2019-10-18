# Testing multithreaded code

Design the code so that the number of threads can be varied independently of the code being tested.  

> E.g. make the code entirely reactive, or pass in a threadpool that it uses to spawn parallel tasks.

Test for properties that are [_invariant_](https://en.wikipedia.org/wiki/Invariant_(mathematics)#Invariants_in_computer_science) with respect to the number of threads.  

> E.g. the behaviour when there are two threads, four threads, 13 threads, etc. should be the same as when there is one thread

Be careful that synchronisation in the test does not end up serialising threads through the code being tested.  E.g. use lock-free data structures in the test code, rather than locks or synchronized blocks.

Make sure you see the test of the invariant fail, before making it pass.

## Example

Here is an example that tests that a projection calculated from an event log produces the same result when updated by four threads as it does when updated by a single thread:

```kotlin
import com.natpryce.hamkrest.assertion.assertThat
import com.natpryce.hamkrest.equalTo
import org.jmock.lib.concurrent.Blitzer
import org.junit.Test
import org.junit.runner.RunWith
import org.junit.runners.Parameterized
import org.junit.runners.Parameterized.Parameters
import java.util.concurrent.atomic.AtomicInteger

@RunWith(Parameterized::class)
class InMemoryProjectorStressTest(threadCount: Int) {
    private val eventStore = InMemoryEventStore<EntityKey, TestEvent>()

    data class ProjectionRow(val key: EntityKey, val value: Int) : KeyedRow {
        override val primaryKey = key.toExternalForm()
    }

    val projector = InMemoryProjector(
        eventsFrom = { eventId -> eventStore.fetchAfter(eventId) },
        updateStateFn = { orderedEvent ->
            when (val event = orderedEvent.event) {
                is Started     -> CreateRow(ProjectionRow(event.key, 0))
                is Stopped     -> DeleteRow(event.key.toExternalForm())
                is Incremented -> UpdateRow<ProjectionRow>(event.key.toExternalForm()) { row ->
                    row.copy(value = row.value + 1)
                }
            }
        }
    )

    val a = EntityKey("a")
    val b = EntityKey("b")

    val blitzer = Blitzer(1000, threadCount)

    @Test
    fun `thread-safety stress test`() {
        eventStore.store(Started(a))
        eventStore.store(Started(b))

        val invocationCount = AtomicInteger(0)
        try {
            blitzer.blitz {
                val i = invocationCount.getAndIncrement()
                projector.currentState()
                eventStore.store(Incremented(if (i%2 == 0) a else b))
            }
        } finally {
            blitzer.shutdown()
        }

        assertThat(invocationCount.get(), equalTo(1000))
        assertThat(projector.currentState().single { it.key == a }.value, equalTo(500))
        assertThat(projector.currentState().single { it.key == b }.value, equalTo(500))
    }

    companion object {
        @JvmStatic @Parameters(name = "{0} thread(s)")
        fun threadCounts() = listOf(1, 2, 4, 13)
    }
}
```
