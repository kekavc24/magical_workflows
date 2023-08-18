# Magical Workflows/Actions
Reusable workflows/actions using [magical_version_bump][mvb_pub_link] package to modify your project's version. You can find the repo [here][mvb_github_link].

Refer to the package's documentation for more info.

## Overview
The available workflows include:
* Bump version - bumps version in yaml file. You need to pass in a desired target.
* Set version - overwrites current version in yaml file. Just pass in the version.

Consider using `v2` which can be integrated as a step rather using `v1`. Why?
1. `v1` is a reusable workflow and you may need to store the updated yaml file as an artifact and replacing the old one in a new job.
2. `v2` is an action and thus can be used as a step and any changes will reflect in the current job calling it.
3. Reusable workflows must be triggered as a `job` while actions are triggered in `steps`. [Learn more][learn_more_link]

Make sure you globally activate the dart sdk in a previous step. Dart is required to activate [magical_version_bump][mvb_pub_link] executable.
```yaml

uses: dart-lang/setup-dart@v1
with:
  sdk: "stable"

# You can use any sdk available.
```

## Bump version
This workflow takes in optional parameters :
* `target` - A [semVer][semver_link] version target. Must be a string. `major`, `minor` or `minor`. 
* `bump-build-number` - Nudges workflow to bump the build number too. Must be a boolean. `true` or `false`
* `path` - Path to yaml. The path must include the file too. Default path is `pubspec.yaml`

You must include at least one of `target` or `bump-build-number`. If none are included, the action terminates with an error.

``` yaml
# Check tags for desired version.

- name: ✨ Bump version
  uses: kekavc24/magical_workflows/.github/actions/bump_version.yaml@v2
  with:
    target: major # Optional. 
    bump-build-number: true # Optional. Default is false
    path: myPath-to-yaml # Optional. Default is pubspec.yaml

# This example will bump the major version and build number.
```

## Set version
This workflow takes in these parameters :
* `version` - Version to update to. Must be a string. This parameter is required. 
* `keep-pre` - Keep any pre-release info. Must be a boolean. `true` or `false`
* `keep-build` - Keep any build info. Must be a boolean. `true` or `false`
* `path` - path to yaml. The path must include the file too. Default path is `pubspec.yaml`

You must include `version`. If not included, the action terminates with an error.

If `keep-pre` or `keep-build` is set to true and none is present on the old version, the action terminates with an error.

``` yaml
# Check tags for desired version.

- name: ✨ Set version
  uses: kekavc24/magical_workflows/.github/actions/set_version.yaml@v2
  with:
    version: 2.0.0 # Required. 
    keep-pre: true # Optional. Default is false
    keep-build: true # Optional. Default is false
    path: myPath-to-yaml # Optional. Default is pubspec.yaml

# This example will set version to 2.0.0 and keep any old pre-release and build info.
```

[mvb_github_link]: https://github.com/kekavc24/magical_version_bump
[mvb_pub_link]: https://pub.dev/packages/magical_version_bump
[semver_link]: https://semver.org/
[learn_more_link]: https://dev.to/github/whats-the-difference-between-a-github-action-and-a-workflow-2gba
