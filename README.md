# GitHub Actions: Copy to Another Repository

`Copy-to-Another-Repository` is reusable workflow to copy selected file to another repository and create Pull Request (PR) to merge.

## Usage

Create GitHub Actions like as follows.
See [action.yml](https://github.com/sator-imaging/Copy-to-Another-Repository/blob/main/action.yml) for further details.

```yaml   {% raw %}
name: GitHub-Actions-Name

on:
  workflow_dispatch: # Allows you to run this workflow manually from the Actions tab
  push:
    branches: [ main ]
    paths:
      - 'README.md'  # limit trigger to the target-filepath.
                     # It's not elegant to set same value in two options but
                     # there is no way to retrieve this value in composite action.

jobs:
  main:
    runs-on: ubuntu-latest
    steps:
      - uses: sator-imaging/Copy-to-Another-Repository@v1
        with:

          # required parameters
          target-filepath: 'README.md'      # file path(s) to copy
          output-branch: 'main or master'   # branch name to create pull request
          output-repo: 'Owner/AnotherRepository' # name of org/repo to copy file(s) and open PR
          git-token: ${{ secrets.TOKEN_TO_ACCESS_OUTPUT_REPO }}
          
          # optional parameters and default values
          target-repo: 'AnotherOwner/AnotherRepository' # name of remote org/repo to copy file(s) from
          commit-message-prefix: '[actions] '   # followed by source repository and file name
          output-directory: "${{ github.event.repository.name }}"   # copy file into sub directory
          pr-branch-prefix: "actions/${{ github.event.repository.name }}"   # branch name prefix followed by date and time
          pr-title: "GitHub Actions: ${{ github.event.repository.name }}"   # followed by source repository and file name
          pr-message: "${{ github.workflow }} on ${{ github.server_url }}/${{ github.repository }}"   # followed by action repository
          git-name: "github-actions[bot]"   # your name can be set by ${{ github.actor }}
          git-email: 'github-actions[bot]@users.noreply.github.com'   # associated user icon is shown in commit page

# {% endraw %}
```

## Learning Resources

- [Metadata syntax for GitHub Actions](https://docs.github.com/en/actions/creating-actions/metadata-syntax-for-github-actions)
  - [`runs` for composite actions](https://docs.github.com/en/actions/creating-actions/metadata-syntax-for-github-actions#runs-for-composite-actions)
- [Contexts](https://docs.github.com/en/actions/learn-github-actions/contexts)
- [Environment variables](https://docs.github.com/en/actions/learn-github-actions/environment-variables)
  - [Default environment variables](https://docs.github.com/en/actions/learn-github-actions/environment-variables#default-environment-variables)
- [Webhook events and payloads](https://docs.github.com/en/developers/webhooks-and-events/webhooks/webhook-events-and-payloads)
- [Workflow commands for GitHub Actions](https://docs.github.com/en/actions/using-workflows/workflow-commands-for-github-actions)

## Copyright

Copyright &copy; 2022 Sator Imaging, all rights reserved.
