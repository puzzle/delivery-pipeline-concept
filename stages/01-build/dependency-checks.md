# dependency checks

Check all included dependencies.

## check dependencies for updates

* Search newer versions for the dependencies.
* Try to automatically update minor versions.

### non-supported versions found

What happens, when the check finds non-supported versions of the dependencies?

Two possible solutions:

* the pipeline stopps with an error
* the pipeline creates a task in the backlog of the developer

## check dependencies for security problems

* [OWASP Dependency Check](https://www.owasp.org/index.php/OWASP_Dependency_Check)

### security vulnerabilities found

First quantify the problems.

What happens, when the security problem threshold is passed?

Two possible solutions:

* the pipeline stopps with an error
* the pipeline creates a task in the backlog of the developer

## license checks

Check the licenses of the dependencies.
Make sure to only use dependencies with the right license, see [Open Source Licenses](https://opensource.org/licenses).

* NPM: [license-checker](https://www.npmjs.com/package/license-checker)
