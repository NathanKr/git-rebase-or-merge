
# Project Name

...

## Project Description

...

## Motivation

in many ocations you ask AI question and sometime it use git merge and somtime git rebase so question is
- what exactly each does 
- when to use each ?
- how commit playes in
- can we see it grphically

## Core Concepts: git merge vs git rebase

### git merge (preserves history)

Combines branches with a merge commit. Does not rewrite commits.

```
A---B---C---------M
         \       /
          D---E
```

Use when: working on shared/public branches, or you want accurate history.

### git rebase (rewrites history)

Replays commits on top of another branch. Creates new commit hashes.

```
A---B---C---D'---E'
```

Use when: cleaning up local branches, or before opening a PR.

### How commits behave

- merge → commits are preserved
- rebase → commits are rewritten (new hashes)

### Visualizing history

```bash
git log --oneline --graph --all
```

## Decision Guide

| Goal                  | Command  |
|-----------------------|----------|
| Update my branch      | rebase   |
| Combine branches      | merge    |
| Prepare clean PR      | rebase   |
| Finalize feature      | merge    |

## Key Takeaways
- [Item 1]
- [Item 2]

## Installation

...

## Usage

...

## Technologies Used

- git

## Code Structure

...

## Demo

...

## Points of Interest
- [Item 1]
- [Item 2]

## References
- [Link/Reference 1]
- [Link/Reference 2]
