# static analysis

Static code analysis (SCA) checks the software source code for

* potential and obvious errors
* code smells

## static code analysis

It is a good practice to use a combination of several tools.

### Java SCA

* [Sonar](http://www.sonarqube.org/) graphical overview of different analyzers
* [Find Bugs](http://findbugs.sourceforge.net/) finds several problems in the source code
* [Checkstyle](http://checkstyle.sourceforge.net/) finds several problems in the source code
* [PMD](http://pmd.sourceforge.net/) finds several problems in the source code
* [JDepend](http://www.clarkware.com/software/JDepend.html) finds cyclic dependencies
* [Classcycle](http://classycle.sourceforge.net/) finds cyclic dependencies

### Ruby SCA

* [RuboCop](https://github.com/rubocop-hq/rubocop) finds several problems in the source code
* [MetricFu](https://github.com/metricfu/metric_fu) overview of different analyzers
* [Rails Best Practices](https://github.com/railsbp/rails_best_practices) finds several issues in Rails applications
* [Flay](http://ruby.sadi.st/Flay.html) find duplicated source code
* [Flog](http://ruby.sadi.st/Flog.html) find source code with high complexity

### Frontend SCA

* JavaScript: [ESLint with «Airbnb JavaScript Style Guide»-based rules](https://github.com/puzzle/frontend-guides/blob/master/doc/03_javascript.md)
* TypeScript: [TSLint mit «Airbnb JavaScript Style Guide»-based rules](https://github.com/puzzle/frontend-guides/blob/master/doc/03_typescript.md)

## Static Application Security Testing (SAST)

* [Source Code Analysis Tools](https://www.owasp.org/index.php/Source_Code_Analysis_Tools)
  * [OWASP SonarQube project](https://www.owasp.org/index.php/OWASP_SonarQube_Project) covers 20+  programming languages

### Ruby SAST

* [Brakeman](http://brakemanscanner.org/) finding security issues in Rails applications

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
