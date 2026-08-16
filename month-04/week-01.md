## pytest contributions

In the first week, I started working through the issues selected in the contribution proposal. The work did not go exactly as planned: one issue turned out to have already been fixed, while the other two led to fixes and pull requests.

### [Issue #13754](https://github.com/pytest-dev/pytest/issues/13754)

I started by reproducing this issue, but while preparing the contribution I found that the behavior had already been fixed by other changes in pytest, even though the issue was still open. After verifying the current behavior, I [documented](https://github.com/pytest-dev/pytest/issues/13754#issuecomment-5272208113) the finding in the issue.

### [Issue #6997](https://github.com/pytest-dev/pytest/issues/6997)

This issue was related to xfail reporting during fixture setup and teardown. The terminal summary reported the test as **XFAIL**, but did not make the phase that caused the failure clear.

I traced the reporting path and updated the terminal output so failures outside the normal test call are identified by their phase. During review, the solution was adjusted to cover both setup and teardown consistently, with regression tests for each case. The changes are in this [PR](https://github.com/pytest-dev/pytest/pull/14875).

### [Issue #11071](https://github.com/pytest-dev/pytest/issues/11071)

This issue involved `conftest.py` discovery when using an explicit `--rootdir`. A `conftest.py` located directly inside the configured root directory could still be treated as `non-top-level` when pytest was invoked from a directory above it.

I traced the initial conftest discovery through `_decide_args()` and `_set_initial_conftests()` and found that discovery could start from the invocation directory without using the configured `rootpath` as an initial anchor. The fix adds the normalized `rootpath` to the initial discovery anchors, with a regression test covering the original scenario. The changes are in this [PR](https://github.com/pytest-dev/pytest/pull/14889).
