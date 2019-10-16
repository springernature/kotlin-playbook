# Speed Bumps

![UK road sign for speed bumps](uk-road-signs-57752.jpg)

We deliberately make it difficult to implement things that we don't want too much of in our codebase, so that we have to consider whether the effort is worth it.

For example, to ensure that our applications do not emit verbose and inconsistent logs, we do not use static loggers and log human-readable text.  Instead we define monitoring event types, and have to pass a Monitor interface to code that wants to emit monitoring events.
