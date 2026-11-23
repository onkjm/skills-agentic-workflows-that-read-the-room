---
name: update-github-info
description: Keep Mona's GitHub Info page current with practical, source-backed updates.
intent: Keep Mona's GitHub Info page current with concise, practical GitHub guidance.
on:
  schedule: daily
  workflow_dispatch:
  skip-if-match: 'is:pr is:open in:body "gh-aw-workflow-id: update-github-info"'
permissions:
  contents: read
  copilot-requests: write
strict: true
engine:
  id: copilot
  copilot-sdk: true
tools:
  bash: [cat]
  edit: true
  web-fetch: {}
network:
  allowed:
    - github.blog
    - github.com
safe-outputs:
  create-pull-request:
    allowed-files:
      - site/content/github-info.md
    max: 1
    draft: false
---

# Update GitHub Info

Read `notes/mona-notes.md` and the current `site/content/github-info.md`. Use
`web-fetch` to read both https://github.blog/latest/ and
https://github.blog/changelog/.

Find recent, verified developments that help developers learn GitHub faster
and fit Mona's existing editorial angle. Update `site/content/github-info.md`
with concise, practical guidance; preserve its structure and existing useful
content. Cite the relevant GitHub Blog or Changelog source with a direct link
whenever you use it. Do not add unsupported claims or unrelated news.

If required files or either source cannot be accessed, do not guess; report the
task as incomplete. If there is no useful new information or no meaningful
content change to make, call `noop` with a brief reason and do not open a PR.

When the page changes, modify only `site/content/github-info.md` and use the
configured `create-pull-request` safe output to open one non-draft pull request
for Mona to review. Summarize the changes and include their source links in the
PR description. Do not open a second PR while this workflow already has an open
PR. Do not write directly to the repository's main branch.
