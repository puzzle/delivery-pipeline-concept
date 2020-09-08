# Security Checks

Security testing of the application artifact to be deployed.

## Docker Container

* Container Security Scan
  * [Anchore](https://github.com/anchore/anchore-engine)
  * [Clair](https://github.com/quay/clair)

### security vulnerabilities found

First quantify the problems.

What happens, when the security problem threshold is passed?

Two possible solutions:

* the pipeline stopps with an error
* the pipeline creates a task in the backlog
