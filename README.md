Maven Java starter
===================================================================================================

![GitHub](https://img.shields.io/github/license/luiinge/maven-java-starter?style=plastic)
![Maven Central](https://img.shields.io/maven-central/v/io.github.luiinge/maven-java-starter?style=plastic)
![GitHub Workflow Status (branch)](https://img.shields.io/github/workflow/status/luiinge/maven-java-starter/Test%20with%20Maven/master?style=plastic)

This artifact is a simple `pom.xml` configuration file that eases the sometimes painful process of
setting up the basic infrastructure for a new Java project using Maven.


Usage
---------------------------------------------------------------------------------------------------

Simply use this artifact as the parent project. Add the following in your `pom.xml`:
```xml
   <parent>
        <groupId>io.github.luiinge</groupId>
        <artifactId>maven-java-starter</artifactId>
        <version>11.2.0</version>
   </parent>
```

If your project already have a parent project, you may modify the parent `pom.xml`, if it is a
viable option. As a last resort, you always can copy the configuration of this artifact and paste
it into yours.

> The *major* segment of the artifact version indicates the Java version that the configuration is 
> tuned for (currently only Java 11), but nothing prevents you from using it with 
> other versions


What includes?
---------------------------------------------------------------------------------------------------

- Sets source and output encodings to `UTF-8`
- Sets the specific Java version
- Includes the SL4FJ API as a `compile`dependency
- Includes Log4J, JUnit 4, Hamcrest and AssertJ as `test` dependencies
- Uses Surefire and Failsafe for testing in the build lifecycle. Also uses JaCoCo as agent in order
to collect coverage data
- Generates JAR files with the source
- Pre-configured profiles:
    - Static analysis tools including Spotbugs, Checkstyle, and PMD
    - Obsolete dependencies analysis 
    - Javadoc generation
    - SonarCloud analysis  
    - OSSRH deployment
   

Profiles
---------------------------------------------------------------------------------------------------

The pre-configured profiles include one o more Maven plugins into the build. Each profile can be 
activated by using either its profile identifier, or a property with the same name. The 
following invokations are equivalent:
```shell
mvn clean install -Pcheck.code.pmd,sonar
mvn clean install -Dcheck.code.pmd -Dsonar
```
So, you may also enable certain profiles declaring the proper property:
```xml
<properties>
    <check.code.pmd>true</check.code.pmd>
    <sonar>true</sonar>
</properties>
```
For an exhaustive list of every configuration option of each plugin, please check the corresponding 
links provided at each profile section.


### Static analysis tools

#### `check.versions`
Display new versions of the project dependencies using [Versions Maven Plugin](https://www.mojohaus.org/versions-maven-plugin/)
#### `check.code.checkstyle`
Check code quality using [Apache Maven Checkstyle Plugin](https://maven.apache.org/plugins/maven-checkstyle-plugin/)
#### `check.code.pmd`
Check code quality using [Apache Maven PMD Plugin](https://maven.apache.org/plugins/maven-pmd-plugin/)
#### `check.code.spotbugs`
Check code quality using [SpotBugs Maven Plugin](https://spotbugs.github.io/spotbugs-maven-plugin/)
#### `check.code`
Enable every `check.code.xxx` profile at the same time (just out of convenience)

### Remote analysis tools

#### `sonar`
Perform a [SonarCloud](https://sonarcloud.io/projects) scan actions after completing the `verify` goal (since
test results are part of the source data). You can configure the server connection properties using
the following properties (although is pre-configured with the usual values):
```
    <sonar.projectKey>${project.organization.name}_${project.artifactId}</sonar.projectKey>
    <sonar.organization>${project.organization.name}</sonar.organization>
    <sonar.host.url>http://sonarcloud.io</sonar.host.url>
```
Tips:
- You may want to specify the code branch by adding the argument `-Dsonar.branch.name=...`.  
- You should establish the environment variable `SONAR_TOKEN` (different for each project).
- You can include the results of other static analysis in your Sonar report by enabling the specific
profile along with the `sonar` profile. For example, in order to notify the code coverage with
JaCoCo, the command should be something like:
  ```shell
  mvn clean verify -Psonar,generate.reports.jacoco
  ```  

### Report generation

> Using the following profiles, reports are *not* generated at the `site` phase
> but as soon as their source data is available.

#### `generate.reports.jacoco`
Generate coverage report using [JaCoCo Maven Plugin](https://www.eclemma.org/jacoco/trunk/doc/maven.html). 
The output directory is defined by the property `project.docdir.coverage`.

#### `generate.reports.javadoc`
Generate Javadoc documentation using [Apache Maven Javadoc Plugin](https://maven.apache.org/plugins/maven-javadoc-plugin/)
. The preset configuration is set to only generate documentation for public methods and classes 
that are not in a `*.internal` nor `*.impl` package. The output directory is defined by the 
property `project.docdir.javadoc`.

#### `generate.reports.dependency`
Generate a project dependencies report using [Apache Maven Project Info Reports Plugin](https://maven.apache.org/plugins/maven-project-info-reports-plugin/)
. The output directory is defined by the property `project.docdir`.

#### `generate.reports.checkstyle`
Generate the Checkstyle analysis report using [Apache Maven Checkstyle Plugin](https://maven.apache.org/plugins/maven-checkstyle-plugin/)
. The output directory is defined by the property `project.docdir`.

#### `generate.reports.pmd`
Generate the Checkstyle analysis report using [Apache Maven PMD Plugin](https://maven.apache.org/plugins/maven-pmd-plugin/)
. The output directory is defined by the property `project.docdir`.

#### `generate.reports.spotbugs`
Generate the Checkstyle analysis report using [SpotBugs Maven Plugin](https://spotbugs.github.io/spotbugs-maven-plugin/)
. The output directory is defined by the property `project.docdir`.

#### `generate.reports`
Enable every `generate.reports.xxx` profile at the same time (just out of convenience)


### OSSRH deployment
This profile (enabled by using `-P ossrh`) generates the artifacts required in order to
deploy the project to the Sonatype Open Source Software Repository Hosting according to
[Sonatype Requeriments](https://central.sonatype.org/pages/requirements.html) and
[Configuring Your Project for Deployment](https://help.sonatype.com/repomanager2/staging-releases/configuring-your-project-for-deployment).

(You would require configuring your local `settings.xml` file regardless)








Dependency versions
---------------------------------------------------------------------------------------------------
Every dependency version is set using a property, so that inherited modules can override
specific versions without change the overall configuration.

```xml
    <!-- dependencies -->
    <slf4j-api.version>1.7.30</slf4j-api.version>
    <junit.version>4.13.1</junit.version>
    <hamcrest.version>2.2</hamcrest.version>
    <log4j.version>2.13.0</log4j.version>
    <assertj.version>3.16.1</assertj.version>

    <!-- build plugins' versions -->
    <maven.compiler.version>3.8.0</maven.compiler.version>
    <maven.compiler.release>11</maven.compiler.release>
    <maven.compiler.source>11</maven.compiler.source>
    <maven.compiler.target>11</maven.compiler.target>
    <maven.surefire.version>2.18.1</maven.surefire.version>
    <maven.failsafe.version>2.18.1</maven.failsafe.version>
    <maven.jacoco.version>0.8.2</maven.jacoco.version>
    <maven.clean.version>3.1.0</maven.clean.version>
    <maven.source.version>3.0.1</maven.source.version>
    <maven.javadoc.version>3.0.0</maven.javadoc.version>
    <maven.surefire-report.version>3.0.0-M5</maven.surefire-report.version>
    <maven.info-report.version>3.1.0</maven.info-report.version>
    <maven.gpg.version>1.5</maven.gpg.version>
    <maven.nexus-staging.version>1.6.7</maven.nexus-staging.version>
    <maven.sonar.version>3.7.0.1746</maven.sonar.version>
    <maven.checkstyle.version>3.1.1</maven.checkstyle.version>
    <maven.pmd.version>3.13.0</maven.pmd.version>
    <maven.spotbugs.version>4.0.4</maven.spotbugs.version>
    <maven.versions.version>2.8.1</maven.versions.version>
```

### Unintended inherited sections
Due to the Maven inheritance mechanism, all sections of this `pom.xml` file are inherited,
included those which have nothing to do with the primary intents of this configuration. At the same
time, some specific sections must be fulfilled to this project be in a public repository, so they
cannot be empty by default.
Because of that, it is strongly recommended overriding the following sections in every `pom.xml`
that use this one as a parent:
- `name`
- `description`
- `url`
- `organization`
- `licenses`
- `issueManagement`
- `developers`
- `scm`

However, in the specific case your project is fully hosted by Github, you may benefit from parts of 
this configuration, since the `url`, `issueManagement` and `scm` would be already prepared. Those
sections are defined using the following properties, which you can redefine:
```xml
    <github.host>github.com</github.host>
    <github.user>${project.organization.name}</github.user>
    <github.project>${project.artifactId}</github.project>
    <github.user.url>https://${github.host}/${github.user}</github.user.url>
    <github.project.url>${github.user.url}/${github.project}</github.project.url>
``` 



Authors
-----------------------------------------------------------------------------------------

- Luis Iñesta Gelabert  |  luiinge@gmail.com

Contributions
-----------------------------------------------------------------------------------------
If you want to contribute to this project, visit the
[Github project](https://github.com/luiinge/maven-java-starter). You can open a new issue / feature
request, or make a pull request to consider. You will be added
as a contributor in this very page.

Issue reporting
-----------------------------------------------------------------------------------------
If you have found any defect in this software, please report it 
in [Github project Issues](https://github.com/luiinge/maven-java-starter/issues). 
There is no guarantee that it would be fixed in the following version but it would 
be addressed as soon as possible.   
 

License
-----------------------------------------------------------------------------------------

```
    MIT License

    Copyright (c) 2020 Luis Iñesta Gelabert - luiinge@gmail.com

    Permission is hereby granted, free of charge, to any person obtaining a copy
    of this software and associated documentation files (the "Software"), to deal
    in the Software without restriction, including without limitation the rights
    to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
    copies of the Software, and to permit persons to whom the Software is
    furnished to do so, subject to the following conditions:

    The above copyright notice and this permission notice shall be included in all
    copies or substantial portions of the Software.

    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
    IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
    FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
    AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
    LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
    OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
    SOFTWARE.
```

