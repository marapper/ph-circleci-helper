# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]
 - Current development changes [ to be moved to release ]

## [0.4.2] - 2024-01-17
 - The `ph/auth-python` step now installs keyrings with the `--break-system-packages` option.

## [0.4.1] - 2023-12-28
 - [TSR-1357](https://pricehubble.atlassian.net/browse/TSR-1357) The `ph/bump` step now preserves whitespace and comments.

## [0.4.0] - 2023-12-12
 - Set docker step default `attest: true`
 - Set bump step default `use-digest: true`

## [0.3.30] - 2023-12-12
 - Fix bug with image tag comment not interpolated in the bump step.

## [0.3.29] - 2023-12-12
 - Bump `ph/cloud-sdk` image to [456.0.0](https://cloud.google.com/sdk/docs/release-notes#45600_2023-12-05)
 - [TSR-1298](https://pricehubble.atlassian.net/browse/TSR-1298) Stabilize image signing by improving error handling for duplicate signatures.
 - Use local docker digest instead of fetching it via the artifact registry
 - Fix issues with gcloud artifact docker images describe that requires an insane amount of permissions in container analysis is enabled
 - Add checks for tag → digest search in the bump step

## [0.3.28] - 2023-11-06
 - [TSR-1236](https://pricehubble.atlassian.net/browse/TSR-1236) Add the option to bump using image digests with the `use-digest: true` parameter.

## [0.3.27] - 2023-11-02
 - [TSR-886](https://pricehubble.atlassian.net/browse/TSR-886) Add the attestation and signature in the push-docker step.

## [0.3.26] - 2023-10-25
 - Add the `node-20-chrome-headless` executor.

## [0.3.25] - 2023-10-24
 - [TSR-1222](https://pricehubble.atlassian.net/browse/TSR-1222) Change default behaviour for the `push-docker` step to also push a rolling tag (defaults to the tag and if no tag is present takes the source branch)

## [0.3.24] - 2023-09-21
 - [TSR-1091](https://pricehubble.atlassian.net/browse/TSR-1091) Add `argocd app wait` in `argocd/sync` to ensure that the pipeline waits for the app to be fully healthy and not just synced.

## [0.3.23] - 2023-09-11
 - Disable the `--grpc-web` flag to attempt to lower `argocd-sync` step flakiness from getting rate-limited. 

## [0.3.22] - 2023-08-29
 - Make the `argocd/bump` step use proper env variable escaping to allow accessing field with [yq reserved characters](https://mikefarah.gitbook.io/yq/v/v3.x/usage/path-expressions#keys-and-values-with-leading-dashes), e.g. `'.serviceAccount."pricehubble.com/wasm_plugin_version"' = abc`

## [0.3.21] - 2023-08-23
 - [TSR-1001](https://pricehubble.atlassian.net/browse/TSR-1001) Make the `argocd/sync` step download the binary from the argo server

## [0.3.18..0.3.20] - 2023-08-18
 - [TSR-933](https://pricehubble.atlassian.net/browse/TSR-933) Update `pricehubble/incluster` executor runner type

## [0.3.17] - 2023-07-18
 - [TSR-832](https://pricehubble.atlassian.net/browse/TSR-832) Allow async `argocd/sync` job

## [0.3.14] - 2023-05-16
 - [TSR-564](https://pricehubble.atlassian.net/browse/TSR-564) Rotate bitbucket cloud public ssh keys

## [0.3.13] - 2023-05-09
 - [TSR-540](https://pricehubble.atlassian.net/browse/TSR-540) Add 'cache' for authentication

## [0.3.12] - 2023-05-09
 - [TSR-529](https://pricehubble.atlassian.net/browse/TSR-529) Add command to fetch ID token for IAP

## [0.3.11] - 2023-04-28
 - [APIS-12](https://pricehubble.atlassian.net/browse/APIS-95) Add command to upload OpenAPI files to developer portal

## [0.3.10] - 2023-04-24
 - Minor fixes to integration tests after [SRE-1624](https://pricehubble.atlassian.net/browse/SRE-1624) (cleanup intermediate credentials)
 - [TSR-413](https://pricehubble.atlassian.net/browse/TSR-413) Add retry on bump push step

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
