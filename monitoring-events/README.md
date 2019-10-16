# Monitoring events, rather than static loggers

We do not use static loggers and our code does not report activity by formatting and logging human-readable text. 

Instead we define monitoring event types for significant events. An object emits montoring events to a Monitor interface that is passed to its constructor. 

Monitoring events derive from a common supertype that implements consistent serialisation of logged events to name/value pairs and a human-readable summary, which are sent to Kibana.

This is an example of a deliberate [speed bump](../speed-bumps/README.md) to ensure that our applications do not emit verbose and inconsistent logs.

## Example

An example monitoring event type:

```kotlin
sealed class OrderProcessingEvent: MonitoringEvent()
data class OrderPlaced(val orderId: String, amount: Money): OrderProcessingEvent()
...
```

Announcing a monitoring event:

```kotlin
class OrderProcessor(val monitor: Monitor<OrderProcessingEvent>) {
   fun exampleMethod() {
      doSomething()
      monitor.record(OrderPlaced( ... ))
   }
   ...
}
```
