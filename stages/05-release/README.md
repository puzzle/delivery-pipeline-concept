# release

The production ready artefact is promoted to the production environment.

## Overview

![Release Stage](images/release.svg)

## Steps

1. deployment to production environment
    * [deployment strategyes](https://docs.openshift.com/container-platform/latest/applications/deployments/deployment-strategies.html)
2. automated smoke tests

    non-exhaustive tests to ensure that the most important functions work

## Stage Output

The artefact from the [packaging stage](../02-packaging/README.md) runs on production.
