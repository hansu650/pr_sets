# Open Source PR Log

This file records every open-source contribution attempt in this workspace,
whether merged, open, closed, or abandoned. The purpose is to preserve the
process, decisions, validation, and resume-ready outcomes.

## PR 1: Matplotlib `Line2D.set_markevery` docstring clarification

- Repository: `matplotlib/matplotlib`
- PR: https://github.com/matplotlib/matplotlib/pull/31664
- Related issue: https://github.com/matplotlib/matplotlib/issues/27842
- Status: Merged
- Merged event: https://github.com/matplotlib/matplotlib/pull/31664#event-25478269262
- Merge commit on upstream `main`: `ca9f6bf`
- Contributor commit: `a61f866 docs: clarify markevery float spacing`
- Branch: `fix/markevery-path-docs`
- Local path: `D:\daima\cursor\pr_sets\matplotlib`
- Date opened: 2026-05-13
- Date merged: 2026-05-13

### Scope

This was a small documentation clarification in Matplotlib's `Line2D`
docstring. The change clarified how float values for `markevery` are interpreted:
the ideal spacing is measured along the drawn line and then coerced to the
nearest available data point. For jagged lines, markers may therefore not appear
evenly spaced along the x- or y-axis.

### Files Changed

- `lib/matplotlib/lines.py`

### Validation

Commands/checks run locally:

```powershell
python -m py_compile lib\matplotlib\lines.py
git diff --check
```

Additional validation:

- Parsed `lib/matplotlib/lines.py` with `ast`.
- Confirmed the updated `Line2D.set_markevery` docstring contained the new
  wording.
- Checked `git status`, `git diff`, and `git diff --stat`.

### Notes

- This was the first completed public open-source PR.
- The contribution was merged into upstream `main`.
- It is suitable for a resume/GitHub profile note.

Resume wording:

```text
Contributed to Matplotlib by clarifying the `Line2D.set_markevery` docstring for float marker spacing behavior.
```

## PR 2: SciPy `scipy.signal.sos2zpk` docstring example

- Repository: `scipy/scipy`
- PR: https://github.com/scipy/scipy/pull/25135
- Related issue: https://github.com/scipy/scipy/issues/7168
- Status: Closed by maintainer, not merged
- Contributor commit: `285b2e5 DOC: add example for signal.sos2zpk`
- Branch: `doc/sos2zpk-example`
- Local path: `D:\daima\cursor\pr_sets\scipy`
- Date opened: 2026-05-13

### Scope

This is a documentation-only PR adding a runnable `Examples` section to
`scipy.signal.sos2zpk`. The example designs a second-order Butterworth filter in
second-order sections form, converts it to zeros-poles-gain form, and verifies a
round-trip conversion with `zpk2sos`.

This is slightly more advanced than the Matplotlib PR because it adds a runnable
API example rather than only clarifying wording.

### Files Changed

- `scipy/signal/_filter_design.py`

### Validation

Commands/checks run locally:

```powershell
python -m py_compile scipy\signal\_filter_design.py
git diff --check
```

Example validation, run from outside the SciPy source tree to avoid importing
the unbuilt local package:

```python
import numpy as np
from scipy import signal

sos = signal.butter(2, 0.25, output='sos')
z, p, k = signal.sos2zpk(sos)

print(np.round(z.real, 4))
print(np.round(p, 4))
print(f"{k:.4f}")
print(np.allclose(signal.zpk2sos(z, p, k), sos))
```

Observed output:

```text
[-1. -1.]
[0.4714+0.3333j 0.4714-0.3333j]
0.0976
True
```

Additional validation:

- Parsed `scipy/signal/_filter_design.py` with `ast`.
- Confirmed the `sos2zpk` docstring contains `Examples`.
- Ran a doctest parser check for the new docstring from outside the source tree:
  `failures=0, tries=8`.
- Checked `git status`, `git diff`, and staged diff before commit.

### Notes

- Commit message includes `[docs only]`, as requested for SciPy documentation-only
  contributions.
- GitHub showed codeowner review requested and workflows awaiting maintainer
  approval, which is normal for an external contributor.
- This PR is still pending at the time of this log entry.

Resume wording after merge:

```text
Contributed to SciPy by adding a runnable API docstring example for `scipy.signal.sos2zpk`, improving documentation coverage for signal-processing conversion utilities.
```

### Status Update: Closed

- Date checked: 2026-05-13
- Status: Closed by maintainer
- Maintainer reason:
  The PR was missing the required AI declaration according to SciPy's AI policy.
  Maintainer invited reopening with the PR template filled in.
- Relevant policy:
  https://scipy.github.io/devdocs/dev/conduct/ai_policy.html

What happened:

- The PR body did include an AI assistance note, but SciPy expected the formal
  AI declaration required by its PR template/policy.
- The closure should be treated as a process/template failure, not a technical
  failure of the docstring example itself.

Lesson:

- For SciPy, always read and fill the current PR template exactly.
- The AI declaration must state whether AI was used, which tools were used, how
  they were used, and which submitted code/text was AI-generated or AI-assisted.
- Communication with maintainers should be written by the contributor, not
  auto-generated.

Resume status:

- Do not count this as a merged contribution.
- It can be mentioned internally as an attempted SciPy documentation PR, but not
  as accepted open-source contribution material.

### Status Update: Reopened

- Date checked: 2026-05-14
- Status: Closed; abandoned after CI Mypy failure
- Follow-up action taken:
  The PR description was updated to use the current SciPy PR template sections,
  including `#### AI Generation Disclosure`.
- Branch maintenance:
  The branch was rebased on the latest `scipy:main` and force-pushed with
  `--force-with-lease`.
- Current note:
  Awaiting maintainer review. No technical review feedback has been received yet.

## PR 3: Seaborn `defaultdict` palette bug fix

