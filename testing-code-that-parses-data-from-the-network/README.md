# Testing code that parses data from the network

## We write parsers as pure functions

Easy to test in isolation.

No hidden dependencies.

## [We never use reflection to parse data from the network](../reflection/README.md)

This is (a) to avoid remote code execution vulnerabilities in popular serialisation libraries, and (b) to decouple on-the-wire formats from the in-memory model, allowing endpoints to support multiple versions of the payload content, selected by HTTP content type negotiation.

## We validate the test _inputs_ against published JSON schemas

If we have documented the protocol with JSON schemas and published those schemas for use by other teams, our tests check that the input data used to test our parsers conform to the schemas we have published.

## We test parsers for incoming requests with fuzz tests

If a function is parsing incoming requests, parse errors should be returned from the function as a Result Failure, not an exception.  This should be unit tested. 

A JSON parse error should be reported by an `InvalidJson` error code that contains a JSON Pointer identifying the invalid elements in the JSON message.  We do not write a unit est for each way that a JSON message can be invalid – there are far to many! Instead we fuzz test parser functions to ensure that they do not throw unexpected exceptions that cannot be type checked and that any errors are reported by an InvalidJson error code with a valid JSON pointer.

We use the Snodge library in our fuzz tests.  Snodge is a mutation fuzzer: it mutates known good messages to produce potentially invalid messages.  We follow a TDD process to write the success cases of a parser, so our tests already have known good messages when we come to the fuzz tests.

## We do not always fuzz test parsers for responses

If a function is parsing a response from a service, we [allow it to throw an exception on invalid input if the application's global error handler is adequate](../error-reporting/README.md).

