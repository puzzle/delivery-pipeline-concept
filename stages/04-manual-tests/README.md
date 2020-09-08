# manual tests

This optional stage is used to test borderline cases which can not accomplished with automated tests

## Overview

![Manual Tests Stage](images/manual-tests.svg)

## Steps

1. Deployment to [User Acceptance Testing](https://en.wikipedia.org/wiki/Acceptance_testing) (UAT) environment
2. Automated smoke tests (non-exhaustive tests to ensure correct behaviour of critical functionality)
3. Manual tests

## Stage Output

Passing this step means that the deployable artifact from the [packaging stage](../02-packaging/README.md) is ready to deployed to production.

The output will be:

* Test report
