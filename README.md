# CircleCI helper orb

[Orbs](https://circleci.com/docs/2.0/orb-intro/) are reusable snippets of code for CircleCI pipeline definition. This orb is destined to simplify common CICD operations in the landing zone (e.g. authenticating to the artifact registry, pushing and pulling images from it, bumping image tags in `ph-releaser`). 

For a more visual documentation see [private orb registry](https://circleci.com/developer/orbs/orb/pricehubble/ph-circleci-helper)

# Getting started

Adding following snippets in your repo's `.circleci/config.yml` will get you up and going faster with implementing your CICD pipelines.
First the orb must be included with:

```yaml
version: 2.1

orbs:
  # "ph" here is the namespace of the functions imported. To call commands (like a function but for circleci pipelines),
  # you'd then need to use `ph/<command_name>`

  # You can specify the patch version 
  ph: pricehubble/ph-circleci-helper@0.3.1

  # or ideally just pin it to a minor version to get all the patch updates
  ph: pricehubble/ph-circleci-helper@0.3
```

From then on, you can call any commands that are included in the orb. The full list of commands are visible in [src/commands](./src/commands).


---

## Bump

The bump step allows the CircleCI step to change the image tag in the [ph-releaser](https://bitbucket.org/pricehubble/ph-releaser/src/master/) repository. This change will then be picked up by Argocd and deployed (see [linking circleci with argocd](https://pricehubble.atlassian.net/l/cp/e9Jk002Z) for a more detailed view of the interaction).

```yaml
version: 2.1

orbs:
  ph: pricehubble/ph-circleci-helper@0.3.2

jobs:
  build:
    steps:
    # Installs yq which is required by the step
    - ph/install-yq

    # Bump can be called in many ways
    #
    # 1. With all defaults. 
    # The following step will change in ph-releaser the "apps/<< parameters.app >>/<< parameters.env >>/values.yaml" file
    # In that yaml file, the ".image.tag" value will be set to the '${CIRCLE_WORKFLOW_ID}.${CIRCLE_SHA1}' value.
    - ph/bump:
        team: geoinsights
        app: neighbourhood
        env: predev

    # 2. Specify value files and yaml path inside the file
    # The following step will change in ph-releaser the "apps/<< parameters.app >>/<< parameters.env >>/values.lz.yaml" file
    # In that yaml file, the ".neighbourhood.image.tag" value will be set to the '${CIRCLE_WORKFLOW_ID}.${CIRCLE_SHA1}' value.
    - ph/bump:
        team: geoinsights
        app: neighbourhood
        env: predev
        value-file: apps/<< parameters.app >>/<< parameters.env >>/values.lz.yaml
        value-tag-referenc: .neighbourhood.image.tag

    # 3. Specify the image tag to be changed
    # The following step will change in ph-releaser the "apps/<< parameters.app >>/<< parameters.env >>/values.yaml" file
    # In that yaml file, the ".image.tag" value will be set to the ${CIRCLE_SHA1} (commit sha) value.
    - ph/bump:
        team: geoinsights
        app: neighbourhood
        env: predev 
        tag: << pipeline.git.revision >>

workflows:
  build_deploy:
    jobs:
    - build:
        # required as it contains the SSH key with which circleci authenticates to bitbucket
        context: landing-zone-root
```

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
  ph: pricehubble/ph-circleci-helper@0.3

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
  ph: pricehubble/ph-circleci-helper@0.3

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
  ph: pricehubble/ph-circleci-helper@0.3

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
