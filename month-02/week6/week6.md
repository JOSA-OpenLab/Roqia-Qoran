# Week 6 - Documentation as Code & Technical Writing

## Task 1 - One real docs PR

**Issue:** https://github.com/mdn/content/issues/36500  
**PR:** https://github.com/mdn/content/pull/44621

---

## Task 2 - Set up a docs site

For this task, I used my repository `phishing-training-platform` and set up a MkDocs Material documentation site. I also linked the documentation site from the project README.

The documentation contains:

- A tutorial
- A how-to guide
- A reference guide

**Repository:** https://github.com/royy92/phishing-training-platform  
**Documentation site:** https://royy92.github.io/phishing-training-platform/

---

## Task 3 - Write an ADR

I wrote an Architecture Decision Record to document why Django was chosen as the web framework for the phishing training platform.

**ADR:** https://github.com/royy92/phishing-training-platform/blob/main/docs/adr/0001-use-django.md

---

## Task 4 - Add Vale to CI

I configured Vale with the Microsoft style guide, ran it on the project documentation, resolved all Vale suggestions, and integrated Vale into the existing GitHub Actions CI workflow.

**Successful CI run:** https://github.com/royy92/phishing-training-platform/actions/runs/28695568634/workflow
