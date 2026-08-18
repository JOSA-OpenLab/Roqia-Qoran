# Week 10 - Grype Contribution Proposal

## 1. The Project

I chose **Grype**, an open-source vulnerability scanner developed by Anchore. Grype scans container images, filesystems, and SBOMs for known vulnerabilities affecting operating-system packages and language-specific packages.

Grype uses package information such as name, version, type, ecosystem, and Linux distribution to match components against vulnerability advisories, helping developers and security teams identify vulnerable components, review severity and available fixes, and prioritize remediation.

Grype is developed and maintained by Anchore engineers together with contributors from the open-source community.

---

## 2. Why Me? Why Grype?

I previously used Syft to generate a CycloneDX SBOM for my phishing-training-platform project and scanned it with Grype. While investigating the results, I reviewed a reported vulnerability and noticed that scanning the entire project directory also included local development components. This experience showed me the importance of understanding the scan scope and validating how reported vulnerabilities relate to the actual project.

I want to continue with Grype because vulnerability scanning is directly connected to my interests in cybersecurity and software supply chain security. I am particularly interested in understanding how Grype identifies packages and matches them with vulnerability advisories. Through issue triage and code review, I want to investigate how package metadata, vendor-specific versions, advisory sources, filtering, and fix information affect scan results.

---

## 3. Selected Contribution Opportunities

The selected contributions include Issue #3530 for triage and several pull requests for code review, covering areas that affect scan accuracy such as package provenance, advisory interpretation, vendor-specific matching, filtering, and distribution support.

### **[Issue #3530](https://github.com/anchore/grype/issues/3530)** — Red Hat backported fix reported as vulnerable

Investigate a reported false positive involving a Red Hat backported fix and determine whether it originates from package metadata, advisory data, or Grype’s matching logic.

### **[PR #3601](https://github.com/anchore/grype/pull/3601)** — Prevent false positives for local Rust packages

Examine how Grype distinguishes local Rust packages from crates.io packages with the same name and version without affecting valid matches.

### **[PR #3633](https://github.com/anchore/grype/pull/3633)** — Parse disputed status from NVD `cveTags`

Understand how Grype parses NVD `cveTags`, determines vulnerability status, and handles the precedence of the disputed status.

### **[PR #3327](https://github.com/anchore/grype/pull/3327)** — Add RapidFort advisory matching

Examine how Grype identifies RapidFort-curated container images and integrates vendor-specific advisory matching without losing upstream distribution findings.

### **[PR #3584](https://github.com/anchore/grype/pull/3584)** — Fix vulnerability counts after filtering

Verify that vulnerability totals, severity counts, and fix-state summaries remain consistent after filtering and deduplication.

### **[PR #2849](https://github.com/anchore/grype/pull/2849)** — Add support for scanning openEuler container images

Understand how Grype adds support for a new Linux distribution and connects package matching with the corresponding advisory data.

---

## 4. 12-Week Milestones

### Weeks 1–2

- Set up the Grype development environment.
- Study the project architecture and contribution workflow.
- Reproduce the selected issue and relevant PR behavior.

### Weeks 3–5

- Trace the reported result through package metadata, advisory sources, and matching logic.
- Begin reviewing the selected PRs and discuss unclear behavior with maintainers.

### Weeks 6–8

- Complete the issue triage and the highest-priority code reviews.
- Reproduce relevant behavior, submit feedback, and respond to discussions.

### Weeks 9–10

- Review contribution updates and validate revised implementations and tests.
- Investigate an additional opportunity if the primary work is completed or blocked.

### Weeks 11–12

- Complete the remaining review cycles.
- Document the findings and contribution outcomes.

---

## 5. Risks

Several factors may affect the progress of this contribution plan:

- Reported issues may no longer be reproducible or may originate from external advisory sources.
- Investigations may involve multiple projects, including Syft, Grype, Vunnel, and `grype-db`.
- Missing package metadata or differences between SBOM formats may affect matching accuracy.
- Changes intended to reduce false positives may introduce false negatives.
- Vendor-specific versions and backported fixes may require deeper investigation.
- Design discussions and maintainer availability may delay review progress.

---

## 6. Mentorship Needed

I expect to complete most reproduction, advisory comparison, code tracing, and test validation independently. However, I may seek clarification through issue discussions, PR reviews, or Anchore community channels when I need to confirm the intended matching behavior, vulnerability-source precedence, or the preferred design for vendor-specific data.
