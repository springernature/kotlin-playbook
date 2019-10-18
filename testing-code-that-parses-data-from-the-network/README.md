# Testing code that parses data from the network

## We write parsers as pure functions

Easy to test in isolation.

No hidden dependencies.

## [We never use reflection to parse data from the network](../reflection/README.md)

This is (a) to avoid remote code execution vulnerabilities in popular serialisation libraries, and (b) to decouple on-the-wire formats from the in-memory model, allowing endpoints to support multiple versions of the payload content, selected by HTTP content type negotiation.

## We validate the test _inputs_ against published JSON schemas

If we have documented the protocol with JSON schemas and published those schemas for use by other teams, our tests check that the input data used to test our parsers conform to the schemas we have published.


## We fuzz test parser functions

We fuzz test parser functions to ensure that they do not throw unexpected exceptions that cannot be type checked.

If a function is parsing incoming requests, parse errors should be reported to the client as a 400 Bad Request status with a payload that identifies th3

report parse errors as a Result that can be converted to an appropriate HTTP status (e.g. 500 Bad Request). 400 HTTP status and do not throw unexpected exceptions that would conv


More things to cover:
 * parsing incoming requests vs responses to outgoing requests
 * fuzz testing (e.g. with Snodge)
