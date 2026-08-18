# Week 10 - OpenSSF Scorecard Contribution Proposal

## 1. The Project

I chose **OpenSSF Scorecard**, an open-source security tool that evaluates software projects against a set of security practices covering repository configuration, development workflows, and the software supply chain.

Scorecard analyzes repositories through checks such as dependency pinning, code review, branch protection, vulnerability detection, token permissions, security policies, and dangerous workflow patterns. It then produces structured findings that help maintainers identify security weaknesses and help users better understand the security posture of their dependencies.

The project is part of the Open Source Security Foundation and is maintained by the OpenSSF Scorecard maintainers, including **Stephen Augustus**, **Raghav Kaul**, **Adam Korczynski**, and **Spencer Schrock**, together with contributors from the wider open-source security community.

---

## 2. Why Me? Why OpenSSF Scorecard?

I first used Scorecard to examine checks such as `Pinned-Dependencies`, `Token-Permissions`, and `Dangerous-Workflow`. While investigating `Dangerous-Workflow`, I traced unsafe pull request metadata used directly in a shell command and contributed a fix that passed it through an environment variable instead.

I want to continue with Scorecard because it connects cybersecurity with software supply chain security. Even when source code is safe, dependencies, workflows, permissions, and release processes may introduce risks. Through triage, code review, and implementation, I want to understand how Scorecard collects evidence and produces its results, investigate inaccurate or unsupported cases, and become more confident working through the full contribution process.

The selected contributions provide a combination of issue triage, code review, and implementation. This will help me strengthen my ability to reproduce security-related problems, identify the responsible component, evaluate proposed fixes, and implement changes with appropriate regression tests.

---

## 3. Selected Contribution Opportunities

I will prioritize the following contribution opportunities based on reproducibility, current project status, maintainer feedback, and the opportunity to make a meaningful technical contribution.

### **[Issue #5090](https://github.com/ossf/scorecard/issues/5090)** — GitHub Actions dependencies reported as having unknown licenses

I selected this issue for triage to reproduce the reported unknown-license findings and determine whether they originate in Scorecard, package representation, or an external license-data source.

### **[Issue #4273](https://github.com/ossf/scorecard/issues/4273)** — Investigate GitHub commit status failures

I selected this issue for triage to verify whether the disabled end-to-end test still fails and whether the cause is GitHub API behavior, test data, or Scorecard's client logic.

### **[Issue #3946](https://github.com/ossf/scorecard/issues/3946)** — Vulnerable project receives a 10/10 Vulnerabilities score

I selected this issue for triage and design investigation to understand why the Vulnerabilities check returns a perfect score despite a known vulnerable package and whether the current check scope can reliably detect this case.

### **[PR #5163](https://github.com/ossf/scorecard/pull/5163)** — Factor private vulnerability reporting into the Security-Policy check

I selected this pull request for code review to examine how the new signal affects scoring, unsupported platforms, historical scans, and the existing `Security-Policy` behavior.

### **[Issue #1174](https://github.com/ossf/scorecard/issues/1174)** — Parse lock files and verify that dependency hashes are present

I selected this issue for implementation because a lock file may still contain dependencies without integrity hashes. I will begin with one agreed lock-file format and add focused verification and regression tests.

### **[Issue #4431](https://github.com/ossf/scorecard/issues/4431)** — Deprecated OSV Scanner grouping API

I selected this as a possible implementation task, but I will first confirm which current OSV Scanner API or local approach the maintainers prefer.

### **[Issue #3480](https://github.com/ossf/scorecard/issues/3480)** — Repository Rules `EnforceAdmins` logic is an over-approximation

I selected this as a possible implementation task because evaluating overlapping repository rules and bypass actors requires an agreed model before making changes.

### **[Issue #4036](https://github.com/ossf/scorecard/issues/4036)** — Findings in test-data paths affect the project score

I selected this as a backup triage opportunity to verify the current handling of test-data directories and determine whether the reported scoring behavior remains reproducible.

---

## 4. 12-Week Milestones

### Weeks 1–2

- Set up the Scorecard development environment.
- Study the project architecture and contribution workflow.
- Reproduce the selected triage issues.

### Weeks 3–5

- Investigate the reproduced issues.
- Review the selected pull request and its tests.
- Ask maintainers about unclear implementation decisions.

### Weeks 6–8

- Complete the selected issue triage and code review.
- Implement an agreed solution and add regression tests.
- Submit contributions and respond to feedback.

### Weeks 9–10

- Refine contributions based on review comments.
- Perform additional validation when requested.

### Weeks 11–12

- Complete the remaining review cycles.
- Document the contribution outcomes.

---

## 5. Risks

Several factors may affect the progress of this contribution plan:

- A reported issue may turn out to be intended behavior rather than a bug.
- The solution may require a broader design discussion or a different approach.
- Some issues may depend on external APIs or vulnerability data sources.
- Some contributions may require a deeper understanding of Scorecard internals.
- Review cycles may take longer depending on maintainer availability.

---

## 6. Mentorship Needed

While I expect to complete most of the investigation independently, I may occasionally seek clarification through issue discussions or pull request reviews when I need confirmation that my understanding of the expected behavior or proposed solution aligns with the project's design.
