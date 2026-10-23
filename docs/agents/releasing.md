# Releasing

This page explains how this project releases a new version, from choosing the version number to checking that the release is complete.
Everyone who releases the project reads it, people and agents alike.
Read it before you start a release, and again after you merge a pull request that changes the version, because the release is not finished until every step below is done.
The `/set-up-project` skill wrote this page.
It started from the release rules that the maintainer shares across all of their projects, and added what this project does in its own way.
When this page and the shared rules disagree, follow this page, and the next `/set-up-project` run brings the two back in line.

## What a release covers

A release here versions the whole repository, not a single package.
The repository holds independent Pi packages, and one version, one tag and one GitHub release cover all of them at once.
The project publishes to GitHub only, so people install a release from a clone checked out at its tag, as the [README](../../README.md) explains.
Nothing goes to npm or any other registry yet.

## Version files

A release sets the new version in this file:

- `package.json` at the repository root holds the version in its `"version"` field.

The packages in `packages/<name>/package.json` keep the version `0.0.0`, and a release does not change them.
That version and their `-local` names are private placeholders until publication is approved, as the [README](../../README.md) says.

## Release order

Every release runs these steps in this order, so that each version has its tag and its GitHub release before anything else depends on it.

0. Start by catching up.
   Read what each part of the project says the newest version is:

   ```sh
   head -20 CHANGELOG.md | grep -m1 '^## [0-9]'
   node -p "require('./package.json').version"
   git fetch --tags origin && git describe --tags --abbrev=0 origin/main
   gh release view --json tagName -q .tagName
   ```

   When one part is behind the others, finish that version first, from the first step it is missing, before you start a new version.
1. Open one pull request that sets the new version and the changelog.
   List the commits since the last tag with `git log $(git describe --tags --abbrev=0)..origin/main --oneline`, and choose the version from them by the version rule below.
   Tell the maintainer the version before anything else, so they can stop the release.
   Set it in `package.json`, and rename `## Unreleased` in `CHANGELOG.md` to `## <version> (<YYYY-MM-DD>)`.
   Run `npm test`, `npm run check` and `npm run test:packages` before the commit, as `AGENTS.md` asks for every commit.
2. Merge the pull request once the maintainer approves it.
3. Tag the merged commit as `v<version>`, and make a GitHub release for that tag.
   The tag is annotated, and its message names the packages the release changes, such as `subagent-guard and session-status v0.2.0`.
   The release title is `<version>: <what it adds>`, such as `0.2.0: subagent-guard`.
   The release notes are the changelog section of that version, followed by a `**Full Changelog**` link that compares the previous tag with the new one.
   No workflow does this step, so the agent runs `git tag -a`, `git push origin v<version>` and `gh release create`.
4. Check the release by running the reads from step 0 again.
   Each read names the new version.

A version is complete when the changelog, `package.json`, the tag and the latest GitHub release all name it.
A release ends at step 4, not at the merge.

## Version rule

- Before version 1.0, a new feature or a breaking change raises the middle number and resets the last number to zero.
  Any other change raises the last number.
- From version 1.0, a breaking change raises the first number.
- A published version is never changed or reused.
  When a published version has a mistake, release a new patch version that fixes it.

A new extension counts as a new feature, as it did for `0.2.0`, which added `subagent-guard`.

## Changelog

- The changelog is `CHANGELOG.md` at the root of the repository, with the newest version first, written for the people who use the packages.
- Each pull request that makes a change users can see adds its line under `## Unreleased` at the top of the changelog.
- Each line names the package it changes, because one release covers every package.

## Roles in a release

- The agent runs these steps: choosing the version, writing the changelog, opening the release pull request, tagging, making the GitHub release and running the checks.
- The maintainer runs these steps: approving the version and the merge.

## Gaps

These parts of the release lifecycle are not settled yet, and each one names where it shows.

- **No changelog yet.**
  The repository has no `CHANGELOG.md`, and the notes of `v0.1.0` and `v0.2.0` live only in their GitHub releases.
  The shared release rules expect a changelog, so the next release pull request creates `CHANGELOG.md`, with the notes of both releases as its first two sections, before it adds the new version.
  Until then, the changelog read in step 0 prints nothing.
- **Four merged pull requests since `v0.2.0`.**
  Pull requests #3 to #6 merged after the `v0.2.0` tag, and no version covers them yet.
  They add the session status sidebar, split the extensions into independent packages, add the sidebar resize keys and align the sidebar controls with its edge.
  Source: `git log --first-parent v0.2.0..origin/main`.
- **The version bump rode in a feature commit before.**
  For `0.2.0`, the commit `document and list subagent-guard` raised the version inside the feature's own pull request.
  This page follows the shared rule of one release pull request for the version and the changelog instead.
- **GitHub releases came after the tags.**
  The tags `v0.1.0` and `v0.2.0` date from 2026-10-10, and their GitHub releases from 2026-10-23.
  From now on, step 3 makes the tag and the release together.
- **Registry publication is undecided.**
  The [README](../../README.md) says that registry names, release versions and publication need a separate decision.
  When the maintainer approves npm publishing, this page gains a publish step after step 3, with the shared npm rules: the agent prepares the publish workspace, and the maintainer runs `npm login` and `npm publish` with their two-factor code.
  That decision also settles whether each package gets its own version, which changes the version files, the tag names and the release titles above.