- Repository: `mwaskom/seaborn`
- PR: https://github.com/mwaskom/seaborn/pull/3940
- Related issue: https://github.com/mwaskom/seaborn/issues/3632
- Status: Open, awaiting review
- Contributor commit: `3b830a2 BUG: allow defaultdict palettes for missing hue levels`
- Branch: `bug/defaultdict-palette`
- Local path: `D:\daima\cursor\opensource\seaborn-3632`
- Date opened: 2026-05-13

### Scope

This is a small bug fix with regression coverage. It fixes the behavior where a
`collections.defaultdict` used as a `palette` would still raise
`ValueError: The palette dictionary is missing keys` when a hue level was not
explicitly present.

The intended behavior is:

- regular `dict` palettes with missing hue levels still raise `ValueError`;
- `defaultdict` palettes with a usable `default_factory` can provide a default
  color for missing hue levels;
- `defaultdict(None, ...)` behaves like a regular dictionary and still raises.

### Files Changed

- `seaborn/_base.py`
- `tests/test_base.py`

### Validation

Commands/checks run locally:

```powershell
$env:PYTHONPATH="."
python -m pytest tests/test_base.py -k "palette or hue or mapping"
python -m pytest tests/test_base.py
python -m pytest tests/test_distributions.py -k "histplot or palette or hue"
git diff --check
```

Observed results:

```text
18 passed
165 passed
126 passed, 133 deselected
git diff --check passed, with only Windows LF/CRLF warnings
```

Additional validation:

- Ran the issue-style `sns.histplot` reproduction with a `defaultdict` palette.
- Confirmed missing hue level `"baz"` used `#000000`.
- Confirmed regular dict and `defaultdict(None, ...)` still raise `ValueError`.

### Notes

- This is stronger resume material than the first documentation PR because it is
  a real behavior fix with a regression test.
- The PR includes an AI assistance disclosure in the body.
- Watch the PR conversation/checks for maintainer feedback before doing more
  work.
- Status checked on 2026-05-14: still open, with no maintainer feedback visible
  yet.

Resume wording after merge:

```text
Contributed to Seaborn by fixing `defaultdict` palette handling for missing hue levels and adding regression coverage that preserves regular dictionary error behavior.
```

## Operating Notes

- Rules for future contributions are recorded in:
  `D:\daima\cursor\pr_sets\OPEN_SOURCE_PR_RULES.md`
- Do not install packages into shared environments.
- Use subagents for bounded research and close them when done.
- Prefer one open-source contribution at a time unless intentionally building a
  queue of low-risk documentation PRs.

## PR 4: Xarray scipy backend pickling regression

- Repository: `pydata/xarray`
- PR: https://github.com/pydata/xarray/pull/11339
- Related issue: https://github.com/pydata/xarray/issues/11323
- Status: Open, abandoned locally after CI Mypy failure
- Contributor commit: `35d2c6d BUG: fix scipy backend pickling after multiple opens`
- Branch: `bug/scipy-backend-pickle`
- Fork branch: `hansu650:xarray/bug/scipy-backend-pickle`
- Local path: removed after abandoning local follow-up
- Date prepared: 2026-05-14
- Date opened: 2026-05-14

### Scope

This is a small scipy-backend-only regression fix with focused test coverage.
The issue was that datasets opened from scipy-backed file-like objects could
fail pickling after another scipy file-like dataset was opened in the same
process.

Root cause:

- `_open_scipy_netcdf` dynamically created and re-registered
  `flush_only_netcdf_file` on every file-like open.
- A second open replaced the class object exposed through the pickle workaround.
- The first dataset still referenced the older class object, so pickle rejected
  it because the module/qualname lookup resolved to a different object.

Fix:

- Register `flush_only_netcdf_file` only once for `flush_only=True`.
- Reuse the stable registered class on later scipy file-like opens.
- Preserve the existing `close()` and `__del__()` behavior.
- Keep the scipy import lazy inside `_open_scipy_netcdf`.

### Files Changed

- `xarray/backends/scipy_.py`
- `xarray/tests/test_backends.py`
- `doc/whats-new.rst`

### Validation

Pre-patch reproduction failed with:

```text
_pickle.PicklingError: Can't pickle <class 'xarray.backends.scipy_._PickleWorkaround.flush_only_netcdf_file'>: it's not the same object as xarray.backends.scipy_._PickleWorkaround.flush_only_netcdf_file
```

Post-patch checks run locally in isolated conda env `xarray_11323`:

```powershell
$env:PYTHONPATH="."
python -m pytest xarray/tests/test_backends.py -k "scipy and pickle"
python -m pytest xarray/tests/test_backends.py -k "scipy"
python -m ruff check xarray/backends/scipy_.py xarray/tests/test_backends.py
git diff --check
```

Observed results:

```text
minimal repro: OK
6 passed, 2 skipped
233 passed, 40 skipped, 6 xfailed
ruff: All checks passed
git diff --check passed, with only Windows LF/CRLF warnings
```

### Notes

- PR #11339 opened against `pydata/xarray:main` from
  `hansu650:bug/scipy-backend-pickle`.
- Initial labels added by automation: `topic-backends`, `io`.
- Initial status check:
  `pre-commit.ci - pr` passed, Read the Docs was pending, and GitHub Actions
  checks were still in progress.
- The PR body should include a concise AI assistance disclosure following
  xarray's AI policy.
- Follow-up status:
  The GitHub Actions run later failed in the `Mypy` job. Full logs were not
  available locally because `gh` is not installed/authenticated and the REST log
  endpoint requires repository admin access.
- Local cleanup:
  The isolated conda environment `xarray_11323` and local repository directory
  `D:\daima\cursor\opensource\xarray-11323` were removed on 2026-05-14.
- Recommended next action:
  Close the PR manually with a short note if choosing not to continue, so
  maintainers do not spend review time on an abandoned branch.

## PR 5 Candidate: IPython `term_title_format` config docs rendering

