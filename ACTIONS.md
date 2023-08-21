# Magical Actions
Reusable actions that power [Magical Workflows](/WORKFLOWS.md).

## Overview
The available actions include:
* [Extract target](#extract-target) - extracts the [semVer][semver_link] target to bump
* [Extract version](#extract-version) - extracts the current version in a yaml/json file
* [Bump version](#bump-version) - bumps version in yaml file. You need to pass in a desired target.
* [Set version](#set-version) - overwrites current version in yaml file. Just pass in the version.

## Extract target
This action has no optional inputs. You must pass in a trigger for each input which must be a `boolean`.
* `major` - A trigger for the `major` version
* `minor` - A trigger for the `minor` version
* `patch` - A trigger for the `patch` version

Outputs for this action include:
* `target` - A valid semVer target. `major`, `minor` or `patch`

### Usage
This example uses the `contains` to check if labels match requirement for each trigger.
``` yaml

# Extract in a step
- name: 🔍 Extract semVer target
  id: extract_target
  uses: kekavc24/magical_workflows/.github/actions/set_version.yaml@v2.1.0
  with:
    major: ${{contains(github.event.pull_request.labels.*.name, 'major release')}}
    minor: ${{contains(github.event.pull_request.labels.*.name, 'minor release')}}
    patch: ${{contains(github.event.pull_request.labels.*.name, 'patch release')}}

# Use it in next step
- name: 🗣 Echo target
  run: echo "${{ steps.extract_target.outputs.target }}"

```

## Extract version
This action has no optional input. You must pass in the path to your file to extract the version as a `string`.
* `path` - path to your yaml/json file

Outputs for this action include:
* `version` - the value of the first version node in your yaml/json file

### Usage
``` yaml

# Get version in a step
- name: 🔍 Get version
  id: get_version
  uses: kekavc24/magical_workflows/.github/actions/set_version.yaml@v2.1.0
  with:
    path: path-to-my-file

# Use it in next step
- name: 🗣 Echo version
  run: echo "${{ steps.get_version.outputs.target }}

```

## Dart-dependent actions
These next actions require the [`dart sdk`][dart_sdk_link]. Make sure you globally activate it as Dart is required to activate the [magical_version_bump][mvb_github_link] executable.

```yaml

uses: dart-lang/setup-dart@v1
with:
  sdk: "stable"

# You can use any sdk available.
```

## Bump version
This action takes in optional parameters :
* `target` - A [semVer][semver_link] version target. Must be a string. `major`, `minor` or `minor`. 
* `bump-build-number` - Nudges workflow to bump the build number too. Must be a boolean. `true` or `false`
* `path` - Path to yaml. The path must include the file too. Default path is `pubspec.yaml`

You must include at least one of `target` or `bump-build-number`. If none are included, the action terminates with an error.

``` yaml
# Check tags for desired version.

- name: ✨ Bump version
  uses: kekavc24/magical_workflows/.github/actions/bump_version.yaml@v2.1.0
  with:
    target: major # Optional. 
    bump-build-number: true # Optional. Default is false
    path: myPath-to-yaml # Optional. Default is pubspec.yaml

# This example will bump the major version and build number.
```

## Set version
This action takes in these parameters :
* `version` - Version to update to. Must be a string. This parameter is required. 
* `keep-pre` - Keep any pre-release info. Must be a boolean. `true` or `false`
* `keep-build` - Keep any build info. Must be a boolean. `true` or `false`
* `path` - path to yaml. The path must include the file too. Default path is `pubspec.yaml`

You must include `version`. If not included, the action terminates with an error.

If `keep-pre` or `keep-build` is set to true and none is present on the old version, the action terminates with an error.

``` yaml
# Check tags for desired version.

- name: ✨ Set version
  uses: kekavc24/magical_workflows/.github/actions/set_version.yaml@v2.1.0
  with:
    version: 2.0.0 # Required. 
    keep-pre: true # Optional. Default is false
    keep-build: true # Optional. Default is false
    path: myPath-to-yaml # Optional. Default is pubspec.yaml

# This example will set version to 2.0.0 and keep any old pre-release and build info.
```

[semver_link]: https://semver.org/
[dart_sdk_link]: https://github.com/dart-lang/sdk
[mvb_github_link]: https://github.com/kekavc24/magical_version_bump