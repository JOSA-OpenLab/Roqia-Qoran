## OpenSSF Scorecard contributions

This week, I worked on three issues from my Scorecard contribution proposal. I investigated the problems, made changes, and added tests to check them. One additional issue is still pending.

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

### [Issue #4036](https://github.com/ossf/scorecard/issues/4036) — Pending
