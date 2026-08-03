# Week 10 - Git Internals & Modern Workflow: Contribution Proposal

## 1. The Project

I chose **pytest**, an open-source testing framework for Python.

Pytest is used to write, organize, and execute automated tests for Python applications. While it is well known for its simple syntax, it also provides advanced capabilities such as fixtures, parametrization, plugin support, flexible test discovery, detailed reporting, and an extensible hook system. These features make it suitable for projects ranging from small scripts to large-scale applications and widely adopted open-source libraries.

Pytest is widely used throughout the Python ecosystem by individual developers, open-source projects, research groups, and software companies to improve software quality and reliability through automated testing. Its plugin architecture also makes it a foundation for many other testing tools and workflows.

The project is maintained by a team that includes **Ronny Pfannschmidt**, **Bruno Oliveira**, **Freya Bruhin**, and **Zac Hatfield-Dodds**, together with many contributors from the Python open-source community.

---

## 2. Why Me? Why pytest?

Most of my recent open-source work has been in Python projects, so contributing to pytest is a natural opportunity to better understand the testing framework used throughout that ecosystem.

This project also aligns well with my long-term career goals in cybersecurity. Python is widely used in cybersecurity for automation, analysis tools, scripting, and testing. A stronger understanding of automated testing will help me build more reliable security tools and improve the quality of my own projects in the future.

Beyond improving my technical skills, I also want to gain practical experience contributing to a large open-source project by participating in issue discussions, submitting pull requests, and learning from maintainer feedback.

---

## 3. Specific Issues

My goal is to begin with investigation and reproduction before committing to a specific implementation, since the expected solution may change after maintainer feedback.

### **[Issue #13754](https://github.com/pytest-dev/pytest/issues/13754)** — Module-scoped fixtures torn down early with `pytest_generate_tests()`

I selected this issue because it has a clear reproducer, affects a core pytest feature (fixtures and parametrization), and offers a realistic opportunity to contribute code, regression tests, or documentation depending on the outcome of the investigation.

### **[Issue #6997](https://github.com/pytest-dev/pytest/issues/6997)** — Incorrect xfail test case count after fixture teardown

I selected this issue because it requires understanding pytest's reporting pipeline and execution phases, making it a good opportunity to learn core internals while working on a well-defined reproducible problem.

### **[Issue #11071](https://github.com/pytest-dev/pytest/issues/11071)** — `conftest.py` discovery with `--rootdir`

I selected this issue because it provides an opportunity to understand pytest's configuration and collection process. Even if the behavior proves to be intentional, it may still lead to improvements in documentation, diagnostics, or regression tests.

### **[Issue #13693](https://github.com/pytest-dev/pytest/issues/13693)** — Live logging race condition with background threads

I selected this issue because it involves concurrency, logging, and output capture, which are important internal components of pytest. Since a previous implementation attempt was not merged, I plan to begin with investigation and maintainer discussion before proposing any changes.

### **[Issue #14289](https://github.com/pytest-dev/pytest/issues/14289)** — `logging.basicConfig()` before `pytest.main()` causes WinError 6 on Windows

I selected this issue because it involves the interaction between pytest's capture system and Python's logging infrastructure. The investigation may lead to an implementation, improved diagnostics, or documentation, depending on the investigation results and maintainer feedback.

### **[Issue #6626](https://github.com/pytest-dev/pytest/issues/6626)** — Parametrized IDs containing parentheses

I selected this as a lower-risk fallback because it has a deterministic reproducer and may result in a documentation or usability improvement if the higher-priority investigations require larger design discussions.

I intentionally selected issues that are reproducible, affect core pytest functionality, and span multiple subsystems rather than focusing on a single component. This increases the likelihood of making a meaningful contribution even if one investigation concludes that the reported behavior is intentional.

---

## 4. 12-Week Milestones

### Weeks 1–2

- Set up the pytest development environment.
- Study pytest's architecture and contribution workflow.
- Reproduce the selected issues.

### Weeks 3–5

- Investigate the selected issues.
- Identify the expected behavior and potential implementation approaches.
- Discuss implementation ideas with maintainers when clarification is needed.

### Weeks 6–8

- Implement the agreed solution or improvements for the selected issue(s).
- Add regression tests to prevent future regressions.
- Submit contributions and respond to feedback.

### Weeks 9–10

- Refine contributions based on review comments.
- Investigate additional issues if time permits.

### Weeks 11–12

- Complete remaining review cycles.
- Document lessons learned and reflect on the contribution process.

---

## 5. Risks

Several factors may affect the progress of this contribution plan:

- The selected issue may turn out to be intended behavior rather than a bug.
- The implementation may require a larger design discussion or a different approach than initially expected.
- Some issues may require a deeper understanding of pytest internals before implementation is possible.
- Review cycles may take longer than expected due to maintainer availability.
- Existing implementation attempts or previous design discussions may require additional investigation before proposing a new solution.

---

## 6. Mentorship Needed

While I expect to complete most of the investigation independently, I may occasionally seek clarification through issue discussions or pull request reviews when I need confirmation that my understanding of the expected behavior or proposed solution aligns with the project's design.
