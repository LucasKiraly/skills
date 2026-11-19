---
name: conventional-commits
description: Write clear, consistent Git commit messages following Conventional Commits.
metadata:
  author: Lucas Kiraly
  updated: "2026-11-19"
---

# Conventional Commits

Write clear, consistent Git commit messages following Conventional Commits.

## Instructions

1. Inspect the staged changes or diff before writing the commit message.

   - Identify all affected files, modules, and behaviors.
   - Do not guess the intent from file names alone when the diff shows a different change.

2. Group unrelated changes when possible or focus the message on the primary change.

   - Keep each commit focused on a single logical change.
   - When multiple files belong to the same feature, fix, or refactor, summarize the cohesive change rather than listing every file.

3. Preserve existing repository conventions if the project already uses specific scopes or commit patterns.

   - Match established scope names (such as `api`, `cli`, `auth`, `ui`, `deps`) when clear from context.
   - Keep the message concise and consistent across the history.

## Commit format

Use this structure for the commit subject line:

```text
<emoji> <type>(<scope>): <description>
```

Keep `<scope>` optional only when the change is truly global or spans unrelated areas without a single clear scope.

```text
✨ feat(parser): support nested configuration blocks
```

```text
🐛 fix(api): handle missing authorization header
```

```text
🔧 chore: update pre-commit hooks
```

### Body and footer

Add a blank line after the subject line only when extra context is genuinely needed (such as motivation, edge cases, or breaking changes).

```text
♻️ refactor(queue): decouple worker dispatch from storage

Extract queue polling into a dedicated runner so tests can inject a
fake backend without patching internal state.

BREAKING CHANGE: `WorkerPool` now requires a `runner` argument.
```

## Commit types

Use a concise set of standard types mapped to a clear emoji:

- `✨ feat`: A new feature or user-facing capability.
- `🐛 fix`: A bug fix.
- `♻️ refactor`: A code change that neither fixes a bug nor adds a feature.
- `⚡ perf`: A performance improvement.
- `📝 docs`: Documentation-only changes.
- `✅ test`: Adding or updating tests.
- `🔧 chore`: Build configuration, tooling, scripts, or general maintenance.
- `📦 build`: Changes affecting the build system or external dependencies.
- `👷 ci`: CI/CD pipeline or workflow changes.
- `🎨 style`: Formatting, whitespace, or non-behavioral code style changes.

## Commit style rules

- Use the exact format `<emoji> <type>(<scope>): <description>` (or `<emoji> <type>: <description>` when scope is omitted).
- Keep the subject line concise, ideally 72 characters or fewer.
- Write `<description>` in lowercase and in the imperative mood (e.g., `add`, `fix`, `remove`, `extract`).
- Do not end the subject line with a period.
- Use lowercase kebab-case for `<scope>` when present (e.g., `auth`, `data-loader`, `ci`).
- Mark breaking changes with `!` after `<type>` or `<scope>` (e.g., `✨ feat(api)!: change payload schema`) and explain them in a `BREAKING CHANGE:` footer when needed.
- Explain what changed and why; avoid vague messages like `fix bug`, `update stuff`, or `wip`.

## Checklist

Before finishing, verify that:

- Inspect the diff or requested changes before drafting the message.
- Match the `<emoji> <type>(<scope>): <description>` structure.
- Use an appropriate `<type>` and matching emoji for the change.
- Keep `<description>` in imperative mood, lowercase, and without a trailing period.
- Keep the subject line concise and at 72 characters or fewer.
- Document breaking changes clearly when applicable.
