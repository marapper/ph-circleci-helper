# CircleCI helper orb

This orb is destined to simplify common CICD operations in the landing zone (e.g. authenticating to the artifact registry, pushing and pulling images from it, bumping image tags in `ph-k8s-releaser`).

## References

- [CircleCI Orb Authoring guide](https://circleci.com/docs/2.0/orb-intro/#section=configuration) 
- [GCP CLI Orb](https://github.com/CircleCI-Public/gcp-cli-orb/blob/master/src/commands/initialize.yml) used as a reference 
- [Yaml Lint reference](https://yamllint.readthedocs.io/en/latest/) for dealing with the CI 

### Repo structure

```
.
├── CHANGELOG.md                # changelog to be updated manually on release
├── Dockerfile                  # dockerfile used for integration testing
├── LICENSE                     
├── README.md                   
└── src
    ├── @orb.yml                # Chart.yaml equivalent for orbs
    ├── commands                # Contains commands (https://circleci.com/docs/2.0/configuration-reference/#commands-requires-version-21)
    │   ├── auth.yml            #   commands are named after the file name, auth.yml => ph-circleci-helper/auth
    │   ├── bump.yaml
    │   ├── install-yq.yml
    │   └── push.yml
    ├── examples                # Examples to be displayed on the orb registry (does not apply for private registries)
    ├── executors               # Contains executors (https://circleci.com/docs/2.0/configuration-reference/#executors-requires-version-21)
    │   └── cloud-sdk.yml       #   executors can be called with the file name, cloud-sdk.yml => ph-circleci-helper/cloud-sdk
    ├── jobs                    # Contains parametrized jobs (https://circleci.com/docs/2.0/configuration-reference/#jobs)
    ├── scripts                 # Contains shell scripts (useful for longer bash scripts and shellcheck) 
    └── tests                   # Contains BATS unit tests
```

### Local development guide

- Checkout the `alpha` branch 
- Pack the orb (groups yaml files in `src/` into a single yaml) with `circleci orb pack src/ > orb.yml` 
- Validate the orb with `circleci config validate --org-slug bitbucket/pricehubble orb.yml` 
- Commit changes which will lint the chart, publish it with a dev tag and then run integration tests defined in `.circle/config.yml` 

### How to Publish

* Create and push a branch with your new features.
* When ready to publish a new production version, create a Pull Request from _feature branch_ to `main`.
* The title of the pull request must contain a special semver tag: `[semver:<segment>]` where `<segment>` is replaced by one of the following values.

| Increment | Description|
| ----------| -----------|
| major     | Issue a 1.0.0 incremented release|
| minor     | Issue a x.1.0 incremented release|
| patch     | Issue a x.x.1 incremented release|
| skip      | Do not issue a release|

Example: `[semver:major]`

* Squash and merge. Ensure the semver tag is preserved and entered as a part of the commit message.
* On merge, after manual approval, the orb will automatically be published to the Orb Registry.

