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

* [OWASP Dependency Check OWASP Page](https://www.owasp.org/index.php/OWASP_Dependency_Check)
* [OWASP Dependency Check GitHub Documentation](https://jeremylong.github.io/DependencyCheck/)

### security vulnerabilities found

First quantify the problems.

What happens, when the security problem threshold is passed?

Two possible solutions:

* the pipeline stopps with an error
* the pipeline creates a task in the backlog of the developer

### false positives

False positives may occur. They can be suppressed very easily. See [OWASP Dependency Check documentation](https://jeremylong.github.io/DependencyCheck/general/suppression.html).

### supported languages

OWASP Dependency Checks supports many languages. Some of them are experimental. Use the command line argument <i>--enableExperimental</i>.
See [OWASP Dependency Check documentation](https://jeremylong.github.io/DependencyCheck/analyzers/index.html).

### Dependency Check implementations

#### Jenkins Pipeline

Declarative Jenkins Pipeline using [Dependency-Check Jenkins Plugin](https://github.com/jenkinsci/dependency-check-plugin)

<details><summary>pipeline Groovy script</summary>
<p>
Update the DEPENDENCY_CHECK_TOOL to the version installed, see <i>Global Tool Configuration</i>.

```Groovy
pipeline {
    agent ...
    options ...

    environment {
      DEPENDENCY_CHECK_TOOL = 'owasp-dependency-check-5.2.4'
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
        stage('Dependency Check') {
            steps {
                withEnv(["PATH+DC=${tool name: env.DEPENDENCY_CHECK_TOOL, type: 'dependency-check'}/bin"]) {
                    // tool version infos
                    sh "dependency-check.sh --version"

                    // do dependency check
                    sh "dependency-check.sh ARGUMENTS"
                }
            }
            post {
                always {
                    dependencyCheckPublisher pattern: 'report/dependency-check-report.xml'
                }
            }
        }
    }
}
```
</p>
</details>

#### Jenkins Pipeline Library

You can use [Puzzle's Jenkins Pipeline Shared Library](https://github.com/puzzle/jenkins-pipeline-shared-libraries/)
containing the [OWASP Dependency Check step](https://github.com/puzzle/jenkins-pipeline-shared-libraries/tree/master/vars).

<details><summary>pipeline Groovy script</summary>
<p>

```Groovy
@Library('jenkins-pipeline-shared-libraries') _

pipeline {
    agent ...
    options ...

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/DevSlop/Pixi.git'
            }
        }
        stage('Dependency Check') {
            steps {
                owaspDependencyCheck "app", "api", tool: "owasp-dependency-check-5.2.4", extraArgs: "--enableExperimental"
            }
        }
    }
}

```
</p>
</details>

### Dependency Check Examples

#### Dependency Check Command Line Examples

<details><summary>Command Line Example</summary>
<p>
The following example:
 <ul>
  <li>scans the folders <i>app</i> and <i>api</i> of the repository</li>
  <li>saves the reports in all available formats</li>
  <li>takes dependency-check-suppression.xml suppress false positives</li>
  <li>lets the pipeline fail with a CVSS score higher than 3</li>
  <li>exludes the folders matching the <i>pathPattern</i> from the scan</li>
</ul> 
</p>
</details>

```Bash
sh "dependency-check.sh --scan app --scan api --format 'ALL' --project 'OWASP Dependency Check' --out report --suppression dependency-check-suppression.xml --failOnCVSS 3 --exclude pathPattern --enableExperimental"
```
For more information about command line arguments, see [OWASP Dependency Check documentation](https://jeremylong.github.io/DependencyCheck/dependency-check-cli/arguments.html).

#### Shared Library Examples

<details><summary>Shared Library Example</summary>
<p>
The following example calls the owaspDependencyCheck from the Shared Library and 
<ul>
  <li>scans the folders <i>app</i> and <i>api</i> of the repository</li>
  <li>uses the installed tool <i>owasp-dependency-check-5.2.4</i></li>
  <li>enables the experimental analyzers for broader language support</i></li>
</ul> 
</p>
</details>

```Groovy
owaspDependencyCheck "app", "api", tool: "owasp-dependency-check-5.2.4", extraArgs: "--enableExperimental"
```


## license checks

Check the licenses of the dependencies.
Make sure to only use dependencies with the right license, see [Open Source Licenses](https://opensource.org/licenses).

* NPM: [license-checker](https://www.npmjs.com/package/license-checker)
