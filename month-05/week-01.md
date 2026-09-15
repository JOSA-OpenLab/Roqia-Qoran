## OpenSSF Scorecard contributions

This week, I worked on three issues from my Scorecard contribution proposal. For each issue, I reproduced the reported behavior and traced the relevant code path to determine where the problem originated.

### [Issue #5090](https://github.com/ossf/scorecard/issues/5090)

This issue reported GitHub Actions dependencies with an unknown license even though their repositories have valid licenses.

I reproduced the behavior and found that Scorecard detects the repository license correctly. I traced the "Unknown License" result to `actions/dependency-review-action` and found that GitHub's Dependency Graph API can return GitHub Actions dependencies with both `license` and `source_repository_url` set to `null`.

The action already had a fallback for retrieving the license from GitHub when the source repository was available, but it could not use it when the source URL was missing. Since the package URL still identifies the GitHub Actions repository, I added a fallback that uses it to determine the repository and retrieve its license.

I added a regression test for this case and opened a [PR](https://github.com/actions/dependency-review-action/pull/1156) with the fix. I also documented my [findings](https://github.com/ossf/scorecard/issues/5090#issuecomment-5641709706) in the original Scorecard issue.

### [Issue #4273](https://github.com/ossf/scorecard/issues/4273)

This issue tracks an E2E test for GitHub commit statuses that had been disabled after it started failing.

I re-enabled the test locally and confirmed that it still fails. I then queried GitHub directly and found that the pinned commit used by the test currently has no Commit Status records. I traced `statusesHandler.listStatuses()` and confirmed that it returns the statuses received from GitHub without filtering them.

I tested the same E2E test with a pinned commit from `pytest-dev/pytest` that currently has Commit Status records, and the test passed. The full `clients/githubrepo` test package also passed.

I posted my [findings](https://github.com/ossf/scorecard/issues/4273#issuecomment-5649842235) and asked the maintainers whether they would prefer updating the existing fixture or using a dedicated fixture under `ossf-tests`.

### [Issue #3946](https://github.com/ossf/scorecard/issues/3946)

This issue reported that `github.com/elijaa/phpmemcachedadmin` receives a `10/10` Vulnerabilities score even though `CVE-2023-6026` is associated with the project.

I reproduced the `10/10` score and traced the Vulnerabilities check to determine whether Scorecard was only checking dependencies and missing the project itself. I found that Scorecard passes the repository commit to OSV Scanner, which ultimately queries OSV using that commit.

I tested the OSV API directly and found that it reports `CVE-2023-6026` and `CVE-2023-6027` for the commit tagged as `1.3.0`, but reports no vulnerabilities for the repository HEAD I tested. I traced the difference to the affected range in the OSV data, where both Git range boundaries resolve to the commit tagged as `1.3.0`.

I also compared the relevant source between `1.3.0` and the tested HEAD and did not find a relevant fix, but I did not independently verify that the later commit is exploitable. Based on the investigation, I found no Scorecard-side bug: Scorecard checks the project commit, but OSV does not report the vulnerability for the tested HEAD.

I documented my [findings](https://github.com/ossf/scorecard/issues/3946#issuecomment-5672895088) and asked whether the affected range should be clarified upstream.

### [PR #5163](https://github.com/ossf/scorecard/pull/5163) — Pending
