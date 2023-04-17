# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]
 - Current development changes [ to be moved to release ]

## [0.3.9] - 2023-04-17
 - [TSR-441](https://pricehubble.atlassian.net/browse/TSR-441) Fix bump step pull when branch has diverged

## [0.3.8] - 2023-02-15
 - [SRE-1603](https://pricehubble.atlassian.net/browse/SRE-1603) Improve argocd sync step and add integration tests 

## [0.3.7] - 2023-02-06
 - Correct typo on convenience image naming 

## [0.3.6] - 2023-02-06
 - [SRE-1579](https://pricehubble.atlassian.net/browse/SRE-1579) Enrich executors with convenience images

## [0.3.5] - 2023-02-01
 - [SRE-1565](https://pricehubble.atlassian.net/browse/SRE-1565) Improve documentation and test coverage

## [0.3.4] - 2023-01-24
 - [SRE-1543](https://pricehubble.atlassian.net/browse/SRE-1543) Fix git change detection for the bump step

## [0.3.3] - 2023-01-24
 - [SRE-1374](https://pricehubble.atlassian.net/browse/SRE-1374) Bump step without changes no longer exits with non 0 code

## [0.3.2] - 2022-11-25
 - [SRE-1337](https://pricehubble.atlassian.net/browse/SRE-1337) Fix intermittent failures for the bump step 

## [0.3.1] - 2022-11-09
 - Removes unused GOOGLE_APPLICATION_CREDENTIALS for the `push-python` step

## [0.3.0] - 2022-09-28
 - Implement nodeJS artifact registry authentication [SRE-1088](https://pricehubble.atlassian.net/browse/SRE-1088)

## [0.2.3] - 2022-09-01
 - Pin keyring pip versions to `keyring==23.8.2` and `keyrings.google-artifactregistry-auth==1.0.0` 

## [0.2.2] - 2022-08-28
 - Add `[skip ci]` to bump step to avoid triggering `ph-releaser`'s CICD uneccessarily [SRE-1041](https://pricehubble.atlassian.net/browse/SRE-1041)

## [0.2.1] - 2022-08-17
 - Fix pip authentication with workload identity federation 

## [0.2.0] - 2022-08-17
 - Implement workload identity federation authentication [SRE-1012](https://pricehubble.atlassian.net/browse/SRE-1012)

## [0.1.8] - 2022-06-20
 - Initial Release
