# Test and Support Code

We organise the Kotlin source of each module into three source trees (what Gradle calls a "configuration"):

* src/main – contains the production code
* src/test – contains the tests of the production code in src/main
* src/testing – contains code to support testing the production code that is also useful for writing tests of modules that depend on this module.

