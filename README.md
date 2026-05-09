
# Project Name

...

## Project Description

A visual, command-based learning project for understanding `git merge` and `git rebase` through commit graphs, real repository history, and direct experimentation.

The project explains how commits, branches, and history rewriting work internally — helping developers build an intuitive mental model of Git instead of memorizing commands.

## Motivation

Git tutorials and AI tools often recommend both `git merge` and `git rebase`, but the difference between them can feel unclear.

This project explores:
- what each command actually does
- how commits and branches behave
- why rebase rewrites history
- how Git history forms a graph

using direct command-based experimentation and visualization.

## Key Takeaways
- Rule of thumb: local/feature branch → rebase; shared/public branch → merge.
- Rebase branches you fully control; merge branches others depend on.
- Rebase is often used before merge — clean up your branch first, then merge into the target branch (never rebase a branch others are actively using).

## Core Concepts: git merge vs git rebase

> Notation: each letter (A, B, C…) represents a commit; `---` is the chain over time (left = oldest, right = newest).

### git merge (preserves history)

Combines branches with a merge commit. Does not rewrite commits.

> `feature` branch merges into `main`

**Before merge:**

```mermaid
gitGraph
   commit id: "A"
   commit id: "B"
   commit id: "C"
   branch feature
   checkout feature
   commit id: "D"
   commit id: "E"
```

**After merge** (history stays branched):

```mermaid
gitGraph
   commit id: "A"
   commit id: "B"
   commit id: "C"
   branch feature
   checkout feature
   commit id: "D"
   commit id: "E"
   checkout main
   merge feature id: "M"
```

> `M` = merge commit (auto-created by git to join the two branches)

Use when: working on shared/public branches — merge preserves the full history so others can see exactly what happened and when.

### git rebase (rewrites history)

Replays commits on top of another branch. Creates new commit hashes.

> `feature` branch rebases onto `main`

**Before rebase:**

```mermaid
gitGraph
   commit id: "A"
   commit id: "B"
   commit id: "C"
   branch feature
   checkout feature
   commit id: "D"
   commit id: "E"
```

**After rebase** (history becomes linear — D1 and E1 have the same changes as D and E but with new hashes):

```mermaid
gitGraph
   commit id: "A"
   commit id: "B"
   commit id: "C"
   commit id: "D1"
   commit id: "E1"
```

Use when: cleaning up local branches or before opening a PR — rebase makes history linear so reviewers see a clean, easy-to-follow commit sequence.

### How commits behave

- merge → commits are preserved
- rebase → commits are rewritten (new hashes)

### Visualizing history

```bash
git log --oneline --graph --all
```

## Decision Guide

| Goal                  | Command  | Why                                     |
|-----------------------|----------|-----------------------------------------|
| Update my branch      | rebase   | Keeps history linear, no merge commit   |
| Combine branches      | merge    | Preserves full history of both branches |
| Prepare clean PR      | rebase   | Cleaner diff for reviewers              |
| Finalize feature      | merge    | Creates explicit record of the merge    |



## Usage

```bash
git log --oneline --graph --all -5  # visualize branch history
git merge feature                   # merge feature into main (run from main)
git rebase main                     # rebase feature onto main (run from feature)
git log --oneline --graph -7        # inspect merge result
git log --oneline --graph -5        # inspect rebase result
```



## Demo

### Setup

#### 1. Create `main` branch with a few commits and run `git log --oneline --graph --all -5` to visualize

![git log main demo start](./figs/git-log-main-demo-start.png)

Observe:
- `main` has a linear sequence of commits
- no branches yet — history is a single chain

#### 2. Branch off to `feature`, add 2 commits, then add 1 more commit to `main` so branches diverge

#### 3. Run `git log --oneline --graph --all -5` to visualize diverged state

![git log show main and feature commits](./figs/git-log-show-main-and-feature-commits.png)

Observe:
- `main` and `feature` share a common ancestor but have diverged
- both branches have commits the other doesn't
- Git history is a graph, not a list

### git merge

#### 4. `git checkout main && git merge feature`
#### 5. Run `git log --oneline --graph -7` to see merge commit M
> `--all` omitted — shows only main's result after merge, not all branches.

![git log after merge show merge commit](./figs/git-log-after-merge-show-merge-commit.png)

Observe:
- a new merge commit M ties both branch tips together
- both branches' commits are preserved with their original hashes
- the graph forks and rejoins — history is non-linear

### git rebase

#### 6. Reset to before-merge state
#### 7. `git checkout feature && git rebase main`
#### 8. Run `git log --oneline --graph -5` to see linear history
> `--all` omitted — shows only feature's linear history after rebase.

<!-- add image here -->

Observe:
- feature's commits (D, E) were replayed on top of main as D1, E1 — new hashes, same changes
- history is now a straight line — no fork, no merge commit
- the original D and E commits no longer exist on this branch



## References
- [git-merge documentation](https://git-scm.com/docs/git-merge)
- [git-rebase documentation](https://git-scm.com/docs/git-rebase)
- [Understanding Git Commits: A Practical Guide](https://youtu.be/6tflUkytj9Q)
