# software-deployment-template

A template repository for the deployment of `spack`-based software

> [!NOTE]
> Feel free to replace this README with information on the repository once the TODOs have been ticked off.

## Things TODO to get your software deployed

### Settings

#### Repository Settings

Relevant branch protections should be set up on `main` and other important branches.

#### Repository Secrets/Variables

There are a few secrets and variables that must be set at the repository level.

##### Repository Variables

* `CONFIG_VERSIONS_SCHEMA_VERSION`: Version of the [`config/versions.json` schema](https://github.com/ACCESS-NRI/schema/tree/main/au.org.access-nri/model/deployment/config/versions) used in this repository
* `SPACK_YAML_SCHEMA_VERSION`: Version of the [ACCESS-NRI-style `spack.yaml` schema](https://github.com/ACCESS-NRI/schema/tree/main/au.org.access-nri/model/spack/environment/deployment) used in this repository
* `RELEASE_DEPLOYMENT_TARGETS`: Space-separated list of deployment targets when doing release deployments. These are often the names of [keys under the `deployment` key of `build-cd`s `config/settings.json`](https://github.com/ACCESS-NRI/build-cd/blob/09cdf100eefc58f06900e8e9145e77b4caf5a39d/config/settings.json#L3), such as `Gadi` or `Setonix`. As noted [below](#environment-secretsvariables), it is the same as the GitHub Environment name. For example: `Gadi Setonix`
* `PRERELEASE_DEPLOYMENT_TARGETS`: Space-separated list of deployment targets when doing prerelease deployments, similar to the above. For example: `Gadi Setonix` - note the lack of a `Prerelease` specifier!

#### Environment Secrets/Variables

GitHub Environments are sets of variables and secrets that are used specifically to deploy software, and hence have more security requirements for their use.

Currently, we have two Environments per deployment target - one for `Release` and one for `Prerelease`. Our current list of deployment targets and Environments can be found in this [deployment configuration file in `build-cd`](https://github.com/ACCESS-NRI/build-cd/blob/main/config/deployment-environment.json).

In order to deploy to a given deployment target:

* Environments with the name of the deployment target and the type must be created _in this repository_ and have the associated secrets/variables set ([see below](#environment-secrets))
* There must be a `Prerelease` Environment associated with the `Release` Environment. For example, if we are deploying to `SUPERCOMPUTER`, we require Environments with the names `SUPERCOMPUTER Release`, `SUPERCOMPUTER Prerelease`.

When setting the environment up, remember to require sign off by a member of ACCESS-NRI when deploying as a `Release`.

Regarding the secrets and variables that must be created:

##### Environment Secrets

* `HOST`: The deployment location SSH Host
* `HOST_DATA`: The deployment location SSH Host for data transfer (may be the same as `HOST`)
* `SSH_KEY`: A SSH Key that allows access to the above `HOST`/`HOST_DATA`
* `USER`: A Username to login to the above `HOST`/`HOST_DATA`

##### Environment Variables

* `DEPLOYED_MODULES_DIR`: Directory that will contain the modules created during the installation of the model. This can be virtual modules created by a [`.modulerc` file](https://github.com/ACCESS-NRI/build-cd/tree/main/tools/modules) in the directory.
* `DEPLOYMENT_TARGET`: Name of the deployment target. It is exported to the deployment target and used for variations in `spack.yaml` build processes - seen most prominently in mutually-exclusive 'when' clauses like `spack.definitions[].when = env['DEPLOYMENT_TARGET'] == 'gadi'`. Also used for logging purposes.
* `SPACK_INSTALLS_ROOT_LOCATION`: Path to the directory that contains all versions of a deployment of `spack`. For example, if `/some/apps/spack` is the `SPACK_INSTALLS_ROOT_LOCATION`, that directory will contain directories like `0.20`, `0.21`, `0.22`, which in turn contain an install of `spack`, `spack-packages` and `spack-config`
* `SPACK_YAML_LOCATION`: Path to a directory that will contain the `spack.yaml` from this repository during deployment
* (Optional) `SPACK_INSTALL_ADDITIONAL_ARGS`: Additional flags outside of `--fresh --fail-fast` to add to the `spack install` command. For advanced users who need to tailor the installation options in their repository.

### File Modifications

#### In `config/versions.json`

* `.spack` must be given a version. For example, it will clone the associated `releases/vVERSION` branch of `ACCESS-NRI/spack` if you give it `VERSION`.
* `.spack-packages` should also have a CalVer-compliant tag as the version. See the [associated repo](https://github.com/ACCESS-NRI/spack-packages/tags) for a list of available tags.

#### In `spack.yaml`

> [!IMPORTANT]
> Unlike Model Deployment Repositories (and [the template](https://github.com/ACCESS-NRI/model-deployment-template) they are based on), you can have multiple `spack.yaml` manifests in one repository. Just remember to demarcate them by directory name - eg. `TOOL1/spack.yaml`, `TOOL2/spack.yaml`, etc.
