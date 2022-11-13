# Delivery Pipeline Concept

Conceptual description of a Delivery Pipeline.

Includes patterns and best practices.

## What Is a Delivery Pipeline

The goal of CI/CD is to enable a constant flow of software updates into production to quicken release cycles and reduce the risks associated with development.

A CI/CD pipeline achieves this by automating the steps between checking code into version control and releasing to production. It gives developers quick feedback on every stage with growing maturity of the artifact you want to deploy.

## Overview

![Delivery Pipeline Overview](images/delivery-pipeline-overview.svg)

## Stages

The pipeline consists of multiple stages. Stages are executed sequentially and depend on the previous stages successful completion. Each stage contains individual tasks (steps) which may run in parallel.

1. [Build and Unit Tests](stages/01-build/README.md)

   This stage contains the classic application build as well as any checks on the code in isolation (unit testing, static code analysis).

2. [Packaging](stages/02-packaging/README.md)

   In this stage the application is packaged for deployment and tested as a whole.

3. [Automated Tests](stages/03-automated-tests/README.md)

   The application is deployed to a test environment and thoroughly inspected using automated testing.

4. [Manual Tests](stages/04-manual-tests/README.md)

   This optional stage is used to test borderline cases which can not accomplished with automated tests.

5. [Release](stages/05-release/README.md)

   The Application is promoted to the production environment.

Each stage provides feedback to the developers in the form of logs and test reports. The scope of the test becomes wider / more integrated with every stage.

If a stage fails - either through an unrecoverable error (e.g. build failure) or too many tests / checks failing - the pipeline is halted. Developers have to fix the problems in the source and launch the pipeline from the start.

The pipeline can be triggered on commit, scheduled, or manually. Generally, we want to push as many artifacts as far down the pipeline as possible. However, advanced tests are resource expensive, so we have to make a trade-off. For example, we could trigger stages 1 & 2 on every commit and 3 & 4 only on a weekly schedule or certain branches.

## Planning of a Pipeline

- how to decide what software to use (?)
- different tests
- [deployment pattern structures](deployment-pattern.md)

## Additional

- [Best Practices](best-practices.md)
- [Glossary](glossary.md)
- [Good additional description of CI/CD](https://harness.io/blog/what-is-ci-cd) [another one](https://www.plutora.com/blog/understanding-ci-cd-pipeline)

## Open Points

- monitoring integration
- pipeline status
- container security
- list technologies used in a container (baseimage and self installed)
- environment config
  - config itself
  - credentials

## About This Repository

### Images

The images are created by [app.diagrams.net](https://app.diagrams.net/) (former draw.io)

Editing:

- The diagram is included in the image.
- Just load the image into draw.io.

Export:

- check _Transparent Background_
- check _Include a copy of my diagram_
