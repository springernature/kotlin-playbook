# We avoid the !! operator

The `!!` operator casts away nullability, undermining any help that the typechecker can give you in handling nulls.  If a null reference is "double-banged", the operator throws an exception that does not have a useful error message.

We consider any use of the `!!` operator an indicator that our design is flawed, and refactor so that it is unnecessary (preferred), or throws an exception with a descriptive error message.
