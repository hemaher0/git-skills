---
name: using-git-worktrees
description: Use before Git work when deciding whether to reuse or create a branch or checkout, especially with parallel writers or unrelated local changes
---

# Using Git Worktrees

Choose the smallest workspace change that lets the task proceed safely. A new
task, an available worktree tool, or a subagent by itself is not a reason to
create a branch or worktree.

## Read Policy and Inspect State

Read the target repository's `AGENTS.md`, optional `AGENTS.local.md`, and any
referenced Git policy. Filled local fields specify that repository's branch,
parallel-agent, remote-Git, and escalation choices; placeholders supply no
rule and do not grant host permissions. Follow the user's existing instructions
and the host's actual permission controls.

Inspect the current branch or detached HEAD, `git status --porcelain -uall`,
`git worktree list --porcelain`, and available host-managed workspaces. Identify
which task or process owns each candidate checkout. Compare
`git rev-parse --git-dir` with `git rev-parse --git-common-dir` to detect a
linked worktree; use `git rev-parse --show-superproject-working-tree` to rule
out a submodule. Do not move or rename a host-managed detached checkout.

Use the repository's configured route for remote Git. Clone or fetch when the
task needs current remote data or a checkout; they do not by themselves require
a separate user decision. If sandbox DNS is known to be blocked, select the
configured working route before running the command. After a new failure, do
not retry the same blocked route. Use outside-sandbox execution according to
the user's instructions, any filled local policy, and actual host approval
rules; otherwise use an available host integration or report the specific
blocker. Do not put credentials in policy files or command output.

## Decide What to Reuse or Create

| Observed situation | Action |
| --- | --- |
| Read-only inspection or discussion | Reuse a checkout; create neither resource. |
| One writer has a suitable checkout and task branch | Reuse both. |
| Independent history or review is required, and the current checkout can switch safely | Reuse the checkout; create only the needed branch. |
| Another checkout is required by concurrent writers, unrelated local changes, a process using this checkout, or project policy | Reuse a suitable free checkout first; otherwise create a worktree and the branch required for that task. |
| Continuing an existing branch or pull request | Reuse its branch and suitable checkout, or restore its archived checkout. |

A checkout is suitable only if project policy allows its use, no other agent is
writing there at the same time, unrelated changes cannot enter this task's
commit or checks, and switching branches will not disrupt another task or
process. Task size alone does not decide whether isolation is needed. A small
change may need a branch under a pull-request policy; a large solo task may
need no new worktree. Do not create a fresh branch for each retry or a fresh
worktree when a free suitable one exists.

Create a task branch when the repository requires review, the user requests
separate history, the current branch is shared or protected, or concurrent
writers need independent commit histories. Otherwise keep a suitable current
branch. A worktree is for a second checkout, not a prerequisite for every new
branch.

For concurrent subagents, assign at most one active writer to a checkout.
Read-only agents may share it. The coordinator allocates writable workspaces
and serializes changes to shared target branches; a worker uses the assigned
path and branch and does not create extra resources without that assignment.
Sequential writers may reuse the same checkout after a verified handoff.

When the project has an assignment or ownership record, the coordinator must
update that existing record before dispatch or editing, including when reusing
a checkout. Record the owning task, actual path and branch, observed HEAD and
local state, and who controls its lifecycle. A checkout marked available in an
old record still needs inspection against Git and active processes. Use the
designated writer for a shared record; workers return observed events rather
than competing to edit it. If no such record exists, use available host task
attachments or the handoff below; do not invent another registry or history.

## Create Only the Missing Resource

For a new worktree, use an available host-managed workspace tool first. If no
such tool exists, follow the project's worktree location; otherwise choose a
path outside tracked source or a project-local path verified with
`git check-ignore`. Do not edit `.gitignore` just to use a preferred path.
Choose the branch base and name from the task and project policy; do not guess
the primary branch. If an essential base or target is genuinely unknown,
resolve that fact before creating a branch.

For newly created resources, record the actual branch, base SHA, checkout path,
owning task or agent, and whether the host or this workflow owns its lifecycle. Use host attachments
and the project's configured work-item record when available; do not create a
second document history. If neither exists, include the owner and resource
state in the task handoff and inspect Git's inventory before later reuse.
In a parallel task, the worker hands the observed branch, HEAD SHA, dirty
state, path, and checks back to the coordinator. A resource may remain for
review or reuse; finishing a task does not require immediate deletion or
archival.

If creation is blocked, use the host or project-approved route. Do not infer
permission to escalate from the task's need for a branch or worktree. Continue
in the current checkout only if the suitability conditions above still hold;
otherwise report the blocker without creating an untracked substitute.

Read the repository's setup instructions and lockfiles before dependency or
build commands. Run the narrowest useful baseline check and report the
workspace, branch, setup performed, and any baseline failure.

Before creating commits in the chosen workspace, use `git-skills:reviewing-before-commit`
to check actual staged content and messages for secrets and allowed disclosure
scope. Choosing a task branch or private checkout does not exempt its commits
from that review.
