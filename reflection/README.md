# We avoid Reflection in production code

Reflection makes code harder to learn and understand. It interferes with IDE navigation, auto-complete, and automatic refactoring. It prevents type-level modelling and using the type checker to catch errors at edit or compile time.

Therefore, we avoid reflection where possible in production code. We don't use reflection in business logic. We avoid reflection as the interface between separate parts of the code. E.g. we don't use reflection to generate output from one part of the code that is input to another part of the code.

We allow reflection in unit tests, but actually I cannot recall a single time we did use it.

As a result, we are forced to make dependencies explicit.  We have found this to be a big help when new developers have joined the project.  They can use the IDE to navigate around the codebase easily, following dependencies between modules.

## Commentary

We ended up using reflection in a couple of places in production code:

* Kotlin code that does HTML templating using Handlebars.
* Our monitoring event types [fall back on reflection](http://wiki.c2.com/?FallBackOnReflection) to flatten monitoring events into name/value pairs that are sent to Kibana. That code is some of the most obscure and confusing in the codebase. I observe that developers frequently do not realise when they have defined a monitoring event that is recorded as unreadable log lines.


# We NEVER use reflection to parse untrusted data.

Reflection and serialisation of untrusted data is a common source of vulnerabilities.  For example, it is the main cause of [CVEs in Jackson](https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=jackson), the most widely used JSON library on the JVM.

Therefore, we never use reflection or serialisation to parse data from incoming HTTP requests or messages, or received in responses from remote services.

Instead, we write functions that map JSON trees to typed data (e.g. [data classes in the domain model](../immutable-domain-models/README.md). 

What do we consider to be ‘untrusted’ data? Basically, anything from the network: bodies in incoming HTTP requests, bodies returned in responses from downstream services.

The database has always been a grey area because access is secured by password, but cannot be completely trusted because people outside the team can access it. For example, on old OPA it turned out that customer support had been given the database password without our knowledge and were monkeying about with JSON records

I’d even consider data passed between our own services to be untrusted because it has crossed the network. “Just because you’re paranoid, it doesn’t mean they’re not out to get you”

As a result, it is more laborious to write code to serialise and deserialise data. But serialisation code can be type-checked, which reduces the testing effort.  E.g. if a property is optional, the type checking will fail if that optionality is not dealt with in the JSON format or represented in the typed data model.

Domain models do not have to be annotated with directives from the networking layer to control serialisation.

The codebase can contain _multiple_ functions that serialise the same domain model type to different JSON formats.  This makes it easy us to support multiple on-the-wire formats when APIs are evolving, and there may be different versions of clients and services running at the same time. 

We can configure our dependency scanner to ignore CVEs in the Jackson databind module when they are related to reflective serialisation.  (That turns out to be all of them.)

The HTTP processing code is composed from multiple functions that receive raw bytes, read the JSON serialisation, and convert the JSON tree into typed domain-model objects.  This allows us to target specific aspects of that processing with exhaustive tests, and run those tests in isolation for better performance.  E.g. fuzz tests of the code that translates JSON trees to typed objects can generate thousands of test cases by mutating JSON data, and do not have to run those cases through the HTTP layer. 


## Commentary

Separating domain model and serialisation paid off when we adopted the Hexagonal Architecture for our services.  We were able to define the domain model and adapter layers for remoting and persistence in different modules, and the domain model had no dependencies on the JSON library (e.g. for annotations).
