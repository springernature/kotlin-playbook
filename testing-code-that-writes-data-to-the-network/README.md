Write this...

Things to cover:

## We write formatters as pure functions

Most formatters are a function from a domain type to JsonNode.  Don't couple the formatter to OutputStream or HttpResponse.

This makes them easy to test in isolation: there are no hidden dependencies.

## We never use reflection or serialisation to write data to the network

[We never use reflection or serialisation to write data to the network](../reflection/README.md). This is to decouple on-the-wire formats from the in-memory model, allowing endpoints to support multiple versions of the payload content, selected by HTTP content type negotiation.

## We validate the outputs against published JSON schemas

If we have documented the protocol with JSON schemas and published those schemas for use by other teams, our tests check that the parser generates output that conforms to the schemas we have published.

## We use approval tests for specific scenarios

It is possible to generate JSON that conforms to the published JSON schema but puts values into the wrong properties.  Therefore, we write approval tests to verify that values from the domain objects end up in the right JSON properties of the message.

Use the `stableMapper`
