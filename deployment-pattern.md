# CI/CD Deployment Patterns

As CI/CD deployment patterns mimic the software lifecycle (SDLC), they vary in relation to the type of organization/industry as well as governance and opinions.
However, all great deployment patterns are fast, safe and repeatable.

A traditional deployment method is the use of shell scripts. However, these don't scale well in distributed systems and therefore lack repeatability.

Things to consider when designing a modern deployment pipeline:

- Quality & test coverage/automation
- Variables & secrets
- Auditing & compliance (e.g. logs, especially important in the banking sector)
- Security (e.g. vulnerability scanning)
- Release strategy (blue/green, canary)
- Rollback and failure strategies
- Target environment configuration
- Approvals
- Maintaining SLA, SLO, SLI

Important factors driving the implementation of CI/CD:

- Environments (increase of deployments to multiple environments and locations [Example of how to Setup a Multi-Environment CI/CD Pipeline on AWS](https://www.phdata.io/blog/setting-up-multi-environments-cicd-pipelines-on-aws/))
- Tests (the automation of several different tests)
- Services (the use of several microservices, requires a service orchestration in order to be flexible and reuseable [More info](https://cloudify.co/blog/why-service-orchestration-matters/))
- Outcome (stable version while adhereing to SLA/SLO/SLI [More info about SLA/SLO/SLI](https://www.atlassian.com/incident-management/kpis/sla-vs-slo-vs-sli))
- People (or avoidance of manual approval steps)

## Common Deployment Patterns

### Outcome Centric (blue/green group or canary)

This approach handles the introduction of changes by deploying the current and new version into two environments. It is commonly used for applications that cannot have downtime. The current (green) environment serves the real traffic. While in the blue environment the changes are implemented and tested. Once the blue environment has matured, the traffic is flipped.

A flavor of this is the canary release. Here the traffic is rerouted slowly, introducing the changes to a subset of users before rolling out the change to the entire infrastructure. This can be favorable, as only a small subset of your users "test" the new environment for you and if any problems are found, they can simply be rerouted back to the old version. This is especially beneficial for capacity testing.

Example: [Facebook Mobile Release Process](https://www.infoq.com/presentations/Facebook-Release-Process/)

### Button Push

This deployment pattern blends human and systemic expertise, as it allows for human approval/interaction at the finish line. This could have several reasons, such as compliance, regulations or confidence building.

### Test Automation

A fully automated test environment requires a thorough development phase, in order to acurrately cover the different parts of the application. With a modification in the application also the test environment needs to be modified. Full test automation can run independently and allows developers to receive feedback early and cost efficiant in their development process.

A good addition to this practise can be a quality assurance pipeline.

### Multiservice

Multiservice as a deployment pattern is mostly used in organizations, which are building platforms.
In this pattern, multiple services (and their artifacts) are used to create the environment, where the traditional approach would be one artifact per pipeline.
Testing multiple services in the same pipeline helps build confidence.
In this scenario it is crucial, that each service runs independent, to avoid complex scenarios.

### Multiservice Environments

Automated deployment of multiple artifacts, with not every artifact and service being created equally. This helps automate an often complex deployment scenario.
This pattern often incorporates separate pipelines throughout pre-production and production before merging them.

### Semi Approval

This pattern is often found in organizations with high-risk changes, such as financial services.
It is a mix of automation, where possible and human approval, where necessary.

### Full Approval Pattern

The full approval pattern takes the semi approval pattern and adds a manual approval to every stage. This very conservative pattern can be used to track bottlenecks in a manual testing process.

### GitOps

In this deployment pattern the desired configuration is stored in a config(uration) file on a version control system, such as Git. Changes can be implement via pull requests, code reviews or merges to master. This system represents the fastest pattern to implement changes.
Therefore, it is perfect for environments, which require a high rate of change, such as a testing environment.

However, the main focus of GitOps is infrastructure automation and continuous deployment/delivery.
