# automated tests

The application is deployed to a test environment and thoroughly inspected using automated tesing. This is the first time the application will be deployed into an environment with connectivity to other components.

## Overview

![Automated Tests Stage](images/automated-tests.svg)

## Steps

1. Deployment to test environment
2. Integration tests
3. Automated acceptance tests (UAT)
4. System tests
5. Performance tests
6. End to end (E2E) tests
7. [Web Application Firewall](https://en.wikipedia.org/wiki/Application_firewall) (WAF) tests

## Stage Output

Passing this step means that the deployable artifact from the [packaging stage](../02-packaging/README.md) is ready to be tested from a tester.

The output will be:

* Test results
