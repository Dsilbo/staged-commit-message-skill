---
name: staged-commit-message
description: Strict generation of one final Chinese Git commit message from `git diff --staged` only. Use when Codex needs to write a commit message from staged changes, especially for requests such as "生成 commit message" or "根据 staged diff 写提交信息", and the output must ignore unstaged files, commit history, branch names, filename guesses, project background, and operator explanations.
---

# Staged Commit Message

## Overview

Generate exactly one final Chinese commit message from the raw staged diff. Treat `git diff --staged` as the only valid evidence source and apply conservative rules whenever the diff is ambiguous.

## Applicability

Use this skill when:

- generating a commit message from staged changes
- enforcing a unified Chinese commit message style
- requiring AI to follow the staged diff strictly instead of freeform summarization

Do not use this skill when:

- the staged area is empty
- the task is to summarize the whole working tree instead of staged diff
- the task is to generate a PR description, change summary, or release note
- the current task is not commit-message generation

## Workflow

1. If the user did not paste staged diff, run `git diff --staged` in the relevant repository.
2. If the command cannot be read, fails, or returns no usable staged diff, reply in the conversation with exactly:

```text
无法读取 git 暂存区（git diff --staged）
```

This reply is the final output for the turn. Do not fabricate a commit message in this case.

3. Read only the staged diff. Ignore unstaged files, commit history, branch names, filename guesses, project background, and operator explanations.
4. Determine the highest-priority applicable type.
5. Add a scope only when it is directly identifiable from the diff and valid under the scope rules below.
6. Write the subject in Chinese with a concrete object and a concrete action.
7. Choose concise or detailed format based on the format rules below.
8. Output only the final commit message with no explanation, analysis, or Markdown wrapping.

## Hard Rules

- Only output the final commit message.
- Do not output explanation, analysis, notes, Markdown, or any prefix/suffix.
- Generate exactly one commit message for one staged diff.
- All judgments must be based on the diff as evidence.
- Never supplement information not explicit in the diff.
- If evidence is insufficient, tighten the wording instead of inventing detail.
- When it is unclear whether the change is a new feature or a fix, use `🐛 fix`.
- Use `♻️ refactor` only when the diff clearly proves there is no behavior change.
- If any behavior, state, data, API, or parameter-semantics change appears, do not use `♻️ refactor` or `💄 style`.

## Type Priority

Apply the first matching type and stop:

1. `⏪ revert`
2. `✅ test`
3. `📝 docs`
4. `🎨 build` / `👷 ci` / `🔧 chore`
5. `⚡️ perf`
6. `🐛 fix`
7. `✨ feat`
8. `♻️ refactor`
9. `💄 style`

## Classification Rules

- Treat changes to user flow, state flow, business rules, API behavior, user data, form submission/validation, list display conditions, permission logic, enum/status semantics, or user-visible enable/disable/operability as core behavior changes.
- In gray areas, prefer the stricter interpretation: if behavior change is not clearly disproven, classify as behavior-changing.
- If it is unclear whether the change belongs to core business logic, treat it as core business logic.
- If it is unclear whether a parameter name, field name, or state value change affects callers, treat it as caller-impacting.
- If it is unclear whether a display-condition change affects user understanding, treat it as user-impacting.
- Use `⏪ revert` only when the diff clearly shows a revert.
- Use `✅ test` only for clearly test-only diffs.
- Use `📝 docs` only for clearly documentation-only diffs.
- Use `🎨 build` / `👷 ci` / `🔧 chore` only for infra/config/maintenance changes that do not match a higher-priority type.

## Mixed Changes

- Choose the subject from the single most user-visible impact.
- If impacts are comparable, rank them as: interface/data change > UI/interaction change > internal structure change.
- Put other important changes into detailed `- 改动点` bullets.
- Do not drop key behavior changes just to keep the message concise.

## Scope Rules

- Scope is optional.
- Use at most one scope.
- Scope must be lowercase kebab-case and directly identifiable from the diff.
- Prefer only a component name or one of: `composables`, `store`, `router`, `types`, `build`.
- Omit scope when certainty is insufficient.

## Subject Rules

- Write the subject in Chinese.
- Keep it within 50 Chinese characters.
- Do not end with `。`.
- Use imperative or result-oriented phrasing.
- Include a concrete object and a concrete action.
- Prefer the highest user-visible impact.
- Do not center the subject on vague verbs such as `优化`、`调整`、`重构`、`处理`、`修改`.

## Format Rules

Use the concise format only when all conditions are true:

- changed files <= 2
- clearly new or behavior-changing lines <= 20
- not core business logic
- one focused change only
- the intent remains complete without a second bullet

Concise format:

```text
<type>(<scope>): <subject>
```

or

```text
<type>: <subject>
```

Otherwise use the detailed format:

```text
<type>(<scope>): <subject>

- 改动点 1
- 改动点 2
- 改动点 3
```

Detailed-format rules:

- Use at most 3 bullets.
- Order bullets by user-visible impact.
- Make each bullet concrete, standalone, and verifiable from the diff.
- Include only facts proven by the diff.

## Revert Format

Use this format only when the diff clearly reverts a previous commit:

```text
revert: ⏪ revert "<commit header>"

This reverts commit <hash>.
```

This is the only case where English output is allowed.

## Output Checklist

Before responding, verify all of the following:

- The result is based only on `git diff --staged`.
- Type priority was applied.
- Scope is omitted unless it is certain.
- The subject is concrete and concise.
- Detailed bullets, if present, are all provable from the diff.
- No extra explanation is included.
