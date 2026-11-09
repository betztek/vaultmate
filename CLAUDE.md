# CLAUDE.md

Personal fork of [Vankir/vaultmate](https://github.com/Vankir/vaultmate) (Flutter, GPLv3).
This file lives on `jb` only. It must never appear in an upstream-bound branch.

## Branch model

| Branch | Purpose | Rules |
|---|---|---|
| `main` | Mirrors upstream `Vankir/vaultmate` `main`. | No direct commits. Only fast-forward from upstream. |
| `feat/<name>` | One focused, upstream-bound change. | Cut from `main`. No CLAUDE.md, no personal tweaks, no unrelated cleanup. |
| `jb` | Personal integration branch. | `feat/*` branches merged in, plus personal tweaks. Never the base of a `feat/*` branch. |

- Before starting a `feat/*` branch, sync `main` with upstream first.
- Before handing off a `feat/*` branch, check `git diff main...feat/<name>` contains nothing personal
  (CLAUDE.md, local config, tweaks from `jb`).
- Upstream's own docs (constitution principle IV, RELEASE_PROCESS.md) describe a `develop` branch and
  `feature/*` / `bugfix/*` names, but upstream has no `develop` branch and merges PRs straight into
  `main`. Follow actual practice (PRs target `main`) and mention the branch naming if the
  maintainer asks.

## Upstream etiquette

- Upstream uses spec-kit: `.specify/`, `specs/`, `.claude/skills/speckit-*`.
- Read `.specify/memory/constitution.md` before any upstream work and follow it.
- Features go through spec-kit (`/speckit-specify` → `/speckit-plan` → `/speckit-tasks` → `/speckit-implement`).
  Small fixes don't need a spec.
- Commit messages: `feat:` / `fix:` / `chore:` / `docs:` (constitution, RELEASE_PROCESS.md).
- Constitution points that commonly bite a contributor PR:
  - Bug fixes need a regression test that fails before the fix and passes after.
  - Features must work on both iOS and Android, or the PR must call out the platform gap and fallback.
  - Changes to task read/write logic must consider both home-screen widget code paths.
  - No new dependencies without a concrete, current justification; stay within `flutter_bloc`/`provider`.
  - No analytics/telemetry; anything sending vault content off-device must be opt-in.
  - Parser/serializer changes must stay compatible with Obsidian Tasks / NoteTask / Reminder formats.
- **I open PRs myself in the GitHub UI.** Do not create PRs. Draft the PR title and description for
  me (what changed, why, linked issue, how it was tested, any constitution deviation called out).

## Testing discipline

- Red-green TDD is mandatory for any logic change:
  1. Write the test.
  2. Run it and show me it **fails for the expected reason** (paste the failure).
  3. Implement until it passes.
- Never edit or delete an existing test to make it pass without telling me why first.
- Run the full `flutter test` before every push. Report the pass/skip/fail count.
- Baseline on `main` (2026-11-09, Flutter 3.44.7 / Dart 3.12.2): 311 passed, 3 skipped
  (`test/src/screens/inbox_tasks/main_messages_test.dart`, marked `skip: true` upstream), 0 failed.

## Working agreements

- Keep each change scoped to what I asked. Note other problems you find; don't fix them unasked.
- Plan first for anything non-trivial; get my OK before implementing.
- When I correct you, propose a CLAUDE.md update so the correction sticks.

## Builds

Cloud sessions only run tests (`flutter pub get`, `flutter test`). No APK/AAB/iOS builds here.
