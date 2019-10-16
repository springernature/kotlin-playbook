# Monitoring events, rather than static loggers

To ensure that our applications do not emit verbose and inconsistent logs, we do not use static loggers and log human-readable text. Instead we define monitoring event types that have a common supertype that implements consistent serialisation of logged events, and have to pass a Monitor interface to code that wants to emit monitoring events.

This is an example of a deliberate [speed bump](../speed-bumps/README.md)