- Repository: `ipython/ipython`
- PR: https://github.com/ipython/ipython/pull/15212
- Issue: https://github.com/ipython/ipython/issues/15177
- Status: Open, CI failing in downstream macOS pyflyby integration test
- Contributor commit: `bd9be24 DOC: clarify term_title_format help text`
- Branch: `docs/term-title-format-help`
- Fork branch: `hansu650:docs/term-title-format-help`
- Local path: `D:\daima\cursor\opensource\ipython-15177`
- Isolated environment: `ipython_15177` (removed after validation)
- Date prepared: 2026-05-14

### Scope

This is a small documentation-source fix for IPython's generated configuration
docs. The `TerminalInteractiveShell.term_title_format` help text used the phrase
`Available substitutions are: {cwd}.`, which appears to be rendered incorrectly
in the generated docs because of the colon before `{cwd}`.

The local patch keeps the same meaning while avoiding the problematic wording:

```text
Customize the terminal title format. This is a Python format string. The
available substitution is {cwd}.
```

### Files Changed

- `IPython/terminal/interactiveshell.py`

### Validation

Commands/checks run locally in isolated conda env `ipython_15177`:

```powershell
conda run -n ipython_15177 python docs/autogen_config.py
conda run -n ipython_15177 python -m py_compile IPython\terminal\interactiveshell.py
conda run -n ipython_15177 python -c "...assert updated term_title_format help..."
git diff --check
```

Observed results:

```text
docs/autogen_config.py completed
py_compile passed
term_title_format help assertion passed
git diff --check passed, with only Windows LF/CRLF warnings
```

### Notes

- The issue page showed the issue open, with no assignee and no linked
  development branch or PR.
- Generated config docs under `docs/source/config/options` are ignored by git
  and were removed after validation, along with generated `__pycache__`
  directories.
- PR result:
  PR #15212 was opened from `hansu650:docs/term-title-format-help` into
  `ipython:main`.
- CI status checked from the PR page:
  21 checks passed, including docs build, Codecov, formatting, and Linux tests.
  The remaining failure was `Run Downstream tests / test (macos-14, 3.13)`,
  specifically the downstream `pyflyby` IPython integration test.
- Current interpretation:
  The failure is unlikely to be caused by this one-line config help text change,
  but the PR is still visibly blocked until the downstream failure is resolved,
  rerun, or judged unrelated by maintainers.

## PR 6 Candidate: cachetools `FIFOCache` update order bug fix

- Repository: `tkem/cachetools`
- Issue: https://github.com/tkem/cachetools/issues/395
- PR: https://github.com/tkem/cachetools/pull/397
- Status: Open, workflow awaiting maintainer approval
- Contributor commit: `f0dd36c Fix FIFOCache update order`
- Branch: `bug/fifo-update-order`
- Fork branch: `hansu650:bug/fifo-update-order`
- Local path: `D:\daima\cursor\opensource\cachetools-395`
- Isolated environment: `cachetools_395` (removed after validation)
- Date prepared: 2026-05-14

### Scope

This is a small behavior fix with regression tests. `FIFOCache` previously
moved an existing key to the end of the FIFO order when its value was updated.
That made an updated key behave like a newly inserted key and caused the wrong
entry to be evicted.

Fix:

- Keep the original insertion order when updating an existing key.
- Continue to add new keys to the end of the FIFO order.
- Preserve existing cache storage and sizing behavior.

### Files Changed

- `src/cachetools/__init__.py`
- `tests/test_fifo.py`

### Validation

Pre-patch reproduction showed the bug:

```text
['a', 'c']
a in cache: True
popitem: (2, 2)
```

Checks run locally in isolated conda env `cachetools_395`:

```powershell
conda run -n cachetools_395 python -m pytest tests/test_fifo.py -q
conda run -n cachetools_395 python -m pytest -q
D:\kaifagongju\Anaconda\envs\cachetools_395\python.exe -m tox -e py -- tests/test_fifo.py
D:\kaifagongju\Anaconda\envs\cachetools_395\python.exe -m tox -e py
D:\kaifagongju\Anaconda\envs\cachetools_395\python.exe -m tox -e flake8,pyright
git diff --check
```

Observed results:

```text
tests/test_fifo.py: 20 passed
full pytest: 280 passed, 2 skipped
tox -e py -- tests/test_fifo.py: 20 passed
tox -e py: 282 passed
tox -e flake8,pyright: passed
git diff --check passed, with only Windows LF/CRLF warnings
```

### Notes

- The issue was open with label `bug`.
- GitHub API showed zero open PRs for the repository when checked.
- The issue has an assignee, but there was no active linked PR and no explicit
  "I am working on this" comment visible.
- Push result:
  The branch was pushed to `https://github.com/hansu650/cachetools.git`.
- Next action:
  Open a focused bug-fix PR from `hansu650:bug/fifo-update-order` into
  `tkem:master`.

## PR 7 Candidate: marshmallow `utils.get_value` missing int key default

- Repository: `marshmallow-code/marshmallow`
- Issue: https://github.com/marshmallow-code/marshmallow/issues/2893
- PR: https://github.com/marshmallow-code/marshmallow/pull/2967
- Status: Closed by maintainer, not merged
- Contributor commit: `7ef3f4da Fix get_value default for missing int keys`
- Branch: `bug/get-value-int-index-default`
- Fork branch: `hansu650:bug/get-value-int-index-default`
- Local path: `D:\daima\cursor\opensource\marshmallow-2893`
- Isolated environment: `marshmallow_2893` (removed after validation)
- Date prepared: 2026-05-15

### Scope

This is a small behavior fix with regression tests. `utils.get_value` first
tries `obj[key]`, then falls back to `getattr(obj, key, default)`. For an
out-of-range integer list index, `obj[key]` raises `IndexError`, and the
fallback call raised `TypeError` because `getattr` requires a string attribute
name.

Fix:

