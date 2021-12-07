# Security Checks

Security testing of the application artifact to be deployed.

## Container

Tools for the analysis and inspection of containers for vulnerabilities:

* [Anchore](https://github.com/anchore/anchore-engine)
* [Clair](https://github.com/quay/clair)

### What to Do in Case of Found Security Vulnerabilities?

Quantify the problems, and if the security problem threshold is passed:

* stop the pipeline with an error, or
* let the pipeline create a task in the backlog.
