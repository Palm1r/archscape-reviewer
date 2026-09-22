# archscape-viewer

What a pull request changes, as a picture in one comment: the touched
classes with their changed members, the arrows the change made, and
under it the modules touched, the changed files and every new direction
between modules - the first time one module reaches another.

Nothing leaves your repository. The picture is drawn on your own runner,
committed to a branch of your own repository (`archscape/pictures`), and
the comment shows it from there. Private repositories included.

## Install

```yaml
# .github/workflows/archscape.yml
name: archscape
on:
  pull_request:
    types: [opened, synchronize, reopened]
  issue_comment:
    types: [created]
permissions:
  contents: write
  pull-requests: write
jobs:
  review:
    if: github.event_name == 'pull_request' || (github.event.issue.pull_request && contains(github.event.comment.body, '@archscape-reviewer'))
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - uses: Palm1r/archscape-viewer@main
        with:
          token: ${{ github.token }}
```

A push to the pull request redraws the comment in place. Saying
`@archscape-reviewer` in a comment draws it again on demand.

## Inputs

| input | default | what |
|---|---|---|
| `token` | - | `github.token` is enough: contents and pull requests, write |
| `branch` | `archscape/pictures` | the branch the pictures are committed to |

## What it reads

17 languages through tree-sitter - C++, Java, Go, Rust, PHP, C#, Kotlin,
Swift, Python, TypeScript, JavaScript, QML and more. The range is
`merge-base..head` of the pull request. Files no class stands in - docs,
build files - are counted, not drawn.

## Not in this version

No rules and no red check: the reviewer informs, it does not judge.

## Licence

This repository - the action manifest and this page - is MIT. The
archscape engine inside the image `ghcr.io/palm1r/archscape-reviewer`
is pre-release software: each image carries the day it stops running,
and a newer image is published before then.