- Return the supplied default when indexed lookup fails and the key is not a
  string.
- Preserve string-key fallback to object attributes.
- Add regression coverage for out-of-range integer list indexes, `int`
  subclasses, and missing integer dictionary keys.
- Add `@hansu650` to `AUTHORS.rst`, as requested by the project's contribution
  checklist for code PRs.

### Files Changed

- `AUTHORS.rst`
- `src/marshmallow/utils.py`
- `tests/test_utils.py`

### Validation

Pre-patch reproduction failed with:

```text
TypeError: attribute name must be string, not 'int'
```

Checks run locally in isolated conda env `marshmallow_2893`:

```powershell
D:\kaifagongju\Anaconda\envs\marshmallow_2893\python.exe -m pytest tests/test_utils.py -k get_value -q
D:\kaifagongju\Anaconda\envs\marshmallow_2893\python.exe -m pytest tests -q -k "not from_timestamp_with_overflow_value"
D:\kaifagongju\Anaconda\envs\marshmallow_2893\python.exe -m ruff check src\marshmallow\utils.py tests\test_utils.py
D:\kaifagongju\Anaconda\envs\marshmallow_2893\python.exe -m py_compile src\marshmallow\utils.py
git diff --check
```

Observed results:

```text
targeted get_value tests: 9 passed, 12 deselected
test suite excluding existing Windows timestamp overflow assertion: 1178 passed, 1 deselected
ruff: All checks passed
py_compile: passed
git diff --check passed, with only Windows LF/CRLF warnings
```

Additional note:

- Running all of `tests/test_utils.py` on Windows exposed an existing unrelated
  failure in `test_from_timestamp_with_overflow_value`, where Windows raises a
  different `ValueError` message for an overflowing timestamp. That failure is
  outside this PR's scope and was excluded from the broader local test run.

### Notes

- The issue page showed the issue open, with no assignee and no linked
  development branch or PR.
- GitHub PR search found no open PR for `#2893`, `get_value`, or the
  out-of-range integer index behavior.
- The project default branch is `dev`, and existing open PRs target `dev`.
- Next action:
  No further action planned. The maintainer closed the PR because the issue
  comments explicitly stated that the current behavior is intentional and that
  new PRs duplicating this AI-attractive fix would be closed.

### Status Update: Closed

- Date checked: 2026-05-16
- Maintainer comment:
  The PR disclosed ChatGPT/Codex assistance, and the maintainer asked why the
  issue comments had not been read.
- Root cause:
  The issue was open, but the discussion clarified that maintainers do not want
  a behavior change. The issue was effectively a documentation/clarification
  reminder, not an invitation to implement the obvious code fix.
- Lesson:
  For every candidate issue, read all issue comments before coding, especially
  when the issue looks like an easy one-line AI fix.

## PR 8 Candidate: pre-commit Windows config path across drives

- Repository: `pre-commit/pre-commit`
- Issue: https://github.com/pre-commit/pre-commit/issues/2530
- PR: https://github.com/pre-commit/pre-commit/pull/3690
- Compare URL: https://github.com/pre-commit/pre-commit/compare/main...hansu650:pre-commit:bug/config-path-different-drive
- Status: Closed by maintainer, not merged
- Contributor commit: `ce37410 Fix config path handling across Windows drives`
- Branch: `bug/config-path-different-drive`
- Fork branch: `hansu650:bug/config-path-different-drive`
- Local path: `D:\daima\cursor\opensource\pre-commit-2530`
- Isolated environment: `precommit_2530` (removed after validation)
- Date prepared: 2026-05-16

### Scope

This is a small Windows behavior fix with a regression test. When
`pre-commit` changes directory to the repository root, it normalizes several
paths with `os.path.relpath`. On Windows, `os.path.relpath` raises
`ValueError` when the target path is on a different drive from the repository
root. That can make commands fail when `--config` points to a file on another
drive.

Fix:

- Add a small `_relpath` helper that preserves the original absolute path when
  `os.path.relpath` cannot represent it.
- Reuse the helper for the existing path normalization points in
  `_adjust_args_and_chdir`.
- Add a Windows-only regression test for `--config` on another drive.

### Files Changed

- `pre_commit/main.py`
- `tests/main_test.py`

### Validation

Checks run locally in isolated conda env `precommit_2530`:

```powershell
conda run -n precommit_2530 python -m pytest tests/main_test.py -k different_drive
conda run -n precommit_2530 python -m pytest tests/main_test.py -k adjust_args_and_chdir
conda run -n precommit_2530 python -m pytest tests/main_test.py
conda run -n precommit_2530 python -m py_compile pre_commit/main.py tests/main_test.py
git diff --check
```

Observed results:

```text
new regression test: 1 passed
adjust_args_and_chdir tests: 6 passed
tests/main_test.py: 30 passed
py_compile: passed
git diff --check passed, with only Windows LF/CRLF warnings
```

Attempted but not completed:

```powershell
conda run -n precommit_2530 pre-commit run --files pre_commit/main.py tests/main_test.py
```

This failed twice while fetching `pre-commit/pre-commit-hooks` from GitHub due
network connection reset / timeout, before running any hook logic.

### Notes

- The issue is open with `bug` and `windows` labels.
- The issue page showed no assignee and no linked development branch or PR.
- No open PR for `#2530` was found during screening.
- HTTPS push failed due local network connectivity, but SSH auth worked and the
  branch was pushed successfully.
- Next action:
  No further action planned on this PR unless the maintainer provides a
  concrete, non-AI-assisted path forward.

### Status Update: Closed

- Date checked: 2026-05-16
- Maintainer comment:
  `ai slop is not acceptable`
- Root cause:
  The patch may have been technically plausible, but it looked too much like a
  generic AI-produced PR to this maintainer. The change generalized path
  normalization through a helper and added a narrow unit test, but it did not
  demonstrate enough project-specific judgment, such as reproducing the exact
  `install -c` workflow from the issue or proposing the minimal behavior change
  in a maintainer-preferred style before submitting.
