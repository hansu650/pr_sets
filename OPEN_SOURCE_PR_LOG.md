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
