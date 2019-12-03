# Packaging

## Overview

![Packaging Stage](images/packaging.svg)

This stage contains packages the application for the deployment.

## Steps

1. packaging
2. component tests
3. security checks

### packaging

When deploying to a Container Plattform, this step includes a container build.

More details and tool suggestions: [packaging.md](packaging.md)

### component tests

The application has to run production-like to be tested.

* run component tests

Testing guidelines: [test pyramid](../../best-practices.md#testing)

### security checks

Security scan of the deployable artefact.

This mostly only applies for Docker Container.

More details and tool suggestions: [security-checks.md](security-checks.md)

## Stage Output

The output will be:

* deployable artefact of the application
  * e.g application container image
* test results