- Lesson:
  Some projects, especially maintainer-driven tooling projects, have very low
  tolerance for PRs that look agentic even when the code is small and tests
  pass. For these projects, either avoid the repository or ask a short,
  human-written implementation question on the issue before coding.

## PR 9 Candidate: Sphinx root document exclude pattern error

- Repository: `sphinx-doc/sphinx`
- Issue: https://github.com/sphinx-doc/sphinx/issues/11873
- PR: Not opened yet
- Status: Open, failing CI; abandoned
- Compare URL: https://github.com/sphinx-doc/sphinx/compare/master...hansu650:sphinx:bug/root-doc-pattern-error
- Contributor commit: `df99978 Improve root document exclude pattern error`
- Branch: `bug/root-doc-pattern-error`
- Local path: `D:\daima\cursor\opensource\sphinx-11873`
- Isolated environment: `sphinx_11873` (removed after validation)
- Date prepared: 2026-05-16

### Scope

This is a small behavior fix with a regression test and changelog entry. Sphinx
already had logic intended to explain when the root document is excluded by
configured patterns, but the exclude-pattern check matched a source-directory
absolute path against relative `exclude_patterns` entries. As a result,
`exclude_patterns = ['index.rst']` fell through to a misleading generic message
claiming the root document was outside the source directory.

Fix:

- Keep the absolute root document path for user-facing error messages.
- Use the source-relative root document path when matching against
  `exclude_patterns` and `include_patterns`.
- Add a regression test for `exclude_patterns = ['index.rst']`.
- Add a `CHANGES.rst` bug-fix entry for `#11873`.

### Files Changed

- `CHANGES.rst`
- `sphinx/builders/__init__.py`
- `tests/test_builders/test_build.py`

### Validation

Pre-patch reproduction:

```text
exclude_patterns = ['index.rst'] produced the generic error:
"The master document must be within the source directory or a subdirectory of it."
```

Post-patch reproduction:

```text
exclude_patterns = ['index.rst'] now reports:
"because it matches an exclude pattern specified in conf.py, 'index.rst'"
```

Checks run locally in isolated conda env `sphinx_11873`:

```powershell
conda run -n sphinx_11873 python -m pytest tests/test_builders/test_build.py -k "root_doc" -q
conda run -n sphinx_11873 python -m pytest tests/test_builders/test_build.py -q
conda run -n sphinx_11873 python -m ruff check sphinx/builders/__init__.py tests/test_builders/test_build.py
conda run -n sphinx_11873 python -m py_compile sphinx/builders/__init__.py tests/test_builders/test_build.py
git diff --check
```

Observed results:

```text
root_doc tests: 2 passed
tests/test_builders/test_build.py: 6 passed
ruff: All checks passed
py_compile: passed
git diff --check: passed
```

### Notes

- The issue is open with `good first issue` and `help wanted` labels.
- The issue was opened by a Sphinx maintainer.
- GitHub showed no open PR for `#11873`.
- The change is limited to 3 files: fix, regression test, changelog.
- Next action:
  Open a focused PR from `hansu650:bug/root-doc-pattern-error` into
  `sphinx-doc:master`.

### Status Update: PR Open, One CI Checkout Failure

- PR opened: https://github.com/sphinx-doc/sphinx/pull/14433
- Date checked: 2026-05-16
- Observed failing check:
  `Lint source code / ty`
- Failure detail:
  The job failed during `actions/checkout`, before running the type checker:

```text
failed to encode 'tests/roots/test-warnings/wrongenc.inc' from UTF-8 to iso-8859
failed to encode 'tests/roots/test-root/wrongenc.inc' from UTF-8 to iso-8859
```

- Assessment:
  This appears unrelated to the submitted patch. The same Sphinx fixture
  encoding issue also appeared during the local Windows checkout. The PR should
  not be changed to work around this unless a maintainer requests it.
- Next action:
  Wait for the remaining CI checks and maintainer feedback. Do not close or
  delete the fork while the PR is still open.

### Status Update: Abandoned

- Date checked: 2026-05-16
- Final observed status:
  The PR had multiple failing checks, including Python 3.15 CI jobs and the
  `ty` lint job. The PR page also displayed GitHub's hidden/bidirectional
  Unicode warning on the diff.
- Decision:
  Abandon this PR and delete the fork if desired.
- Root cause:
  Sphinx was a poor Windows-local target for this workflow. The repository has
  encoding-sensitive test fixtures and CI paths that were not adequately
  screened before submission. The local checkout already showed encoding
  warnings, and that should have been treated as a stop condition instead of a
  manageable warning.
- Lesson:
  If a repository checkout itself produces encoding warnings, hidden Unicode
  warnings, or platform-specific fixture conversion errors, do not submit from
  that local checkout. Pick a repository whose local clone is clean before any
  code changes.

## PR 10 Candidate: pipx `inject` option order parsing

- Repository: `pypa/pipx`
- Issue: https://github.com/pypa/pipx/issues/1444
- PR: Not opened
- Status: Stopped before editing; current `main` no longer reproduces the issue
- Branch attempted locally: `fix/inject-option-order`
- Local path: `D:\daima\cursor\opensource\pipx-1444` (removed after stop)
- Isolated environment: `pipx_1444` (removed after stop)
- Date checked: 2026-05-18

### Screening Result

This looked like a good candidate on paper:

- issue open with `bug` and `help wanted` labels;
- no assignee;
- no linked development branch or PR visible;
- no obvious open PR found for `#1444`;
- contribution docs were standard and required a changelog fragment;
- clone was clean with no encoding or hidden Unicode warnings.

### Stop Condition

Current `main` already parses both forms successfully:

```text
pipx inject ansible-core ansible --force
pipx inject ansible-core --force ansible
```

Parser-level reproduction showed both commands produce:

