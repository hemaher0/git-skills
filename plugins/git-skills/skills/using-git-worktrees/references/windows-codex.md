# Git execution in native Windows Codex

Use this when the native Windows terminal tool shows a PowerShell wrapper
for a Git command or asks again for a Git operation that the user already
allowed. Keep the workspace and sandbox selected for the task.

## Call Git directly

- Run one Git command per execution call. Set the tool's `workdir` to the
  checkout that owns the change. On a host configured for native PowerShell,
  leave `shell` unset to inherit it. The terminal tool can still show a
  bundled `pwsh.exe` path in the approval prompt.
- Quote Git's literal pathspec separator as `'--'` in the PowerShell command
  text so Git receives the argument `--`. Use it wherever the command
  requires the pathspec separator, including after `--dry-run`.
- Use the tool's output and exit status. Read or save evidence separately if
  needed. Keep pipelines, shell redirection, and setup scripts out of this
  call.

```powershell
git status --short '--' README.md
git add --dry-run '--' README.md
git add '--' README.md
```

Run the last command only when staging that path is in scope. Review the
staged diff before committing, following the applicable repository policy.

## Handle Git metadata and approval

A linked worktree can write to a Git directory shared with its primary
checkout. Compare `git rev-parse --git-dir` and
`git rev-parse --git-common-dir` in the affected checkout when diagnosing
a denied write. Worktree placement, repository metadata write permission,
and whether the execution tool matches a command rule are separate checks.

Start with sandboxed execution when it supports the requested operation.
If an authorized write requires the Windows host's approval flow, request
it for the direct command in the same checkout. When offering a reusable
prefix for staging is appropriate, use `["git", "add"]` in `prefix_rule`.
That field proposes a scope to approve. The runtime decides whether an
existing rule matches and whether to ask. If the prompt still offers a
versioned PowerShell path and a script body, inspect that invocation before
retrying. Keep approvals scoped to the intended Git operation.
When testing a rule, keep the checkout and tool options fixed. A static
prefix check evaluates the supplied argument list. Verify the actual
terminal invocation to check how the host parses it.

## Evidence and limits

On a native Windows Codex Desktop host using bundled PowerShell 7, the same
`git add --dry-run` call in the same worktree prompted with a bare `--` and
ran under an existing approval with `'--'`. The quoted form ran in another
worktree as well. Use this comparison for Git's literal pathspec
separator and verify other options against the actual invocation.

- [Codex issue #15298 — quoted and unquoted Git separators on Windows](https://github.com/openai/codex/issues/15298)
- [Codex issue #19715 — bundled PowerShell wrapper in approvals](https://github.com/openai/codex/issues/19715)
