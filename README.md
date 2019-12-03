# Delivery Pipeline Concept

Conceptual description of a Delivery Pipeline.

Includes patterns and best practices.

## What is a delivery pipeline

A delivery pipeline provides fast feedback on every stage with growing maturity of the image you want to deploy.

## Overview

![Delivery Pipeline Overview](images/delivery-pipeline-overview.svg)

## Stages

Stages of the delivery pipeline.

1. [build and unit tests](stages/build/build.md)
2. [packaging](stages/packaging/packaging.md)
3. [automated acceptance tests](stages/automated-tests/automated-tests.md)
4. [manual tests](stages/manual-tests/manual-tests.md)
5. [release](stages/release/release.md)

Each individual stage provides fast feedback to the developer.

The artefact gains on maturity passing the stages from top to bottom.
