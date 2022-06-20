# CircleCI helper orb

[Orbs](https://circleci.com/docs/2.0/orb-intro/) are reusable snippets of code for CircleCI pipeline definition. This orb is destined to simplify common CICD operations in the landing zone (e.g. authenticating to the artifact registry, pushing and pulling images from it, bumping image tags in `ph-releaser`). 


# Getting started

Adding following snippets in your repo's `.circleci/config.yml` will get you up and going faster with implementing your CICD pipelines.
First the orb must be included with:

```yaml
version: 2.1

orbs:
  # "ph" here is the namespace of the functions imported. To call commands (like a function but for circleci pipelines),
  # you'd then need to use `ph/<command_name>`
  ph: pricehubble/ph-circleci-helper@0.1.3
```

From then on, you can call any commands that are included in the orb. The full list of commands are visible in [src/commands](./src/commands).

---

## Authenticate

The orbs contains the following set of authentication functions:

| Command       | Target                   |
|---------------|--------------------------|
| `auth-gcp`    | GCP project              |
| `auth-docker` | docker artifact registry |
| `auth-helm`   | helm artifact registry   |
| `auth-python` | pip artifact registry    |

These will however require some secrets, which are only accessible in the `landing-zone-root` context.  For example, to authenticate to the docker registry:

```yaml
version: 2.1

orbs:
  ph: pricehubble/ph-circleci-helper@0.1.3

jobs:
  build:
    steps:
    - ph/auth-docker

workflows:
  build_deploy:
    jobs:
    - build:
        context: landing-zone-root
```

All commands come with a set of reasonable defaults that are extracted from appropriate circleci context but all parameters are customizable which can be found in the `parameters` section of each command's definition (see [src/commands/auth-docker](./src/commands/auth-docker.yml)).

---
## Install prerequisites

To simplify some operations, the orb also comes with some installation helpers:

| Command                  | Target                                     |
|--------------------------|--------------------------------------------|
| `install-yq`             | https://github.com/mikefarah/yq            |
| `install-docker-compose` | https://github.com/docker/compose/releases |
| `install-helm`           | https://github.com/helm/helm/releases      |

---
## Build and publish docker images

```yaml
version: 2.1

orbs:
  ph: pricehubble/ph-circleci-helper@0.1.3

jobs:
  build:
    executor: ph/cloud-sdk
    parameters:
      app:
        type: string
      team:
        type: string
    steps:
    - checkout
    - ph/auth-docker
    - setup_remote_docker:
        version: 20.10.7
        docker_layer_caching: true
    - run:
        name: Build docker Image
        command: docker build --progress=plain -t launchpad .
    - ph/push-docker:
        team: << parameters.team >>
        app: << parameters.app >>
        source-tag: launchpad:latest

workflows:
  build_deploy:
    jobs:
    - build:
        context: landing-zone-root
        team: growth
        app: launchpad
```

---

## Build and publish pip packages

```yaml
version: 2.1

orbs:
  ph: pricehubble/ph-circleci-helper@0.1.4

jobs:
  publish_pip:
    executor: ph/cloud-sdk
    steps:
    - checkout
    - ph/auth-python
    - run:
        name: Build package
        command: |
          git config --global user.email "ping@pong.com"
          git config --global user.name "Anonymous"
          apt-get install -y python3-venv

          pip install \
            build \
            coverage \
            twine
          python3 setup.py install
          python3 -m build
    - ph/push-python:
        path: /root/project/dist

workflows:
  build_deploy:
    jobs:
    - publish_pip:
        context: landing-zone-root
```
