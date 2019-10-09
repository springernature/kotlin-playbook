# Classes, Methods and Extension Functions

We tend to...

* Define data classes to represent the "platonic ideal" of a concept.  E.g. just the essential properties.

* Define fundamental operations as extension functions, [usually in the same file](../organising-code/README.md).

* Define methods on the class only when required for object-oriented polymorphism: to implement an interface, or to extend an abstract class.

* Define extension functions that relate the class to other concepts in other packages or modules.  E.g. in "adapter" packages, when using the Hexagonal architecture.
