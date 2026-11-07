# renovate-config

Organisation-wide Renovate settings for datum-labs. The Mend Renovate app reads `org-inherited-config.json` from this repository and applies it to every repository in the organisation, underneath that repository's own config.

It does two things:

1. Applies Datum's shared dependency update policy from datum-cloud/.github, so routine updates wait on each repository's dependency dashboard and security fixes open a pull request on their own.
2. Turns off onboarding pull requests and makes a per-repository config optional, so every repository is covered without one pull request per repository.

3. Merges low-risk updates without a person: patch, pin and digest updates, lock file maintenance, and minor updates to development-only npm dependencies (not 0.x). They open without waiting on the dashboard and Renovate merges them once every check on the branch is green and the pull request is approved. A new version waits three days first, except a security fix. Majors and runtime minors still wait on the dashboard and a reviewer. fleet keeps human review for everything.

Renovate merges these itself rather than arming GitHub auto-merge. The datum-labs ruleset requires an approval but no status checks, so GitHub auto-merge would merge on the approval alone, before CI finishes. Renovate waits for every check, refuses a branch someone else has pushed to, and treats a repository with no CI as never green. The approval comes from the Mend Renovate Approve app (`renovate-approve[bot]`), which approves only Renovate pull requests set to auto-merge. A repository whose CODEOWNERS covers the changed files still needs a code owner's approval, which the app cannot give.

A repository's own `renovate.json` still merges on top of this. The app skips forked repositories unless their own config sets `"forkProcessing": "enabled"`.

The app only notices this file when a commit changes it. If a change does not take effect, push another commit to the file and wait a minute.
