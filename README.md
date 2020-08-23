Maven Java starter
===================================================================================================

This artifact is a simple `pom.xml` configuration file that eases the sometimes painful process of
setting up the basic infrastructure for a new Java project.


Usage
---------------------------------------------------------------------------------------------------

Simply use this artifact as the parent project. Add the following in your `pom.xml`:
```xml
   <parent>
        <groupId>io.github.luiinge</groupId>
        <artifactId>maven-java-starter</artifactId>
        <version>11.1.0</version>
   </parent>
```

If your project already have a parent project, you may modify the parent `pom.xml`, if it is a
viable option. As a last resort, you always can copy the configuration of this artifact and paste
it into yours.



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
    - Documentation generation
    - OSSRH deployment
    - SonarCloud analysis
    - Other static analysis tools (Spotbugs, Checkstyle, PMD)
    - Obsolete dependencies analysis 


Profiles
---------------------------------------------------------------------------------------------------

### Documentation generation
This profile (enabled by using either `-P generate.reports` or `-Dgenerate.reports`) produces 
a set of artifacts in the folder `${project.docdir}` (by default `./target/site`):
- Javadoc API documentation (only for public members and excluding `*.internal,*.impl` packages)
- JaCoCo (coverage) report
- Surefire (unit test) report
- Failsafe (integration test) report
- Dependencies report
- Obsolete dependencies report 

Notice that, using this profile, the above reports are *not* generated in the `site` phase
but as soon as their source data is available.

### OSSRH deployment
This profile (enabled by using `-P ossrh`) generates the artifacts required in order to
deploy the project to the Sonatype Open Source Software Repository Hosting according to
[Sonatype Requeriments](https://central.sonatype.org/pages/requirements.html) and
[Configuring Your Project for Deployment](https://help.sonatype.com/repomanager2/staging-releases/configuring-your-project-for-deployment).

(You would require configuring your local `settings.xml` file regardless)


### SonarCloud analysis
This profile (enabled by using `-P sonar`) will perform a 
[SonarCloud](https://sonarcloud.io/projects) scan actions after completing the `verify` goal (since
test results are part of the source data). You can configure the server connection properties using
the following properties (although is pre-configured with the usual values):

```
    <sonar.projectKey>${project.organization.name}_${project.artifactId}</sonar.projectKey>
    <sonar.organization>${project.organization.name}</sonar.organization>
    <sonar.host.url>http://sonarcloud.io</sonar.host.url>
```
Tips:
- You may want to specify the code branch by adding the argument `'-Dsonar.branch.name=...`.  
- You should establish the environment variable `SONAR_TOKEN` (different for each project).
- You can include the results of other static analysis in your Sonar report by enabling the specific
profile along with the `sonar` profile


### Other static analysis tools

This profile (enabled by using `-P check.code` or `-Dcheck.code`) will perform static analysis using 
different tools. See the section [Static analysis tools](#static-analysis-tools) for detailed information. 


### Obsolete dependency analysis
This profile (enabled by using `-P check.versions` or `-Dcheck.versions` ) will show which dependencies
have newer versions available.  



Static analysis tools
---------------------------------------------------------------

When using either `check.code` or `generate.reports` profiles, some static analysis tools
will be used. 

By default, each of them is enabled. In order to disable this analysis, set the property 
`<tool>.skip` to `true` either in the `properties` section or using `-D<tool>.skip`.
 
### Checkstyle
This option will perform a 
[Checkstyle](https://checkstyle.sourceforge.io/) analysis. By default it will use the `google_checks`
rule set, but you can change it using the property `maven.checkstyle.config`:

```
<maven.checkstyle.config>google_checks.xml</maven.checkstyle.config>
```

Disabled with property `checkstyle.skip`.

### PMD
This option will perform a [PMD](https://pmd.github.io/) analysis.
It runs the Copy-Paste Detector (CPD) as well.

Disabled with property `pmd.skip`.

### Spotbugs
This option will perform a [Spotbugs](https://spotbugs.github.io/) analysis.

Disabled with property `spotbugs.skip`.




Dependency versions
---------------------------------------------------------------------------------------------------
Every dependency version is set using a property, so that inherited modules can override
specific versions without change the overall configuration.

```xml
    <!-- dependencies -->
    <slf4j-api.version>1.7.30</slf4j-api.version>
    <junit.version>4.13</junit.version>
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

