# Test and Support Code

We organise the Kotlin source of each module into three directories:

* src/main – contains the production code
* src/test – contains the tests of the production code in src/main
* src/testing – contains code to support testing the production code that is also useful for writing tests of modules that depend on this module.

The Gradle build treats these as separate module "configurations".  This allows Gradle to declare a dependency from the production configuration of a module to the production configuration of another module, and from the test configuration of the module to the testing configuration of the other module.

For example, in the `anura` repository, the lib/stoa-domain module depends on lib/common-domain and lib/underware, and its tests depend on the testing support code from those modules:

```Groovy
compile project(path: ":lib:common-domain")
testingCompile project(path: ":lib:common-domain", configuration: "testingElements")

compile project(path: ":lib:underware")
testCompile project(path: ":lib:underware", configuration: "testingElements")
```
