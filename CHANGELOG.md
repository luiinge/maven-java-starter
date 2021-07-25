Changelog
====================================================================================================
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org).


[11.3.0]
----------------------------------------------------------------------------------------------------
**Release date:** 2021-07-25
### Changed:
- Properties `maven.compiler.releas`, `maven.compiler.source` and `maven.compiler.target` inherit value from `java.version`
### Added:
- Profiles `check.versions`, `generate.reports.versions`, and `ossrh` now check vulnerabilities in 
dependencies according to the [National Vulnerability Database](https://nvd.nist.gov/)


[11.2.0]
----------------------------------------------------------------------------------------------------
**Release date:** 2020-12-13
### Security:
- Bump junit 4.13 to 4.13.1 due to a [low severity vulnerability](https://github.com/advisories/GHSA-269g-pwp5-87pp)
### Changed:
- Preset property `maven.javadoc.failOnError` to `false` in order to prevent builds failing because 
the Javadoc executable was not able to be found
### Added:  
- Profiles `check.code` and `generate.reports` offer variants to use only a specific tool 
(prevent from downloading unnecessary plugins)
  

[11.1.0]
----------------------------------------------------------------------------------------------------
**Release date:** 2020-08-23
### Added:
- New profile `sonar` to perform analysis against a [SonarCloud](https://sonarcloud.io/projects) server
- New profile `check.code` to perform static analysis
- New profile `check.versions` to check obsolete dependencies

[11.0.0]
----------------------------------------------------------------------------------------------------
**Release date:** 2020-08-04

- Initial release for Java 11.
