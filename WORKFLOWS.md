# Magical Workflows
Reusable workflows powered by [Magical Actions](/ACTIONS.md) and [magical_version_bump][mvb_github_link].

## Overview
The available reusable workflows include:
* Bump version in project

## Bump version in project
This workflow `bumps` the version, creates a `tag` and `commits` it to the repo that called it.

The required inputs include:
* `trigger_major` - Must be a `boolean`. Acts as a trigger for `major` version. 
* `trigger_minor` - Must be a `boolean`. Acts as a trigger for `minor` version.
* `trigger_patch` - Must be a `boolean`. Acts as a trigger for `patch` version.
* `bump_build` - Must be a `boolean`. Whether to bump build version.
* `create_tag` - Must be a `boolean`. Whether to create a tag after committing changes made.
* `rebuild_project` - Must be a `boolean`. Whether to generate any build file and commit them too. Uses [`build_runner`][br_link] package.

Optional inputs include:
* `tag_prefix` - Preferred prefix before tag version. Default value is `v`.
* `verify_build` - Must be a `boolean`. Whether to run test verifying the generated files. Default value is `false`
* `path` - Path to your yaml/json file. Default value is `pubspec.yaml`.

You also have to pass in some `secrets` to give workflow access to change and commit them to your repo. These include:
* `ACCESS_TOKEN` - helps bypass repo restrictions on default branch
* `BOT_GPG_KEY` - A GPG key to sign the commits & tags. 

Read more on generating a GPG key [here][gpg_link].

Read more on generating an access token [here][gen_token_link].

Read more on adding secrets to your workflow [here][add_secrets_link].

### Usage
This workflow can only be triggered as job and not a step. [Learn more][learn_more_link]

The example below is triggered based on label present when PR is merged.
``` yaml

jobs:
    update-version:
        needs: verify-merge
        uses: kekavc24/magical_workflows/.github/workflows/bump_version.yaml@v2.1.0
        with:
            trigger_major: ${{contains(github.event.pull_request.labels.*.name, 'major release')}}
            trigger_minor: ${{contains(github.event.pull_request.labels.*.name, 'minor release')}}
            trigger_patch: ${{contains(github.event.pull_request.labels.*.name, 'patch release')}}
            bump_build: true
            create_tag: true
            tag_prefix: ''
        secrets:
            ACCESS_TOKEN: ${{ secrets.TOKEN }}
            BOT_GPG_KEY: ${{ secrets.BOT_GPG_KEY }}

# Will bump the target that is true between major, minor & patch.
# Also bumps the build-number
# Creates a tag and pushes it to the repo

```

The commit in your repo will have :
1. Name of Author linked to GPG key
2. Commit message

![image](/assets/commit_snip.png)

The corresponding tag will look like 👇🏼 depending on the prefix:

![image](/assets/tag_snip.png)

[mvb_github_link]: https://github.com/kekavc24/magical_version_bump
[add_secrets_link]: https://docs.github.com/en/actions/security-guides/encrypted-secrets
[br_link]: https://pub.dev/packages/build_runner
[gen_token_link]: https://docs.github.com/en/enterprise-server@3.6/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens
[gpg_link]: https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key
[learn_more_link]: https://dev.to/github/whats-the-difference-between-a-github-action-and-a-workflow-2gba
