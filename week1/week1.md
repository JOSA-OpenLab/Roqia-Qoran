# Week 1 - Git Internals & Modern Workflow

## Git Archaeology Reflection

Before this, I mainly used Git to push projects to GitHub and collaborate with teammates. I thought Git was simply storing folders and files. After exploring commits, trees, and blobs with `git cat-file`, I understood that Git actually works using objects and pointers connected through hashes, which made its behavior much more logical to me.

---

## Walking Through Git Objects

### Commit Object

```bash
git cat-file -p fefaba592
```

![Commit Object](images/git-log1.png)

---

### Tree Object

```bash
git cat-file -p c0dcca8f5875f65a65898ee540a0efaaf1eaa97a
```

![Tree Object](images/git-log2.png)

---

### Blob Object

```bash
git cat-file -p 8f4cf3302aef2f43b25be97a778e683c80aca20e
```

![Blob Object](images/git-log3.png)

---

## Reflog Rescue Drill
I created multiple commits, then intentionally broke the branch using:

```bash
git checkout -b reflog-practice
```

![1](images/reflog-recovery1.png)



```bash
git log --oneline
```

![2](images/reflog-recovery2.png)



```bash
git reset --hard HEAD~2
```

![3](images/reflog-recovery3.png)


```bash
git reflog
```

![4](images/reflog-recovery4.png)


```bash
git reset --hard 2189cf06b
git log --oneline
```

![5](images/reflog-recovery5.png)

---

## Interactive Rebase

I used interactive rebase to clean the commit history, rename commits using Conventional Commits, and squash unnecessary commits.

### Creating Multiple Test Commits

![0](images/interactive-rebase.png)

Open the last 5 commits to edit:

```bash
git rebase -i HEAD~5
```

![1](images/interactive-rebase1.png)

### Editing Rebase Instructions

![2](images/interactive-rebase2.png)

### After Rebase

![3](images/interactive-rebase3.png)

---

## Final Clean Commit History

```bash
git log --oneline
```

![Final Git Log](images/interactive-rebase4.png)
