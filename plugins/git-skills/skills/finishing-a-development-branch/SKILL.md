---
name: finishing-a-development-branch
description: Use when branch work needs a verified handoff, pull request, integration decision, or intentional preservation
---

# Finishing a Development Branch

Separate routine work on a task branch from changing the repository's primary
branch. Finishing a task does not itself authorize a merge, direct commit, or
push to the primary branch, and it does not require immediate branch or
worktree cleanup.

A routine authorized commit uses the commit-review workflow without requiring
this branch-finishing procedure. Use this skill when a handoff, publication,
integration, or preservation outcome needs coordination.

## Establish the Actual State

Read applicable repository instructions, including `AGENTS.md`,
`AGENTS.local.md` when present or required, and referenced Git policy. Check the
target, review path, integration method, execution, and retention rules that
apply to the chosen outcome.

Resolve an essential missing fact, rule, or authorization before the operation
it affects; unrelated optional settings do not block the task. Honor an
integration choice already made within existing authority and actual host
controls. Read-only inspection may proceed while dependent work is unresolved.

Inspect the branch or detached HEAD, intended target, HEAD and target SHAs,
uncommitted and untracked changes, upstream and remote state, attached pull
request, and workspace owner. Do not assume that the target is named `main` or
that a directory name proves worktree ownership. Use the configured route for
remote checks or fetches, and do not repeat a known blocked DNS route.

Run the project's required gate and checks relevant to the exact tree being
handed off. Report failed or unavailable checks rather than claiming the
branch is ready. Review the diff against the intended target, including any
work that has not been committed.

## Hand Off Parallel Work

A writing subagent may commit on its assigned branch under the task and
repository policy, after applying `git-skills:reviewing-before-commit` to the candidate
tree, message, and disclosure scope. It returns the actual branch or detached
commit SHA, checkout path, dirty state, changed files, check results, and the
commit review's scope and coverage. The coordinator
checks that state independently and serializes integration into a shared
target. A worker does not merge, fast-forward, cherry-pick, commit directly
onto, or push the primary branch on its own.

When project instructions configure work items, forward actual branch names,
target and result SHAs, checks, and merge or abandonment outcomes to the
assigned work-item owner. In parallel work, use linked child records when the
local schema supports them; otherwise send events to one designated writer.
Use the project's `.docs-schema` if no document workflow is installed. Do not
create a document branch or a second work history.

## Choose the Outcome

- **Keep working or preserve:** Report the branch or commit SHA and workspace.
  An available checkout can be reused later; an open pull request can keep its
  workspace for feedback. Do not delete or archive merely because this task
  ended or a pull request was opened or merged.
- **Request review:** Confirm the diff and target, follow the repository's
  publication rules, and verify that the outgoing commits and destination
  match their existing content reviews before pushing or publishing through a
  host integration. Review the pull
  request's own audience and text before creating it. Opening a pull request
  does not change the primary branch. Preserve the branch and a usable
  workspace for follow-up.
- **Integrate locally or merge a pull request:** Read the project's history
  shape and allowed integration methods. Use only a method allowed for that
  delivery path and current Git state, after the primary-branch decision gate
  below. A linear-history rule excludes merge commits from the resulting
  primary history, including commits introduced while updating the task
  branch; permission for merge commits does not itself choose one. Verify the
  result on the target branch before considering any cleanup.

Check the permitted task-branch update strategy as well as the target's
integration method. A fast-forward-only rule does not by itself authorize
rebasing, squashing, or otherwise rewriting the source to make that update
possible. Source preparation is allowed when the repository policy or user's
existing instruction permits it. If no allowed preparation and integration
can produce the required history, present the concrete conflict for a decision
before rewriting the source or changing the target. An unpublished branch
does not override the configured method constraints.

### Primary-Branch Decision Gate

Before an action that changes the primary branch ref, identify the exact
target, source branch or SHA, changed files, check results, and integration
method. This includes local merge, fast-forward, cherry-pick, direct commit or
push, and pull-request merge. If the user has not already authorized this
integration, present that concrete result and obtain the decision before the
mutation. Prior explicit authorization counts; do not ask again just because
this skill runs. Creating a branch, worktree, branch commit, or pull request is
not this gate. A sandbox or host escalation request is a separate permission
matter governed by the local policy and actual host controls.

If integration creates a commit, apply `git-skills:reviewing-before-commit` before its
creation, including merge, squash, cherry-pick, and resolved rebase commits.

If the target, history shape, or permitted method is genuinely unclear or the
policy fields conflict, resolve that before integration rather than guessing
a merge strategy.

If checks fail or the target moves, inspect the new state and re-evaluate the
reviewable result. Do not force-push a protected branch or bypass a failed
check. For linked OpenSpec changes, pass the adopted implementation, merged
SHA, checks, and delta spec to its sync/archive workflow; unadopted
requirements do not enter main specs.

## Retain and Clean Up Deliberately

Keep each created resource identifiable by its owning task or pull request and
actual branch or commit SHA. A free, suitable active worktree may be reused.
After a verified handoff, have the designated owner update an existing
assignment record with the observed SHA, dirty state, and retention or reuse
decision. Reused resources need this update too. Finishing work does not turn
a resource into an unowned or available one; release it for another writer
only after its work and ongoing uses have been accounted for.
When cleanup is useful, act only on a workspace this workflow owns; use the
host's archive or cleanup tool for host-created workspaces. Check for running
processes, unique commits, uncommitted changes, and untracked or ignored files
that an archive would not preserve. A merged pull request alone does not prove
the worktree is free or the branch is safe to delete.

For irreversible deletion of unique work, show the exact branch, commits,
changes, and workspace that would be lost. Obtain the user's explicit discard
decision; otherwise preserve the work. Record an abandoned branch's last SHA
and reason before any authorized deletion. If a worker stops unexpectedly,
inspect its actual checkout and branch before reuse or cleanup rather than
assuming its work is empty.
