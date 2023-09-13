# Magical Workflows/Actions

Reusable workflows/actions using [magical_version_bump][mvb_github_link] package to modify your project's version. You can find the package [here][mvb_pub_link].

Refer to the package's documentation for more information.

## Overview

Consider using `v2` and above. Why?

1. `v1` has reusable workflows which do modify but you may need to store the updated yaml file as an artifact and replacing the old one in a new job.
2. `v2` and above includes `actions` you can use in your steps. Reusable workflows also modify and push the changes to your repo.

NB: Reusable workflows must be triggered as a `job` while actions are triggered in `steps`. [Learn more][learn_more_link].

Access documentation about actions at [ACTIONS.md](/ACTIONS.md)

Access documentation about workflows at [WORKFLOWS.md](/WORKFLOWS.md)

**CAUTION**: Be careful when choosing a tag to prevent erroneous bumps/commits/tags

[mvb_github_link]: https://github.com/kekavc24/magical_version_bump
[mvb_pub_link]: https://pub.dev/packages/magical_version_bump
[learn_more_link]: https://dev.to/github/whats-the-difference-between-a-github-action-and-a-workflow-2gba
