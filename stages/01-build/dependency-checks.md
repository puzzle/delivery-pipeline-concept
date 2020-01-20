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

False positives may occur. They can be suppressed very easily:  
*Due to how dependency-check identifies libraries false positives may occur (i.e. a CPE was identified that is incorrect). Suppressing these false positives is fairly easy using the HTML report. In the report next to each CPE identified (and on CVE entries) there is a suppress button. Clicking the suppression button will create a dialogue box which you can simple hit Control-C to copy the XML that you would place into a suppression XML file. If this is the first time you are creating the suppression file you should click the “Complete XML Doc” button on the top of the dialogue box to add the necessary schema elements.*

For further information or a sample suppression file see [OWASP Dependency Check documentation "Suppressing False Positives"](https://jeremylong.github.io/DependencyCheck/general/suppression.html).

### supported languages

OWASP Dependency Checks supports many languages. Some of them are experimental. Use the command line argument *--enableExperimental* if you want to scan a language marked as experimental.
See [OWASP Dependency Check documentation analyzers](https://jeremylong.github.io/DependencyCheck/analyzers/index.html) to check if your language is experimental or not.

### Dependency Check implementations

#### Jenkins Pipeline

Declarative Jenkins Pipeline using [Dependency-Check Jenkins Plugin](https://github.com/jenkinsci/dependency-check-plugin)

<details><summary>pipeline Groovy script</summary>
<p>
Update the DEPENDENCY_CHECK_TOOL to the version installed, see <i>Global Tool Configuration</i>.<br/>
See examples for the ARGUMENTS below at <a href="#dependency-check-examples">Dependency Check Examples</a>

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
See examples for the ARGUMENTS below at <a href="#dependency-check-examples">Dependency Check Examples</a>

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
                owaspDependencyCheck "folder1", "folder2", "EXTRA ARGUMENTS"
            }
        }
    }
}

```
</p>
</details>

The command line option --format implicitly set to 'ALL'. We set all here because we publish XML reports. Default output would be HTML.  
With the format option set to 'ALL' we generate XML, CSV, HTML, JSON and XML files.

### Dependency Check Examples

#### Shared Library Examples

```Groovy
owaspDependencyCheck "app", "api", tool: "owasp-dependency-check-5.2.4", extraArgs: "--enableExperimental --failOnCVSS 7 --project 'My Named Project'"
```

This example calls the owaspDependencyCheck from the Shared Library and

* scans the folders *app* and *api* of the repository
* uses the installed tool *owasp-dependency-check-5.2.4*. The version must match as it's part of the tool's name.
* enables the *experimental analyzers* for broader language support (also see section supported languages)
* lets the pipeline fail when a CVSS score higher than 3 is reached
* sets the *project name* to 'My Named Project'. This project name is displayed as a heading in the report.
* sets the output format implicitly to *--format 'ALL'* so that xml, csv, html, json and xml files are generated

#### Dependency Check Command Line Examples

```bash
dependency-check.sh --scan app --scan api --format 'ALL' --project 'OWASP Dependency Check' --out report --suppression dependency-check-suppression.xml --failOnCVSS 7 --exclude pathPattern
```

This example:

* scans the folders *app* and *api* of the repository
* saves the reports in all available formats
* sets *OWASP Dependency Check* as project name
* saves the reports to the folder *report*
* takes dependency-check-suppression.xml to suppress false positives
* lets the pipeline fail when a CVSS score higher than 3 is reached
* exludes the folders matching the *pathPattern* from the scan

For more information about command line arguments, see [OWASP Dependency Check documentation](https://jeremylong.github.io/DependencyCheck/dependency-check-cli/arguments.html).

## license checks

Check the licenses of the dependencies.
Make sure to only use dependencies with the right license, see [Open Source Licenses](https://opensource.org/licenses).

* NPM: [license-checker](https://www.npmjs.com/package/license-checker)
