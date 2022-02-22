# automated tests

The application is deployed to a test environment and thoroughly inspected using automated testing. This is the first time the application will be deployed into an environment with connectivity to other components.

## Overview

![Automated Tests Stage](images/automated-tests.svg)

## Steps

1. Deployment to test environment
2. Integration tests
3. Automated acceptance tests (UAT)
4. System tests
5. Performance tests
6. End to end (E2E) tests
7. [Security tests](#security-tests)

### Security Tests

* Dynamic Application Security Testing (DAST)
* Web Application Firewall (WAF) tests

More details and tool suggestions: [security-checks.md](security-checks.md)

## Stage Output

Passing this step means that the deployable artifact from the [packaging stage](../02-packaging/README.md) is ready to be tested from a tester.

The output will be:

* Test results