```text
package: ansible-core
dependencies: ['ansible']
force: True
```

Running the real CLI for the issue command did not fail with
`unrecognized arguments`; it reached command execution and failed only because
the target venv was not installed:

```text
Can't inject 'ansible' into nonexistent Virtual Environment 'ansible-core'.
```

### Decision

No patch was made. Opening a PR would risk submitting a no-op fix for stale
issue behavior.

### Lesson

Always reproduce on current `main` before implementing, even when an issue is
open and labeled `help wanted`. A stale open issue is still a stop condition if
the reported failure no longer occurs.

## PR 11 Candidate: pip-audit `PIPAPI_PYTHON_LOCATION` during fixes

- Repository: `pypa/pip-audit`
- Issue: https://github.com/pypa/pip-audit/issues/1030
- PR: https://github.com/pypa/pip-audit/pull/1036
- Status: Open; approved by maintainer; blocked by unrelated upstream CI failure
- Branch: `fix/pip-source-python-location`
- Commit: `2a11d40 Use PIPAPI_PYTHON_LOCATION when fixing dependencies`
- Fork branch:
  https://github.com/hansu650/pip-audit/tree/fix/pip-source-python-location
- Local path: `D:\daima\cursor\opensource\pip-audit-1030`
- Isolated environment: `pip_audit_1030`
- Date checked: 2026-05-19

### Screening Result

- Issue `#1030` was open with `bug-candidate` label.
- No assignee was shown.
- The issue had no linked branches or pull requests.
- Searches for open PRs mentioning `#1030`, `PIPAPI_PYTHON_LOCATION`, or
  `PipSource.fix` found no active duplicate.
- `CONTRIBUTING.md` says public or CLI behavior changes should update
  `CHANGELOG.md` under `Unreleased`.
- Fresh clone was clean before editing.

### Reproduction

A regression test was added first and failed on current `main`.

The failing behavior was that `PipSource.fix()` still built its install command
with `sys.executable`, even when `PIPAPI_PYTHON_LOCATION` was set. This matched
the issue report.

### Local Patch

Modified files:

- `pip_audit/_dependency_source/pip.py`
- `test/dependency_source/test_pip.py`
- `CHANGELOG.md`

Change summary:

- `PipSource.fix()` now uses `PIPAPI_PYTHON_LOCATION` when it is set, falling
  back to `sys.executable`.
- Added a regression test that patches `subprocess.run` and checks the generated
  `pip install` command uses the configured Python location.
- Added a short `Unreleased / Fixed` changelog entry.

### Validation

Observed results:

```text
python -m pytest test/dependency_source/test_pip.py -k "fix"
3 passed, 6 deselected

python -m pytest test/dependency_source/test_pip.py
9 passed

python -m ruff check pip_audit/_dependency_source/pip.py test/dependency_source/test_pip.py
All checks passed!

git diff --check
passed
```

Note:

- The test run emitted a dependency warning from `pip_api` importing Python's
  deprecated `sre_constants` module under Python 3.12. This was not introduced
  by the patch.
- The initial Windows line-ending state was normalized to LF before final
  validation.

### PR Status Update

- PR opened: https://github.com/pypa/pip-audit/pull/1036
- Maintainer `woodruffw` approved the PR with: "Thank you @hansu650, this
  seems right to me."
- CI failed in `test_requirement_source_require_hashes_unpinned`, which is
  outside the PR's changed files.
- The same test was also failing on current upstream `main`, so this was
  treated as an unrelated upstream failure rather than something to fix inside
  this PR.
- A short PR comment was posted explaining that the PR would be left unchanged
  to avoid mixing in an unrelated requirement-source fix:
  https://github.com/pypa/pip-audit/pull/1036#issuecomment-4486142454

Relevant CI evidence:

```text
PR CI run:
https://github.com/pypa/pip-audit/actions/runs/26015682749

Failing test:
test/dependency_source/test_requirement.py::test_requirement_source_require_hashes_unpinned

Upstream main CI also failed on the same test:
https://github.com/pypa/pip-audit/actions/runs/26036592893
```

Next action: do not push or rebase unless maintainers ask for it or upstream
`main` becomes green and the PR needs a rerun/update.

## PR 12 Candidate: pyproject-hooks thread-local subprocess runner

- Repository: `pypa/pyproject-hooks`
- Issue: https://github.com/pypa/pyproject-hooks/issues/226
- PR: Not opened
- Status: Local proof prepared; do not open PR before maintainer preference is clear
- Branch: `fix/thread-local-subprocess-runner`
- Local path: `D:\daima\cursor\opensource\pyproject-hooks-226`
- Isolated environment: `pyproject_hooks_226`
- Date checked: 2026-05-18

### Screening Result

- Issue `#226` was open.
- No assignee was shown.
- Searches for open PRs mentioning `#226`, `subprocess_runner`,
  `thread-safe`, or `ContextVar` found no active duplicate.
- Test setup is light: `dev-requirements.txt` installs `pytest`, `flake8`,
  `testpath`, and `setuptools`; `noxfile.py` runs pytest for tests.
- No explicit AI policy was found.

### Maintainer Signal

Important caution:

- A maintainer comment suggested that `BuildBackendHookCaller` objects are cheap,
  and that documenting one instance per thread may be preferable to adding
  thread-local behavior.

Decision:

- Prepare and validate a local proof, but do not open a PR without first asking
  whether maintainers prefer a code change or documentation note.

Suggested issue comment:

```text
I checked this locally and can reproduce the cross-thread runner leak with a
small regression test. I also have a minimal ContextVar-based patch that keeps
the temporary runner scoped to the current context while preserving the default
runner on the shared caller.

Before opening a PR, would you prefer a code fix for this behavior, or would you
rather document that callers should use a separate BuildBackendHookCaller per
thread?
```

### Reproduction

A regression test was added first and failed on current `main`:

