# Gradle
https://docs.gradle.org/current/userguide/gradle_wrapper.html

## Wrapper
The Wrapper is a script that invokes a declared version of Gradle, downloading it beforehand if necessary.

**benefits:**
* Standardizes a project on a given Gradle version, leading to more reliable and robust builds.
* Provisioning a new Gradle version to different users and execution environment is as simple as changing the Wrapper definition.
## Upgrading Wrapper
* upgrade the Gradle version is manually change the distributionUrl property in the Wrapper property file.
* run the wrapper task and provide the target Gradle version as described in Adding the Gradle Wrapper. 
## Using Wrapper
Depending on the operating system you either run gradlew or gradlew.bat instead of the gradle command. The following console output demonstrate the use of the Wrapper on a Windows machine for a Java-based project.

**Executing the build with the Wrapper batch file**
```
gradlew.bat build
```
