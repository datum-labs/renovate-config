# renovate-config

Organisation-wide Renovate settings for datum-labs. The Mend Renovate app reads `org-inherited-config.json` from this repository and applies it to every repository in the organisation, underneath that repository's own config.

It does two things:

1. Applies Datum's shared dependency update policy from datum-cloud/.github, so routine updates wait on each repository's dependency dashboard and security fixes open a pull request on their own.
2. Turns off onboarding pull requests and makes a per-repository config optional, so every repository is covered without one pull request per repository.

A repository's own `renovate.json` still merges on top of this. The app skips forked repositories unless their own config sets `"forkProcessing": "enabled"`.

The app only notices this file when a commit changes it. If a change does not take effect, push another commit to the file and wait a minute.
