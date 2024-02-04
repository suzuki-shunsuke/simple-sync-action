# simple-sync-action

GitHub Actions to synchronize specific files with the specific branch to prevent tamper

This action is useful for the security of CI.

[action.yaml](action.yaml)

## Motivation

CI has security threats that attackers tamper files and execute malicious codes.
For example, if you execute shell scripts in CI via pull request event, attackers can tamper files in pull requests and execute malicious codes in CI.
So you should prevent these files from being tampered without any review.

This action synchronizes files with the given branch, basically this would be the default branch.
So even if attackers tamer files in pull requests, you can prevent them from being executed without any review.

## :bulb: Prevent workflows from being tampered by `pull_request_target` event

Even if you use this action, it's meaningless if attackers can tamper workflows.
To prevent it, you should consider using `pull_request_target` event instead of `pull_request` event.

Please see [Secure GitHub Actions by pull_request_target](https://dev.to/suzukishunsuke/secure-github-actions-by-pullrequesttarget-641) too.

## How to use

e.g.

```yaml
---
name: test
on: pull_request_target
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@b4ffde65f46336ab88eb53be808477a3936bae11 # v4.1.1
      - uses: suzuki-shunsuke/simple-sync-action@ed6c2035e78b5bd43ae8167446f00755692ca06c # v0.1.0
        with:
          files: |
            # The list of synchronized files and directories
            # comment
            # empty lines are ignored

            # Prevent malicious codes from being executed in CI
            Makefile
            scripts
      - run: make test
```

## Requirements

- bash
- rsync

## What does this action do?

1. Clone the given branch temporarily
2. Synchronize give files by rsync
3. Remove the cloned repository

## Inputs

- `files`: (Required) The list of synchronized files and directories. Empty lines and lines starting with `#` are ignored.

> [!WARNING]
> The action fails if `files` don't exist on the `ref`.

- `ref`: (Optional) The base ref. The default value is `main`

## Outputs

Nothing.

## LICENSE

[MIT](LICENSE)
