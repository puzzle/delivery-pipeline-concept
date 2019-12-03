# build and unit tests

## Overview

![Build Stage](images/build.svg)

This stage contains the setup for a delivery pipeline. Normally you will find these elements within the source build

## Steps

1. code compilation and build
2. unit tests
3. static analysis
4. dependency checks
5. security checks
6. artefact generation

### code compilation and build

* build the code

Any build failure must stop the pipeline. This to provide fast feedback.

### unit tests

* run all unit tests
* collect test results
* collect test coverage

Failing unit tests will not stop the pipeline.

Any non passing test must change the status of this step to unstable.

### static analysis

* Static Application Security Testing (SAST)
  * Source Code Analysis Tools: https://www.owasp.org/index.php/Source_Code_Analysis_Tools
  * (OWASP)? SonarQube project covers 20+  programming languages
    * Brakeman for Ruby for example is ok too

### dependency checks

* check dependencies for updates
* OWASP Dependency Check
* licence checks

### security checks

* Dynamic Application Security Testing (DAST)
  * OWASP ZAP (Zed Attack Proxy)

### artefact generation

Packaging of the artifact.

## Stage Output

The output will be:

* application artifacts
* test results
