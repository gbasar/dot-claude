---
name: git-repo-aware
description: Preserve Git worktree context from planning through staged implementation and larger verified commits. Use when beginning work in a directory, changing directories, or modifying a Git repository. Report pre-existing changes and unusual branch state, keep unrelated work separate, stage approved work after verification, and commit at larger checkpoints, especially before a new task.
---

# Git Repo Aware

Keep repository state visible without treating every warning as a blocker or taking
ownership of changes that were already present.

## On entry

When work begins in a directory, and whenever the working directory changes:

1. Determine whether the directory is inside a Git worktree.
2. If it is, inspect the repository root, branch, and concise working-tree status.
3. Report staged, unstaged, untracked, conflicted, or in-progress Git operations.
4. Treat everything present at the first inspection as pre-existing work unless there
   is evidence that this agent created it.
5. Keep later work separate from that baseline.

A dirty worktree is an alert, not automatically a blocker. Stop for direction when the
requested work would overlap, overwrite, or make ownership of existing changes unclear.
Never stash, reset, discard, stage, or commit pre-existing work merely to obtain a clean
tree.

## First visit of the day

On the first visit to a repository during the local calendar day, report when:

- the current branch is neither `main` nor `master`;
- HEAD is detached; or
- a merge, rebase, cherry-pick, bisect, or revert is underway.

Use neutral, non-preachy language. A non-primary branch may be intentional. State the
fact and use judgment about whether the requested task belongs there; do not switch
branches automatically.

If persistent daily state is unavailable, treat the first visit in the current session
as the first visit of the day.

Example:

> Heads-up: today's first visit to `avocado` is on `feature/control`, not `main` or
> `master`. That may be intentional; I will verify that this task belongs here before
> changing files.

## Move from planning to staged work

Use this progression:

    planning → working tree → staging area → commit

- **Planning:** Inspect, reason, and propose. Do not modify files or the staging area.
- **Implementation:** When the user says `do it`, `apply it`, or gives an equivalent
  instruction, make the approved changes in the working tree.
- **Staging:** After the approved changes pass appropriate verification, stage only the
  explicit agent-owned paths and inspect the staged diff.

Authorization to implement includes normal staging of the verified, in-scope changes. It
does not authorize staging pre-existing or unrelated work, or creating a commit before a
checkpoint.

## Commit larger checkpoints

Do not commit every small completed change. Commit a larger, reviewable set of related
work when:

- it has one clear outcome;
- verification proportional to the combined change has passed;
- it can be reviewed and reverted independently;
- it contains no unrelated or pre-existing changes; and
- the user requests a commit, or the work has reached a natural tested milestone.

Treat the user's phrase `New task` as a checkpoint signal. Before starting the new task,
inspect the previous task's staged and unstaged changes. Commit completed, verified,
agent-owned work from the previous task. If that work is incomplete, failing, forbidden
from commit, or has unclear ownership, do not force a commit; report the condition before
starting the new task.

Before committing:

1. Inspect the diff.
2. Correct spelling errors in agent-owned prose included in the intended chunk. Do not
   alter identifiers, quoted material, generated data, or pre-existing work to make this
   correction.
3. Run verification proportional to the change.
4. Stage explicit owned paths. Do not use broad staging in a dirty worktree.
5. Inspect the staged diff and confirm that it contains only the intended chunk.
6. Commit with a message that describes the outcome. Begin the subject with the
   committing agent's identifier followed by a colon and a space, for example,
   `codex: describe the outcome`.

Do not commit when the user or repository forbids it, verification is failing, the chunk
is incomplete, or ownership of overlapping changes is unclear. Report the reason
instead.

## Reporting

Keep alerts factual and short. For example:

> Existing worktree state: 2 modified, 1 staged, and 3 untracked files. I will treat
> these as pre-existing and keep my changes separate.

After a commit, report its hash and what the chunk accomplished.
