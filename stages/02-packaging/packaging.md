# Packaging

Packaging of the application artifact to be deployed.

## Docker Container

### Container Build

* take newest Baseimage for your Container
* run software and library updates
* only install really needed software and libraries for running the application
* Container build best practices: [Container Build](../../best-practices.md#container-build)

### Hardening the container

This means middleware and OS. Beside patching the system on a regular basis the following points are important:

* controlled admin access,
* restricted network traffic,
* encrypted disk data and/or communication protocols.
