# Dependency Checks

Check all included dependencies.

## Check Dependencies for Updates

* Search newer versions for the dependencies.
* Try to automatically update minor versions.

### What To Do If Non-supported Versions Are Found

If that happens, there are two possible solutions:

* the pipeline stops with an error, or
* the pipeline creates a task in the backlog of the developer.

## Check Dependencies for Security Problems

We use the OWASP Dependency Check tool:

* [OWASP Dependency Check OWASP Page](https://owasp.org/www-project-dependency-check/)
* [OWASP Dependency Check GitHub Documentation](https://jeremylong.github.io/DependencyCheck/)

### Security Vulnerabilities Found

Quantify the problems and when the security problem threshold is passed, there are two possible solutions:

* The pipeline stops with an error, or
* the pipeline creates a task in the backlog of the developer.

### False Positives

False positives may occur. They can be suppressed very easily:  

*Due to how dependency-check identifies libraries false positives may occur (i.e. a CPE was identified that is incorrect). Suppressing these false positives is fairly easy using the HTML report. In the report next to each CPE identified (and on CVE entries) there is a suppress button. Clicking the suppression button will create a dialogue box which you can simple hit Control-C to copy the XML that you would place into a suppression XML file. If this is the first time you are creating the suppression file you should click the “Complete XML Doc” button on the top of the dialogue box to add the necessary schema elements.*

For further information or a sample suppression file see [OWASP Dependency Check documentation section "Suppressing False Positives"](https://jeremylong.github.io/DependencyCheck/general/suppression.html).

### Supported Languages

OWASP Dependency Check supports many languages. Some of them are experimental. Use the command line argument *--enableExperimental* if you want to scan a language that is marked as experimental.
See [OWASP Dependency Check documentation section "Analyzers"](https://jeremylong.github.io/DependencyCheck/analyzers/index.html) to check if your language is experimental or not.

### CVSS Score

The pipeline can be configured to fail when a certain CVSS score is exceeded. The CVSSv3 is used to evaluate the CVSS. See [Wikipedia](https://en.wikipedia.org/wiki/Common_Vulnerability_Scoring_System) or [nvd.nist.gov](https://nvd.nist.gov/vuln-metrics/cvss) for more information.

In the [examples below](#dependency-check-examples) we let the pipeline fail when a CVSS score of 5 is reached (MEDIUM). Practice will show which thresholds work.

### Reports

With the format option set to 'ALL' we generate XML, CSV, HTML, JSON and XML files.  
A sample HTML report can be found [here](https://jeremylong.github.io/DependencyCheck/general/SampleReport.html).

### Dependency Check implementations

There are a Jenkins Pipeline Shared Library and an official Dependency-Check Jenkins Plugin available:

#### Jenkins Pipeline Library

You can use [Puzzle's Jenkins Pipeline Shared Library](https://github.com/puzzle/jenkins-pipeline-shared-libraries/)
containing the [OWASP Dependency Check step](https://github.com/puzzle/jenkins-pipeline-shared-libraries/tree/master/vars).

<details><summary>Pipeline Groovy script</summary>
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

The command line option *--format* is implicitly set to *'ALL'*. We set 'ALL' here because we publish XML reports. Default output would be HTML.  
With the format option set to 'ALL' we generate XML, CSV, HTML, JSON and XML files.

#### Jenkins Pipeline

Declarative Jenkins Pipeline using [Dependency-Check Jenkins Plugin](https://github.com/jenkinsci/dependency-check-plugin)

<details><summary>Pipeline Groovy script</summary>
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

### Dependency Check Examples

#### Shared Library Examples

```Groovy
owaspDependencyCheck "app", "api", tool: "owasp-dependency-check-5.2.4", extraArgs: "--suppression 'dependency-check-suppression.xml' --enableExperimental --failOnCVSS 5 --project 'My Named Project'"
```

This example calls the owaspDependencyCheck from the Shared Library and

* scans the folders *app* and *api* of the repository,
* uses the installed tool *owasp-dependency-check-5.2.4* (the version must match as it's part of the tool's name),
* adds the *suppression file dependency-check-suppression.xml* to suppress false positives (also see section [false positives](#false-positives)),
* enables the *experimental analyzers* for broader language support (also see section [Supported Languages](#supported-languages)),
* lets the pipeline fail when a [CVSS](https://en.wikipedia.org/wiki/Common_Vulnerability_Scoring_System) score higher than 5 is reached (CVSSv3 score is used here),
* sets the *project name* to 'My Named Project' (this project name is displayed as a heading in the report),
* sets the output format implicitly to *--format 'ALL'* so that xml, csv, html, json and xml files are generated.

#### Dependency Check Command Line Examples

```bash
dependency-check.sh --scan app --scan api --format 'ALL' --project 'OWASP Dependency Check' --out report --suppression dependency-check-suppression.xml --failOnCVSS 5 --exclude pathPattern
```

This example:

* scans the folders *app* and *api* of the repository,
* saves the reports in all available formats,
* sets *OWASP Dependency Check* as project name,
* saves the reports to the folder *report*,
* takes dependency-check-suppression.xml to suppress false positives (also see section [false positives](#false positives)),
* lets the pipeline fail when a CVSS score higher than 5 is reached, and
* excludes the folders matching the *pathPattern* from the scan.

For more information about command line arguments, see [OWASP Dependency Check documentation section "Command Line Arguments"](https://jeremylong.github.io/DependencyCheck/dependency-check-cli/arguments.html).

## License Checks

Check the licenses of the dependencies.
Make sure to only use dependencies with the right license, see [Open Source Licenses](https://opensource.org/licenses).

* NPM: [license-checker](https://www.npmjs.com/package/license-checker)
