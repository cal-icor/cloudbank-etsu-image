# cloudbank-etsu-image


### Modifying the new image

The process to modify and push an image to the Google Artifact Registry via the
CI/CD pipeline is located in the [contribution guide](CONTRIBUTING.md)


## About this template repository

This template repository uses the
[jupyterhub/repo2docker-action](https://github.com/jupyterhub/repo2docker-action)
to build a Docker image using the contents of this repo, and pushes it to our
[Google Artifact Registry](https://cloud.google.com/artifact-registry) when
a pull request is merged to `main`.

### The environment

The repo provides a default `environment.yml` conda configuration file for
`repo2docker` to use to define and build a single-user server image. This file
is used to define the python packages that will be installed during the image
build process, either via `conda` or `pip`.

**Note:**
A complete list of configuration files that can be added to the
repository and used by `repo2docker` to build the Docker image can be found in
the [repo2docker documentation](https://repo2docker.readthedocs.io/en/latest/config_files.html#configuration-files).

### Making changes to a single user server image

Once you've created the new image repo from this template, please refer to
[the contribution instructions](CONTRIBUTING.md) located in the repo for
detailed instructions.

### The GitHub Action workflows

This template repository provides GitHub Action workflows that can build
and push the image to Google Artifact Repository when configured, and push a
commit to the [Cal-ICOR Jupyterhub repo](https://github.com/cal-icor/cal-icor-hubs)
repository that modifies `hubploy.yaml` for any hubs using this image with the
new SHA tag.

#### 1. Build and test container image :arrow_right: [test.yaml](https://github.com/cal-icor/cloudbank-etsu-image/blob/main/.github/workflows/build-test-image.yaml)

This workflow is triggered when a pull request is opened against the default
branch (`main`). During PR builds, the image is **only** built and **not**
pushed to the Google Artifact Registry.

Please note that the image will not be built for documentation changes
(markdown files or any graphic images in the `images/` subdirectory).

#### 2. YAML linting :arrow_right: [yaml-lint.yaml](https://github.com/cal-icor/cloudbank-etsu-image/blob/main/.github/workflows/yaml-lint.yaml)

This workflow is triggered when a pull request is opened against the default
branch (`main`). It uses [yamllint](https://yamllint.readthedocs.io/en/stable/)
to check all yaml files in the repo for correctness.

#### 3. **Temporarily disabled:** Test this PR on Binder Badge :arrow_right: [binder.yaml](https://github.com/cal-icor/cloudbank-etsu-image/blob/main/.github/workflows/binder.yaml.disable)

Since our images are typically large and take > 10m to build, this means that
Binderhub builds will currently time out.

#### 4. Build, test and push container image :arrow_right: [build-push-open-pr.yaml](https://github.com/cal-icor/cloudbank-etsu-image/blob/main/.github/workflows/build-push-create-pr.yaml)

After a PR is merged to `main`, this workflow builds the image again, pushes it
to the Google Artifact Registry and then creates a commit that updates the image tag
for any hubs that use this image. That commit is then pushed to the 
[Cal-ICOR Jupyterhub repo](https://github.com/cal-icor/cal-icor-hubs), and you will
then need to manually create a pull requests to merge and deploy the new image.
