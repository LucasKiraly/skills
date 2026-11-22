---
name: conventional-commits
description: Writes clear, consistent Git commit messages that follow the Conventional Commits specification. Use when writing or improving commit messages.
---

# Conventional Commits

Commit messages follow the Conventional Commits specification and are clear and consistent.

Each one describes a single logical change, based on the diff rather than on file names alone. When the staged changes mix unrelated changes, separate commits are suggested.

Scopes and wording match the conventions already used in the repository's history.

## Subject line

The subject line has a type, an optional scope, and a description. The scope is omitted when the change is global or no meaningful scope applies.

```text
<type>(<scope>): <description>
```

```text
feat(checkout): support coupon codes
```

```text
fix(payments): handle missing gateway token
```

```text
chore: update pre-commit hooks
```

- The subject is at most 72 characters and has no trailing period.
- The description is lowercase and in the imperative mood (`add retry`, not `adds retry`).
- The description is specific enough to tell what changed without opening the diff (`handle missing gateway token`, not `fix bug`).
- The scope is lowercase kebab-case (`data-loader`, `ci`).

## Types

The type comes from a fixed set:

- `feat`: A new feature or user-facing capability.
- `fix`: A bug fix.
- `refactor`: A code change that neither fixes a bug nor adds a feature.
- `perf`: A performance improvement.
- `docs`: A documentation-only change.
- `test`: A change that adds or updates tests.
- `chore`: A tooling, script, or maintenance change that fits no other type.
- `build`: A change to the build system or external dependencies.
- `ci`: A change to CI/CD pipelines or workflows.
- `style`: A formatting or whitespace change that does not alter behavior.

## Body and footer

When the subject alone is not enough, a body after a blank line adds the missing context, such as the motivation for the change or edge cases.

A breaking change is marked with `!` after the type or scope. A `BREAKING CHANGE:` footer describes what breaks when the subject alone does not.

```text
refactor(payments)!: require a gateway when creating a checkout

Checkout no longer creates a default gateway.
Tests can now inject a fake gateway without patching internal state.

BREAKING CHANGE: `Checkout` now requires a `gateway` argument.
```
