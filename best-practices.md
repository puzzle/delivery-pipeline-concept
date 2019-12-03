# best practices

## step execution

* run as many steps in parallel as possible

## testing

Division of the test cases: test pyramid

![Test Pyramid](images/test-pyramid.png)

Image from [Le Blog des Octos](https://blog.octo.com/en/the-test-pyramid-in-practice-5-5/)

## Container Plattform

### Container Build

Changes made for the container (Images itself or self installed stuff) has to pass the same pipeline as the application code itself.

* limit number of commands / lines inside the Dockerfile
  * this will reduce the numbers of layers
  * this will reduce the size of the Docker Image
* [Docker best practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
* [OpenShift image creation best practices](https://docs.openshift.com/container-platform/4.2/openshift_images/create-images.html)
