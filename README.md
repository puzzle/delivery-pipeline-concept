# Delivery Pipeline Concept

Conceptual description of a Delivery Pipeline.

Includes patterns and best practices.

## What is a delivery pipeline

A delivery pipeline provides fast feedback on every stage with growing maturity of the image you want to deploy.

## Overview

![Delivery Pipeline Overview](images/delivery-pipeline-overview.svg)

## Stages

Stages of the delivery pipeline.

1. [build and unit tests](stages/01-build/README.md)
2. [packaging](stages/02-packaging/README.md)
3. [automated tests](stages/automated-tests/README.md)
4. [manual tests](stages/manual-tests/README.md)
5. [release](stages/release/README.md)

Each individual stage provides fast feedback to the developer.

The artefact gains on maturity passing the stages from top to bottom.

## Additional

* [best practices](best-practices.md)

## About this repository

### images

The images are created by [draw.io](https://www.draw.io/)

Editing:

* The diagram is included in the image.
* Just load the image into draw.io.

Export:

* check *Transparent Background*
* check *Include a copy of my diagram*
