# Releasing

This page explains how pi-packages releases a new version.
Read it before you start a release, and after you merge a pull request that changes the version, because a release is finished only when every step below is done.
The [`/set-up-project`](https://github.com/yahyabedirhan/skills/tree/main/skills/set-up-project) skill wrote it from the maintainer's shared release rules.

## What a release ships

This section names the parts that must all show the new version.

- One version for the whole repository, in the `"version"` field of the root `package.json` and in the two root entries of `package-lock.json`.
  The packages in `packages/<name>/package.json` keep their placeholder version `0.0.0`.
- A tag, `v<version>`, and a [GitHub release](https://github.com/yahyabedirhan/pi-packages/releases) for it.
- A section in `CHANGELOG.md`.

Releases go to GitHub only, and nothing is published to npm or any other registry.

## Release order

This section lists the steps of every release, in order, so that every version has its tag and its GitHub release.

0. Check that the changelog, the root `package.json`, the latest tag and the latest GitHub release all name the same version, and finish any version that is behind first.
1. Open one pull request that sets the version in `package.json` and `package-lock.json` and renames `## Unreleased` in `CHANGELOG.md` to `## <version> (<YYYY-MM-DD>)`.
   Before 1.0, a feature or a breaking change raises the middle number, and anything else raises the last number.
2. Merge it once the maintainer approves.
3. Tag the merged commit as `v<version>` and make its GitHub release, with the changelog section as the notes.
4. Check again that every part names the new version.

## Roles in a release

This section names who does each step.

- The agent picks the version, writes the changelog, opens the release pull request, makes the tag and the GitHub release, and runs the checks.
- The maintainer approves the version and the merge.
