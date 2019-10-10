# Pipelines of Transformations

## Make the data fit the calculation

Transform the data into a shape that makes the calculation easy.

If that's hard, make the data fit a calculation that makes the data fit the calculation.

If that's hard... you know what to do!

This results in an architecture of pipelines of functions that translate source data into intermediate forms that simplify each further step of processing.

(Compare with the OO approach of making objects of the application domain model support persistence, UI, validation, serialisation, etc. by polymorphic methods in the model, or annotations on the fields and methods.)

## Refactoring pipelines

Factor big steps of collection pipelines into sequences of small steps.  Those small steps will be more generic, allowing logic to be more easily shared.

Flatten pipelines to make them easier to read when downstream transformations do not need parameters of upstream transformations.

Factor multiple intermediate steps out into extension functions.
