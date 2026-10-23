# Changelog

This file lists the changes in each release of pi-packages, with the newest version first.
Each version has a tag and a GitHub release with the same notes.

## Unreleased

## 0.2.0 (2026-10-10)

Adds the `subagent-guard` extension, next to `session-status`.

- New `subagent-guard` extension: keeps the subagents of `@tintinweb/pi-subagents` inside your `tool_call` guardrail.
- Gates `Agent` and `SubagentWorkflow` calls in `tool_call`, child agents included.
- Blocks isolated, worktree and scheduled agent calls, with an actionable reason.
- Resolves an agent type the way pi-subagents loads it: global, then project, by name.
- Asks before an edit or write to pi-subagents config, and blocks it when there is no UI.
- Warns at session start about risky project agent files and `subagents.json` keys.
- Lists `subagent-guard` in `pi.extensions` and the README.

## 0.1.0 (2026-10-10)

First release: the `session-status` extension for the [Pi coding agent](https://pi.dev).

- New `session-status` extension, ported from the claude-mods session-status mod. Its core modules are copied unchanged, with the source commit recorded in `core/readme.md`.
- Registers the status tool with the mod's schema, ids and replies, and adds the mod's instructions as a prompt section in `before_agent_start`.
- Saves each change as a custom session entry and restores it from the branch. A fork starts empty.
- Shows a widget above the editor and a band in the footer.
- Answers decisions and surprises from an overlay, opened with `alt+s` or `/session-status`.
- Adds links from `gh` output and reads link states, effort and ticket counts in the background.
- Tests for the store, the tool, the answers, the view, the overlay and the wiring.
