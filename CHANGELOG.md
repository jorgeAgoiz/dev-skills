# dev-skills

## 0.2.3

### Patch Changes

- [#8](https://github.com/jorgeAgoiz/dev-skills/pull/8) [`6f5e16d`](https://github.com/jorgeAgoiz/dev-skills/commit/6f5e16dbb0bc32146b94cbd5dca3d27f21922ed8) Thanks [@jorgeAgoiz](https://github.com/jorgeAgoiz)! - Add `AGENTS.md` with repository guidance for AI coding agents: layout, tooling limits, release flow, and `api-to-bruno` safety rules.

## 0.2.2

### Patch Changes

- [#6](https://github.com/jorgeAgoiz/dev-skills/pull/6) [`bb43b69`](https://github.com/jorgeAgoiz/dev-skills/commit/bb43b697197ee291e4b1507f67bfeccc79bfc7b5) Thanks [@jorgeAgoiz](https://github.com/jorgeAgoiz)! - Harden `api-to-bruno` against prompt injection and unverifiable external dependencies. New guardrails: allowlisted Git hosts, pinned refs resolved to commit SHAs, 256 KB per-file cap, subagent-isolated reads of external content, mandatory user confirmation of the inventory before writing files, output isolation from the cloned repo, and no copy of literal values from source files into generated `.bru` requests. See `skills/api-to-bruno/SKILL.md` and `BRUNO.md` for the full security model.

## 0.2.1

### Patch Changes

- [#3](https://github.com/jorgeAgoiz/dev-skills/pull/3) [`1a172d6`](https://github.com/jorgeAgoiz/dev-skills/commit/1a172d605ef7991b72d4ad47967465a93c9a7639) Thanks [@jorgeAgoiz](https://github.com/jorgeAgoiz)! - Fix release workflow to use GITHUB_TOKEN instead of RELEASE_TOKEN

## 0.2.0

### Minor Changes

- [#1](https://github.com/jorgeAgoiz/dev-skills/pull/1) [`1a32fd5`](https://github.com/jorgeAgoiz/dev-skills/commit/1a32fd5305418ec6f85455a8f84ab489e9c97e25) Thanks [@jorgeAgoiz](https://github.com/jorgeAgoiz)! - First release: initial collection of development skills

  - Skill: api-to-bruno - Bruno collection generation from APIs
  - Initial project setup with changesets
  - Automated release workflow with GitHub Actions
  - Updated GitHub Actions to latest versions
