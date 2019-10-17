# Deployable & Library Modules

We organise our modules into deployable components that are deployed to our cloud platform and library modules that are shared between deployable components.

Source trees for deployable components are at the top-level of the repository.  Library modules are under the lib/ directory.

We have a build and deployment pipeline per deployable component.

The names of deployable components are prefixed by the name of the application, so that related components are grouped together in Cloud Foundry.  For example:

 * opa-web-bff
 * opa-ejp-gateway
 * opa-stoa-gateway
 * opa-jards-gateway
 * thresher-frontend
 * thresher-backend
 * thresher-aqapp-gateway