```text
assert calls == ["default", "scoped"]
E AssertionError: assert ['scoped', 'scoped'] == ['default', 'scoped']
```

This shows that a subprocess runner override active in one thread can affect a
concurrent call in another thread using the same `BuildBackendHookCaller`.

### Local Patch

Modified files:

- `src/pyproject_hooks/_impl.py`
- `tests/test_call_hooks.py`

Change summary:

- Added a `ContextVar` for the temporary subprocess runner override.
- Kept `self._subprocess_runner` as the default runner.
- `subprocess_runner()` now sets/resets the context-local override token.
- `_call_hook()` uses the context-local override when present, otherwise the
  default runner.
- Added a deterministic threading regression test using `threading.Event`, not
  sleeps.

### Validation

Observed results:

```text
python -m pytest tests/test_call_hooks.py -k "runner or subprocess_runner"
2 passed, 18 deselected

python -m pytest tests/test_call_hooks.py
20 passed

git diff --check
passed
```

Additional checks:

- Scanned modified files for hidden/bidi/control characters: none found.
- Normalized modified files to LF.
- `python -m flake8 src tests` was attempted but current `main` has existing
  flake8 failures across unchanged files, including E501 and F401. Do not treat
  that full-repo command as a clean validation baseline for this patch.

### Next Action

Ask on issue `#226` whether maintainers want a code fix or documentation-only
guidance before opening a PR.

### Status Update - 2026-05-19

- Maintainer reply:
  https://github.com/pypa/pyproject-hooks/issues/226#issuecomment-4486882059
- Outcome: abandoned as a code PR target.
- Reason: maintainer prefers documenting the limitation for now instead of
  adding a ContextVar/thread-local workaround around mutable runner state.
- If a code fix is ever needed later, maintainer suggested a more explicit API
  shape, such as returning a modified hook caller copy with a custom runner,
  rather than adding hidden context-local behavior.
- Existing local ContextVar patch remains local learning/proof material only.
  Do not push it or open a PR from `fix/thread-local-subprocess-runner`.
- Possible future path: a small documentation-only PR could be considered, but
  it should start from a fresh `upstream/main` branch and must not include the
  local code patch.

## PR 13 Candidate: pip-audit skip editable before hash validation

- Repository: `pypa/pip-audit`
- Issue: https://github.com/pypa/pip-audit/issues/1024
- PR: https://github.com/pypa/pip-audit/pull/1037
- Status: Open; waiting for maintainer review / workflow approval
- Branch: `fix/skip-editable-hashes`
- Commit: `9b12082 Skip editable requirements before hash validation`
- Fork branch:
  https://github.com/hansu650/pip-audit/tree/fix/skip-editable-hashes
- Local path: `D:\daima\cursor\opensource\pip-audit-1024`
- Isolated environment: `pip_audit_1024`
- Date checked: 2026-05-19

### Screening Result

- Issue `#1024` was open with `bug-candidate` label.
- No assignee was shown.
- The issue had no linked branches or pull requests.
- Searches for open PRs mentioning `#1024`, `skip-editable`, or hash
  validation found no active duplicate.
- The issue has a maintainer comment saying they would take a look and noting
  that `uv audit` may be relevant for uv-based workflows. This is not a
  rejection, but it means a PR should stay narrow.
- `pip-audit #1036` remained open and was later approved by maintainer
  `woodruffw`; its CI failure matched an unrelated upstream `main` failure, so
  this PR did not need to pause for #1036.

### Reproduction

A regression test was added first and failed on current `main`.

Observed failure:

```text
RequirementSourceError: requirement --editable file:flask.py#egg=flask==2.0.1 does not contain a hash
```

This matched the issue: when one requirement in a file has a hash, hash mode is
inferred before editable requirements are skipped.

### Local Patch

Modified files:

- `pip_audit/_dependency_source/requirement.py`
- `test/dependency_source/test_requirement.py`
- `CHANGELOG.md`

Change summary:

- In `_collect_preresolved_deps()`, `skip_editable` handling now runs before
  hash validation.
- Skipped editable requirements now `continue` immediately, so they do not
  proceed into URL, marker, duplicate, or pinning logic.
- Added a regression test with an editable requirement lacking a hash and a
  normal pinned requirement with a hash in the same file.

### Validation

Observed results:

```text
python -m pytest test/dependency_source/test_requirement.py -k "editable and hash"
1 passed, 45 deselected

python -m pytest test/dependency_source/test_requirement.py -k "skip_editable_before_hash_validation or disable_pip_editable_skip or disable_pip_incomplete_hashes or require_hashes_inferred" --skip-online
4 passed, 42 deselected

python -m ruff check pip_audit/_dependency_source/requirement.py test/dependency_source/test_requirement.py
All checks passed!

git diff --check
passed
```

Additional checks:

- Scanned modified files for hidden/bidi/control characters: none found.
- Normalized modified files to LF.
- Running the entire `test/dependency_source/test_requirement.py` file timed out
  on Windows, even with `--skip-online`, because the file contains heavier
  dependency-resolution paths outside this patch's scope. The focused
  hash/editable/disable-pip subset was used as the local proof boundary.

### Next Action

Open the PR using:

```text
https://github.com/pypa/pip-audit/compare/main...hansu650:pip-audit:fix/skip-editable-hashes
```

### PR Draft

Title:

```text
Skip editable requirements before hash validation
```

Body:

```markdown
Fixes #1024.

This skips editable requirements before enforcing hash validation when
`--skip-editable` is enabled.

Non-editable requirements still keep the existing hash validation behavior.

Tests:
- `python -m pytest test/dependency_source/test_requirement.py -k "editable and hash"`
- `python -m pytest test/dependency_source/test_requirement.py -k "skip_editable_before_hash_validation or disable_pip_editable_skip or disable_pip_incomplete_hashes or require_hashes_inferred" --skip-online`
- `python -m ruff check pip_audit/_dependency_source/requirement.py test/dependency_source/test_requirement.py`
- `git diff --check`
```

