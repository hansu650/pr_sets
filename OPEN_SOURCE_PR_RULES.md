# Open Source PR Execution Rules

This workspace is for small, real open-source contributions. The goal is to
complete safe PRs that build contribution history without taking on heavy
features, hardware work, or fragile local setup.

## Goal-Driven Loop

- Start each contribution with an explicit goal and concrete success criteria.
- The main Codex agent is the master agent: it owns final decisions, edits, and
  verification.
- Use subagents only for bounded parallel checks, such as issue suitability,
  duplicate PR search, repository context, or validation suggestions.
- Treat subagent output as advice, not final truth. The master agent must verify
  before editing or declaring completion.
- Close subagents when their task is done or no longer relevant.

## Preferred PR Shape

- Prefer documentation, docstring examples, small tests, or very small bug fixes.
- Keep changes to 1-3 files unless the user explicitly approves more.
- Avoid big features, architecture discussion, full examples, heavy builds,
  training jobs, GPU requirements, large datasets, or dependency churn.
- Avoid pure typo-only PRs unless they fix a real user-facing issue.
- Every change must have a clear reason tied to an issue, docs gap, or verified
  behavior.

## Issue Selection

Before editing:

- Confirm the issue is open.
- Check labels and task scope.
- Check that no one is clearly working on it.
- Check that no open PR already solves it.
- Check maintainer guidance and repository contribution rules.
- Stop if the issue is ambiguous, already claimed, already solved, too broad, or
  requires hardware/heavy local setup.

## Environment Rules

- Do not pollute global Python or shared environments.
- Do not install packages into `base`.
- `base` may be used for lightweight commands only, with no package installs.
- `DL` may be used only when a task genuinely needs existing PyTorch-related
  packages.
- Never change the PyTorch version in `DL`.
- Do not use or modify the user's other conda environments unless the user
  explicitly permits it.
- For non-Torch projects, prefer repository-local commands and existing Python
  if they work without installing dependencies.
- If dependencies are needed, create a project-specific environment with a clear
  name. Do not install into shared environments.
- Do not run GPU, training, full test suites, large docs builds, or downloads
  unless they are necessary and explicitly approved.

## Local Workflow

- Clone upstream or the user's fork as appropriate.
- Use a short, descriptive branch name, such as `doc/sos2zpk-example`.
- Read the relevant README, contribution guide, issue thread, and target file
  before editing.
- Use minimal diffs. Do not format unrelated code, reorder imports, or touch
  neighboring files casually.
- Use `apply_patch` for manual file edits.
- Run focused validation matched to the change.
- Always inspect `git status`, `git diff`, and `git diff --check` before commit.
- Commit with the repository's expected style. Include special tags such as
  `[docs only]` when the project asks for them.

## GitHub Boundaries

- If `gh` is unavailable or not logged in, do not block local work.
- The user must perform browser login, fork creation, and issue comments when
  authentication is required.
- Codex can set remotes, push branches, and prepare PR text after the fork exists
  and credentials are available.
- Do not open a second PR while the first PR is waiting for review unless the
  user explicitly decides to proceed.

## PR Description Requirements

- Keep the description factual and modest.
- Link the related issue in the format expected by the project.
- List actual validation commands that were run.
- Explain skipped heavy validation when relevant.
- Include AI assistance disclosure when the project policy asks for it, and make
  clear that the final change was reviewed and understood by the user.

## Stop Conditions

Stop and report instead of forcing a PR if:

- Another contributor is already working on it.
- An open PR already solves it.
- The change expands beyond 3 files.
- The task needs GPU, training, large datasets, or heavy local setup.
- Validation cannot be done at all.
- The code behavior is uncertain.
- Maintainers require discussion before changes.
- The proposed change does not have real value.
