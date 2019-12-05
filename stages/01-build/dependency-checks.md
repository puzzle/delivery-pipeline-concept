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

### Dependency Check implementations

#### Jenkins Pipeline

Declarative Jenkins Pipeline using [Dependency-Check Jenkins Plugin](https://github.com/jenkinsci/dependency-check-plugin)

<details><summary>pipeline Groovy script</summary>
<p>
Update the DEPENDENCY_CHECK_VERSION to the version installed, see <i>Global Tool Configuration</i>.

It scans the folders <i>app</i> and <i>api</i> of the Pixi repository.

```Groovy
pipeline {
    agent ...
    options ...

    environment {
      DEPENDENCY_CHECK_VERSION = '5.2.4'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/DevSlop/Pixi.git'
            }
        }
        stage('Preparation') {
            steps {
                // clean and prepare report folder
                sh 'rm -rf report'
                sh 'mkdir report'
            }
        }
        stage('Check') {
            steps {
                withEnv(["PATH+DC=${tool name: env.DEPENDENCY_CHECK_VERSION, type: 'dependency-check'}/bin"]) {
                    // tool version infos
                    sh "dependency-check.sh --version"

                    // do dependency check
                    sh "dependency-check.sh --scan app --scan api --format 'ALL' --project 'Cryptopus OWASP Dependency Check' --out report"
                }
            }
            post {
                always {
                    junit 'report/*junit.xml'  // JUnit plugin
                    dependencyCheckPublisher pattern: 'report/dependency-check-report.xml'
                }
            }
        }
    }
}
```
</p>
</details>

## license checks

Check the licenses of the dependencies.
Make sure to only use dependencies with the right license, see [Open Source Licenses](https://opensource.org/licenses).

* NPM: [license-checker](https://www.npmjs.com/package/license-checker)
