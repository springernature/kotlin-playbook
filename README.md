# Kotlin Playbook

How we write (backend) Kotlin code in the Content Acquisition domain of Springer Nature Digital IT.

(A work in progress)

We follow the [standard Kotlin coding conventions](https://kotlinlang.org/docs/reference/coding-conventions.html), although our codebase predates these, and so may occasionaly diverge for historical reasons.

Beyond the standard coding conventions, we have the following rules of thumb:

Organising the codebase:
* [Classes, Methods, and Extension Functions](classes-methods-extension-functions/README.md)
* [Organising code into files](organising-code/README.md)
* [Tests and supporting code](test-code/README.md)

Mostly-Functional Design:
* [Immutable domain models](immutable-domain-models/README.md)
* [Pipelines of transformations](pipelines-of-transformations/README.md)
* [Reporting and handling errors](error-reporting/README.md)
* [Monitoring events, rather than logging](monitoring-events/README.md)

Things we avoid (in production code):
* [Avoid `lateinit`](lateinit/README.md)
* [Avoid the `!!` operator](bang-bang/README.md)
* [Avoid reflection](reflection/README.md)
