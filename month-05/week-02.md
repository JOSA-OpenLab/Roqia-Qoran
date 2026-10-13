## OpenSSF Scorecard contributions

This week, I worked on four issues from my Scorecard contribution proposal. I investigated the problems, made changes, and added tests to check them. The backup triage opportunity developed into an implementation, followed by an investigation of the broader exclusion request.

### [Issue #1174](https://github.com/ossf/scorecard/issues/1174)

This issue requests lockfile analysis to verify that dependencies have integrity hashes. A lockfile can contain dependencies without hashes, so its presence alone does not establish that those dependencies are pinned.

I traced the Pinned-Dependencies check and added support for `package-lock.json` and `npm-shrinkwrap.json` versions 1, 2, and 3. The implementation checks dependency entries for supported integrity hashes and compares them with dependencies declared in `package.json`, reporting missing hashes and missing lockfile entries as unpinned.

I added regression tests for nested dependencies, shrinkwrap precedence, workspace dependencies, malformed input, and unsupported lockfile versions. I also tested Git dependencies to verify that an integrity hash alone does not make a mutable branch reference count as pinned.

The focused tests passed, including workspace tests with the race detector. I opened a [PR](https://github.com/ossf/scorecard/pull/5285) with the implementation and tests.

### [Issue #4431](https://github.com/ossf/scorecard/issues/4431)

This issue tracks deprecated OSV Scanner API usage. During my investigation, I found that the external grouping API had already been replaced by a local helper, so I focused on bugs in that replacement.

I reproduced two grouping problems. Entries with identical IDs but no aliases remained separate, producing duplicate findings. Merging connected groups could also leave existing members with stale group indexes, splitting vulnerabilities connected through aliases into multiple groups.

I changed the helper to merge group roots and resolve every member to its final root before extracting the results. I also added matching for identical non-empty IDs while preserving representative-ID selection and alias sorting and deduplication.

I added regression tests for both bugs and tested all 24 input permutations of a connected four-entry case. The tests check the complete grouped result and verify that the probe produces a single finding.

The package tests passed with the race detector, and package-specific lint reported 0 issues. I opened a [PR](https://github.com/ossf/scorecard/pull/5290) with the fix as a follow-up related to the original issue.

### [Issue #3480](https://github.com/ossf/scorecard/issues/3480)

This issue reports an over-approximation in repository ruleset admin-enforcement logic. A ruleset with bypass actors can cause `EnforceAdmins` to become false even when removing that ruleset would not change the effective protection of the branch.

I traced how repository rulesets are translated and merged with classic branch protection. I reproduced the problem with overlapping rulesets in both input orders, then changed the implementation to collect protection from rulesets without bypass actors before evaluating bypassable rulesets.

The implementation checks whether the bypassable protection is already covered. It compares approval counts, review settings, deletion and force-push restrictions, linear history, and required status checks. It also checks review-thread resolution and status-check integration IDs using the original ruleset parameters, and recognizes redundant `CREATION` and `REQUIRED_SIGNATURES` rules.

Classic branch protection contributes to coverage when it already enforces admins. When classic protection allows admin bypass, the implementation restores `EnforceAdmins=true` only if rulesets without bypass actors fully cover its protection and every bypassable ruleset is also covered. Partial coverage and unsupported rule types remain conservative.

I added regression tests for redundant and additional protection, ruleset ordering, combined coverage, matching and different status-check integrations, and full and partial coverage of classic protection.

The package tests passed with the race detector, and the final `make e2e-pat` run passed across 95 suites. `make all` stopped at repository-wide lint with 31 findings that matched the baseline findings exactly. Package-specific lint also reported the same 7 existing findings, with no new findings.

I opened a [draft PR](https://github.com/ossf/scorecard/pull/5295) with the implementation and tests.

### [Issue #4036](https://github.com/ossf/scorecard/issues/4036) — From triage to implementation

I initially selected this issue as a backup triage opportunity. During the investigation, I confirmed that the shared file filter excluded lowercase `testdata` directories but did not recognize capitalization variants such as `testData`. I also found that OSV filesystem scanning ran independently and did not use this filter.

The investigation developed into an implementation. I made test-data directory matching case-insensitive and added repository-scoped exclusions to OSV filesystem scans. I added regression tests for capitalization variants, nested directories, similarly named directories, and repositories beneath a `testdata` parent or whose root directory is named `testdata`. The tests verify that production binary-artifact and shell-script download findings remain reported, and that production packages remain queried by OSV.

Package tests passed with the race detector on Linux, and the eight OSV capitalization and absolute/relative path subtests passed with the race detector on Windows before the upstream merge. After merging upstream main, I resolved a test-file conflict and prevented the scanner tests from overlapping a test that changes OSV's global logger. The three affected packages then passed with the race detector on Linux. I submitted the changes in [PR](https://github.com/ossf/scorecard/pull/5317).

Before merging upstream main, I compared scans of the same clean IntelliJ Community checkout. Binary-Artifacts details decreased from 1,093 to 155, and Pinned-Dependencies details decreased from 135 to 54. After removing test-data entries from the baseline reports, the remaining details matched the modified reports after sorting. These counts include diagnostic messages, not solely scoring findings. Both check scores remained unchanged.

I also investigated the broader exclusion request. Existing maintainer annotations provide check-level explanations and cause Scorecard to omit annotated checks' alerts from SARIF output. They do not change check scores or support path-specific exclusions. Custom path exclusions and findings that remain visible without affecting scoring were not implemented in this PR.

I [asked](https://github.com/ossf/scorecard/issues/4036#issuecomment-6157563479) the maintainers whether repository-relative path exclusions for selected checks would be an appropriate direction for a separate follow-up, and whether there is an existing design or implementation to follow.
