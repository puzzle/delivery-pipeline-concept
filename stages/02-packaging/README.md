# Packaging

## Overview

![Packaging Stage](images/packaging.svg)

In this stage the application is packaged for deployment and tested as a whole.

## Steps

The steps are:

1. packaging,
2. component tests, and
3. security checks

### Packaging

When deploying to a Container Plattform, this step includes a container build.

More details and tool suggestions: [packaging.md](packaging.md)

### Component Tests

The goal is to check that all the units of the application work together as expected. External resurces are simulated/mocked (e.g. with an in-memory database).

* run component tests

Testing guidelines: [test pyramid](../../best-practices.md#testing)

### Security Checks

Security scan of the deployable artifact.

This mostly only applies to container images.

More details and suggestions for tools: [security-checks.md](security-checks.md)

## Stage Output

The output will be:

* a deployable artifact of the application (e.g application container image), and
* test results.
