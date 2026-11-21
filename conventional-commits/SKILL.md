---
name: conventional-commits
description: Generate clear, consistent Git commit messages that follow the Conventional Commits specification.
---

# Conventional Commits

Follow these instructions to create clear, consistent commit messages that adhere to the Conventional Commits specification.

## Instructions

1. Inspect the staged changes or diff before writing the commit message.

   - Identify all affected files, modules, and behaviors.
   - Do not infer the intent from file names alone when the diff reveals otherwise.

2. Keep commits focused on a single logical change.

   - When multiple files belong to the same feature, fix, or refactor, summarize the cohesive change rather than listing every file.
   - Avoid combining unrelated changes into a single commit.

3. Preserve existing repository conventions.

   - Match established scope names (such as `api`, `cli`, `auth`, `ui`, `deps`) when clear from context.
   - Keep the message concise and consistent with the commit history.

## Commit format

Use this structure for the commit subject line:

```text
<emoji> <type>(<scope>): <description>
```

The scope is optional when the change is global or no meaningful scope applies.

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

Add a blank line after the subject line only when extra context is genuinely needed, such as motivation, edge cases, or breaking changes.

Use the body to explain why the change was made or provide relevant context not captured by the subject.

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

- Follow the exact format `<emoji> <type>(<scope>): <description>` or `<emoji> <type>: <description>` when the scope is omitted.
- Keep the subject line concise, ideally 72 characters or fewer.
- Write the description in lowercase and imperative mood, starting with a verb (e.g., `add`, `fix`, `remove`, `extract`).
- Do not end the subject line with a period.
- Use lowercase kebab-case for scopes (e.g., `auth`, `data-loader`, `ci`).
- Mark breaking changes with `!` after the type or scope (e.g., `✨ feat(api)!: change payload schema`) and explain them in a `BREAKING CHANGE:` footer when needed.
- Explain what changed and why; avoid vague messages like `fix bug`, `update stuff`, or `wip`.

## Checklist

Before finishing, verify that:

- The staged changes or diff have been inspected.
- The commit follows the required format and uses the appropriate type and emoji.
- The description is lowercase, imperative, and has no trailing period.
- The subject line is concise and within the recommended length.
- Breaking changes are documented when applicable.
