# Speed Bumps

![UK road sign for speed bumps](speed-bump.jpg)

We deliberately make it difficult to implement things that we don't want too much of in our codebase, so that we have to consider whether the effort is worth it.

For example, to ensure that our applications do not emit verbose and inconsistent logs, our code [announces monitoring events to a monitor interface passed to its constructor, rather than write human-readable text to a static logger](monitoring-events/README.md).
