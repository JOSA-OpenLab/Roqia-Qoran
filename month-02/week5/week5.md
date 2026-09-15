# Week 5 - Testing, CI/CD, and GitHub Actions

## Task 1 - Add tests where there are none

**Issue:** httpie/cli #1539  
**Issue link:** https://github.com/httpie/cli/issues/1539

**PR:** https://github.com/httpie/cli/pull/1881

In this PR, I added regression tests for `_discover_system_pip()` covering two scenarios:

- Falling back to `pip3` when `pip` is unavailable.
- Skipping a Python 2 `pip` and using `pip3` instead.

---

## Task 2 - Build a GitHub Actions workflow

For this task, I chose my repository "phishing-training-platform" to implement the required workflow.

What I did:

First, I added a small test and ran `python manage.py test` to verify that it passed successfully.

The workflow includes:

- Ruff linting
- Django tests
- Python version matrix (3.12, 3.13)
- pip cache
- CI status badge in the README


**Workflow:** https://github.com/royy92/phishing-training-platform/blob/main/.github/workflows/ci.yml

**Repository:** https://github.com/royy92/phishing-training-platform

---

## Task 3 - Local workflow reproduction with act

- Installed act and Docker.
- Ran the workflow locally before pushing.

- Issues encountered:
  - Docker daemon was not running.
  - Permission denied for the Docker socket, so I used `sudo`.
  - `ruff` was not installed in the workflow.
  - PAT required the `workflow` permission to push `.github/workflows/ci.yml`.

- Final result:
  - Workflow completed successfully locally with `act`.
  - Workflow also passed on GitHub Actions.