## PR 14: installer normalize RECORD paths when writing

- Repository: `pypa/installer`
- Issue: https://github.com/pypa/installer/issues/158
- PR: https://github.com/pypa/installer/pull/335
- Status: Open; all checks passed, waiting for maintainer review
- Branch: `fix/normalize-record-paths`
- Commit: `0464764 Normalize RECORD paths when writing`
- Fork branch:
  https://github.com/hansu650/installer/tree/fix/normalize-record-paths
- Local path: `D:\daima\cursor\opensource\installer-158`
- Isolated environment: `installer_158`
- Date checked: 2026-05-19

### Screening Result

- Issue `#158` was open.
- No assignee was shown.
- The issue had no linked branches or pull requests.
- Searches for open PRs mentioning `#158`, `RecordEntry.to_row`, forward-slash
  normalization, or RECORD path normalization found no active duplicate.
- Open PRs `#327` and `#328` mention RECORD-related install behavior, but they
  do not modify `src/installer/records.py` or address `RecordEntry.to_row()`.
- Maintainer comment suggested using `PureWindowsPath(...).as_posix()` for
  normalization, so the implementation followed that direction.

### Reproduction

Because the local machine is Windows, current `main` already normalizes
backslashes in normal execution through the old `os.sep == "\\"` branch. To
prove the cross-platform failure locally, the reproduction simulated a
non-Windows platform by setting `installer.records.os.sep = "/"` before calling
`RecordEntry.to_row()`.

Observed failing behavior on current `main`:

```text
row ('distribution-1.0.dist-info\\RECORD', '', '')
AssertionError
```

Expected row:

```text
('distribution-1.0.dist-info/RECORD', '', '')
```

### Local Patch

Modified files:

- `src/installer/records.py`
- `tests/test_records.py`

Change summary:

- `RecordEntry.to_row()` now normalizes the output path with
  `PureWindowsPath(path).as_posix()` instead of only replacing backslashes when
  the current platform separator is `\`.
- Added regression coverage for Windows-style RECORD paths without a prefix.
- Added regression coverage for Windows-style RECORD paths with a `path_prefix`.

### Validation

Observed results:

```text
python -m pytest tests/test_records.py -k "backslash_path or string_representation"
21 passed, 44 deselected

python -m pytest tests/test_records.py
65 passed

python -m pytest --cov=installer --cov-fail-under=100 --cov-report=term-missing --cov-context=test -n auto tests
152 passed
Required test coverage of 100% reached. Total coverage: 100.00%

python -m ruff check src/installer/records.py tests/test_records.py
All checks passed!

python -m ruff format --check src/installer/records.py tests/test_records.py
2 files already formatted

python -m pre_commit run --files src/installer/records.py tests/test_records.py
all hooks passed

git diff --check
passed
```

### PR Status

PR opened:

```text
https://github.com/pypa/installer/pull/335
```

Status after opening:

- GitHub Actions test matrix: success on Ubuntu, Windows, macOS, and PyPy
- `docs/readthedocs.org:installer`: success
- `pre-commit.ci - pr`: success

Next action: monitor CI and respond only if a failing check is related to the
two touched files.

## Notification Triage - 2026-05-19

GitHub notifications were reviewed through `gh api notifications`.

Processed notifications:

- `pypa/pip-audit #1036`
  https://github.com/pypa/pip-audit/pull/1036
  - Status: open, maintainer approved.
  - Current blocker: CI failure appears unrelated to the PR and also present on
    upstream `main`.
  - Action taken earlier: left a short comment explaining that the PR will stay
    unchanged to avoid mixing in an unrelated requirement-source fix.
  - Next action: wait for upstream CI/main to recover or maintainer guidance.
- `pypa/pip-audit #1037`
  https://github.com/pypa/pip-audit/pull/1037
  - Status: open, review/checks still pending.
  - Next action: wait; do not stack more changes onto this PR.
- `pypa/installer #335`
  https://github.com/pypa/installer/pull/335
  - Status: open, all visible checks passed.
  - Next action: wait for maintainer review.
- `pypa/pyproject-hooks #226`
  https://github.com/pypa/pyproject-hooks/issues/226#issuecomment-4486882059
  - Status: maintainer prefers documentation for now.
  - Next action: do not open the local ContextVar code PR.
- Old closed/abandoned PR notifications for Sphinx, pre-commit, marshmallow,
  IPython, xarray, SciPy, and Matplotlib were reviewed and require no action.

Notification cleanup:

- Current GitHub notification threads were marked done after triage.
- `gh api notifications?all=false` returned no unread notifications after
  cleanup.

## PR 15 Candidate Rejected: markdown-it-py table parsing

- Repository: `executablebooks/markdown-it-py`
- Issue: https://github.com/executablebooks/markdown-it-py/issues/376
- PR: Not opened
- Status: rejected before local proof
- Date checked: 2026-05-19

### Screening Result

- Issue `#376` is open and labeled `bug`.
- No assignee is shown.
- The issue includes a small reproduction for table parsing behavior.

### Maintainer Signal

Critical maintainer comment:

```text
This is consistent with the output of https://markdown-it.github.io,
so it is best if you raise this in https://github.com/markdown-it/markdown-it
first
```

Source:

```text
https://github.com/executablebooks/markdown-it-py/issues/376#issuecomment-3656289151
```

### Decision

Do not implement or open a PR for this target.

Reason:

- The maintainer already indicated that the behavior matches upstream
  `markdown-it`.
- A direct markdown-it-py patch would likely be rejected unless the upstream
  JavaScript project first changes or accepts the behavior.
- This matches a previous failure pattern: technically plausible patch, but
  maintainer direction is not aligned.

Next action:

- Ask Pro for a replacement target that explicitly includes a maintainer-comment
  review step before recommending implementation.
