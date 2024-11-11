# Release

The production-ready artifact is promoted to the production environment.

## Overview

![Release Stage](images/release.svg)

## Steps

1. Deployment to production environment
2. Automated smoke tests

### Deployment to production environment

App deploying can be done by different [deployment strategies](https://docs.openshift.com/container-platform/latest/applications/deployments/deployment-strategies.html).

More details and tool suggestions: [deployment.md](deployment.md)

### Automated smoke tests

Non-exhaustive tests ensuring the most important functionality

## Stage Output

The artifact from the [packaging stage](../02-packaging/README.md) runs on production.
