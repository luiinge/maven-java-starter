
# Maven Java starter

This artifact is a simple `pom.xml` configuration file that eases the sometimes painful process of
setting up the basic infrastructure for a new Java project.

## Usage

Simply use this artifact as the parent project. Add the following in your `pom.xml`:
```xml
   <parent>
        <groupId>io.github.luiinge</groupId>
        <artifactId>maven-java-starter</artifactId>
        <version>11.0.0</version>
   </parent>
```

If your project already have a parent project, you may modify the parent `pom.xml`, if it is a
viable option. As a last resort, you always can copy the configuration of this artifact and paste
it into yours.



## What includes?

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

## Profiles

### Documentation generation
This profile (enabled using `-P gendoc`) produces a set of artifacts in the
folder `${project.docdir}` (by default `./docs`):
- Javadoc API documentation (only for public members and excluding `*.internal,*.impl` packages)
- JaCoCo (coverage) report
- Surefire (unit test) report
- Failsafe (integration test) report
- Dependencies report

Notice that, using this profile, the above reports are *not* generated in the `site` phase
but as soon as their source data is available.

### OSSRH deployment
This profile (enabled using `-P ossrh`) generates the artifacts required in order to
deploy the project to the Sonatype Open Source Software Repository Hosting according to
[Sonatype Requeriments](https://central.sonatype.org/pages/requirements.html) and
[Configuring Your Project for Deployment](https://help.sonatype.com/repomanager2/staging-releases/configuring-your-project-for-deployment).

(You would require configuring your local `settings.xml` file regardless)

## Dependency versions
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
        <maven.javadoc.version>3.0.0</maven.javadoc.version> <!-- with 3.2.0 doesn't work! -->
        <maven.surefire-report.version>3.0.0-M5</maven.surefire-report.version>
        <maven.info-report.version>3.1.0</maven.info-report.version>
        <maven.gpg.version>1.5</maven.gpg.version>
        <maven.nexus-staging.version>1.6.7</maven.nexus-staging.version>
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
    <github.user>${.organization.name}</github.user>
    <github.project>${project.artifactId}</github.project>
    <github.user.url>https://${github.host}/${github.user}</github.user.url>
    <github.project.url>${github.user.url}/${github.project}</github.project.url>
``` 
