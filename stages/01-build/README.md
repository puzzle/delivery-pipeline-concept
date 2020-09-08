# Build and Unit Tests

## Overview

![Build Stage](images/build.svg)

This stage contains the classic application build as well as any checks on the code in isolation (unit testing, static code analysis).

The goal of the test/checks in this stage is to make sure that each unit of code performs correctly. Buisness logic shoud - if possible - be checked in this stage.

## Steps

1. [Code Compilation and Build](#code-compilation-and-build)
2. [Unit Tests](#unit-tests)
3. [Static Analysis](#static-analysis)
4. [Dependency Checks](#dependency-checks)
5. [Security Checks](#security-checks)
6. [Artifact Generation](#artifact-generation)

### Code Compilation and Build

* Build the code.

Any build failure must stop the pipeline. This to provide fast feedback.

More details and tool suggestions: [build.md](build.md)

### Unit Tests

The unit test stage should: 

* run all unit tests,
* collect test results, and
* collect test coverage.

Failing unit tests will not stop the execution of the step to ensure proper collection of the results of all tests at the end of the step.
Any non-passing test must change the status of this step to unstable.

Stop the pipeline if the step status returns unstable (failing unit tests).

Testing guidelines: [test pyramid](../../best-practices.md#testing)

### Static Analysis

The static analysis step consists of:

* static Code Analysis (SCA), and 
* static Application Security Testing (SAST).

More details and tool suggestions: [static-analysis.md](static-analysis.md)

### Dependency Checks

This pipeline step consists of:

* checking dependencies for updates,
* checking dependencies for security problems, and
* checking licenses.

More details and tool suggestions: [dependency-checks.md](dependency-checks.md)

### Security Checks

* Dynamic Application Security Testing (DAST)

More details and tool suggestions: [security-checks.md](security-checks.md)

### Artifact Generation

Generation of the application artifact.

## Stage Output

The output will be:

* application artifacts, and
* test results
