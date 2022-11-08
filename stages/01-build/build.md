# Build

## Java

### Use Maven/Gradle Wrapper

By using [Maven Wrapper](https://github.com/apache/maven-wrapper) or [Gradle Wrapper](https://docs.gradle.org/current/userguide/gradle_wrapper.html)
the build process is encapsulated and provided by the project.

Using these tools obsoletes the need to have Maven or Gradle installed on your machine or on the build server.
The correct version of the build tool will automatically be downloaded as a Java JAR artifact.

## Go

In Go the dependencies are handled as modules. They are defined in the mod.go file. Modules can be browsed [here] (https://pkg.go.dev/).
Task running in Go is mostly implemented in the language. For example, tests can be defined in a file ending with *** test.go and can be executed with the command <code>go test</code> [more info](https://go.dev/doc/tutorial/add-a-test). Similarly, the command <code>go build</code> will compile the compile the main.go program in your current directory, generatingang an executeable binary of your application [more info, how to build for different OS](https://www.digitalocean.com/community/tutorials/building-go-applications-for-different-operating-systems-and-architectures).

## JavaScript

In JavaScript dependencies as well as tasks are defined in a .json file. The there defined tasks/scripts can be run under their defined name via <code>npm</code> [example and more info](https://blog.jayway.com/2014/03/28/running-scripts-with-npm/).
