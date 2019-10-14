# Classes, Methods and Extension Functions

We tend to...

* Define data classes to represent the "platonic ideal" of a concept.  E.g. just the essential properties.

* Define fundamental operations as extension functions, [usually in the same file](../organising-code/README.md).

* Define methods on the class only when required for object-oriented polymorphism.  E.g. to implement an interface, or to extend an abstract class.

* Define extension functions in other packages to relate the class to other concepts.  
  * For example, an "adapter" package for persistence of Articles as JSON would define extension functions `Article.toJson(): JsonNode` and `JsonNode.toArticle(): Article`.

* Define withXXX, plusXXX, minusXXX extensions to abstract the use of the builtin `copy` method of data classes where that simplifies code, removes duplication, and/or reduces possibility of coding error.
