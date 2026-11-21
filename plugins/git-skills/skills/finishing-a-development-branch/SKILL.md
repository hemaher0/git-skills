---
name: finishing-a-development-branch
description: Use when branch work needs a verified handoff, pull request, integration decision, or intentional preservation
---

# Finishing a Development Branch

For a host-listed companion, read or invoke its exact installed name and path
from that list. Do not substitute an adjacent vendor or cache copy.

Separate routine work on a task branch from changing the repository's primary
branch. Finishing a task does not itself authorize a merge, direct commit, or
push to the primary branch, and it does not require immediate branch or
worktree cleanup.

A routine authorized commit still needs the repository's candidate content,
message, secret, and disclosure review, but it does not require this branch-
finishing procedure. Use this skill when a handoff, publication, integration,
or preservation outcome needs coordination.

## Establish the Actual State

Read applicable repository instructions, including whichever of `AGENTS.md` and
`CLAUDE.md` exist, `AGENTS.local.md` when present or required, and referenced
Git policy. Check the target, review path, integration method, execution, and
retention rules that apply to the chosen outcome.

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
repository policy after the candidate tree, message, secrets, and disclosure
scope have been reviewed. Use `reviewing-before-commit` when the current host
lists it as available; do not infer availability from a skill folder in a
vendor tree or cache. Otherwise follow the project's equivalent procedure or
inspect the staged paths, diff, relevant staged blobs, and message directly,
run configured index-aware secret checks, exclude prohibited or out-of-scope
content, and verify that the resulting commit matches the reviewed candidate.
The subagent returns the actual branch or detached commit SHA, checkout path,
dirty state, changed files, check results, and the commit review's scope and
coverage. The coordinator
checks that state independently and serializes integration into a shared
target. A worker does not merge, fast-forward, cherry-pick, commit directly
onto, or push the primary branch on its own.

When project instructions configure work items, forward actual branch names,
target and result SHAs, checks, and merge or abandonment outcomes to the
assigned work-item owner. In parallel work, use linked child records when the
local schema supports them; otherwise send events to one designated writer.
Follow the documentation or record workflow listed by the current host or
designated by project instructions. If neither supplies one, use an existing
project `.docs-schema` procedure when configured; otherwise keep the required
events in the existing handoff or work record. Do not look for an unlisted
workflow in vendor or cache directories, and do not create a document branch
or a second work history.

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

If integration creates a commit, use `reviewing-before-commit` before creation
when the current host lists that skill as available. Otherwise apply the
project's equivalent procedure or directly review the exact generated tree,
message, secrets, and disclosure scope before merge, squash, cherry-pick, or
resolved rebase commits. Keep required hooks and index-aware checks enabled,
and verify the resulting commit against the reviewed candidate.

If the target, history shape, or permitted method is genuinely unclear or the
policy fields conflict, resolve that before integration rather than guessing
a merge strategy.

If checks fail or the target moves, inspect the new state and re-evaluate the
reviewable result. Do not force-push a protected branch or bypass a failed
check. For linked OpenSpec changes, follow the project's configured OpenSpec
sync/archive procedure when it is available. Otherwise return the adopted
implementation, merged SHA, checks, and delta spec to the designated OpenSpec
owner for later processing; do not search vendor or cache files for an
unlisted workflow. Unadopted requirements do not enter main specs.

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
