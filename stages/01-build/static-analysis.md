# static analysis

Check the code without building it.

## static code analysis

* Java: [SonarQube](https://www.sonarqube.org/)
* JavaScript: jslint
* Ruby: [RuboCop](https://github.com/rubocop-hq/rubocop)
* TypeScript: TSLint

## Static Application Security Testing (SAST)

* [Source Code Analysis Tools](https://www.owasp.org/index.php/Source_Code_Analysis_Tools)
  * [OWASP SonarQube project](https://www.owasp.org/index.php/OWASP_SonarQube_Project) covers 20+  programming languages
* Ruby: [Brakeman](https://brakemanscanner.org/)

### Jenkins Pipeline

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
