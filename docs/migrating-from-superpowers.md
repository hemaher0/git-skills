# Moving Git Workflows from Superpowers

Assign one provider to branch, checkout, commit, publication, integration, and
Git-resource lifecycle decisions. Keep planning, implementation, debugging,
TDD, code review, and verification in Superpowers. Describe those Git
responsibilities in consumer instructions so they also work without Git Skills
installed. Resolve an available skill only when that responsibility applies.
A plugin namespace distinguishes two skill names, but it does not redirect a
caller that explicitly requests `superpowers:using-git-worktrees`.

## Identify the Actual Overlap

Inspect the versions being distributed and loaded. The following differences
were observed in Superpowers 6.4.2 and the personal
6.2.0+codex.20260811091828 package; they are not claims about every fork.

| Existing Superpowers instruction | Git Skills behavior | Migration consequence |
| --- | --- | --- |
| Execution workflows require an isolated workspace; the worktree skill proposes one from a normal checkout without an existing preference. | Decide whether a branch or another checkout is needed; reuse a suitable checkout. | Move the workspace decision and its callers together. |
| Add and commit `.gitignore` when the preferred local worktree directory is not ignored. | Choose an approved location; do not make an incidental `.gitignore` change. | Remove this automatic commit recipe from the Git provider. |
| Run dependency installation from detected manifests. | Read project setup instructions and run only needed, authorized setup. | Keep project setup under the implementation workflow. |
| Fall back to the current checkout after a worktree permission failure. | Recheck whether that checkout is suitable; concurrent writers or unrelated changes can make it unusable. | Preserve the isolation requirement when creation is blocked. |
| Present a fixed finishing menu, then use a generic `git pull` / `git merge` recipe. | Honor a prior integration choice and check repository history shape, allowed method, and target. | Update finishing calls and integration recipes. |
| After local merge, clean up the worktree and delete the branch; directory names establish cleanup ownership. | Retain useful resources and verify ownership and actual Git state before cleanup. | Transfer ownership and handoff rules, not just command text. |

The inspected `6.2.0-hemaher0.2` fork already omits the two Git skill
definitions and optionally calls `git-skills:using-git-worktrees` and
`git-skills:finishing-a-development-branch`. That fork is already partly
separated. Its commit-producing paths still need the pre-commit content and
disclosure review contract when adopting this version of Git Skills. Re-audit
after upstream updates.

## Migration Procedure

1. **Inventory definitions and callers in the source packages.** Search skill
   names, worker prompts, examples used as execution recipes, diagrams,
   reference files, and scripts. Include `executing-plans`, `writing-plans`,
   `subagent-driven-development`, and review-agent instructions. Identify
   commands that create commits, publish refs, or create and remove Git
   workspaces. Editing an installed cache is temporary and can be overwritten;
   make durable changes in the package source.

2. **Record the repository's Git provider and policy.** Fill the relevant
   fields in the [AGENTS.local.md template](../plugins/git-skills/templates/AGENTS.local.md).
   Establish the primary branch, workspace ownership, active writers, working
   remote route, commit secret and disclosure checks, intended audiences,
   history shape, integration methods, and how prior approval is recognized. Leave actual
   host permissions under the host's controls. A template does not grant them.
   Specify task-branch updates separately: allowing a target fast-forward does
   not itself select a source rebase or squash. For example, a policy can allow
   rebase onto the target followed by fast-forward, or preserve source history
   and require another decision when fast-forward would introduce merge commits.

3. **Redirect operational callers by responsibility.** Use the portable
   contract below in execution skills and worker prompts. Select the
   repository's configured Git provider from the skills actually available in
   the session, matching their descriptions to the needed responsibility. If
   no provider is configured, use available Git guidance compatible with the
   user and repository policy; do not inherit a conflicting isolation, merge,
   or cleanup recipe just because its name matches. Qualify the selected skill
   name when invoking it to distinguish overlapping packages.

   If the selected provider is absent, carry out the same contract with
   repository instructions and available host or Git tools. Absence alone is
   not a reason to install a plugin, ask for permission, skip a check, or stop
   the task. A missing essential repository decision or actual host permission
   is still handled under its own policy. One provider owns the Git decisions;
   host tools execute them and do not constitute a competing policy.

   Update an existing resource assignment record before dispatch or editing,
   even when reusing a checkout. Keep the owner, actual branch and SHA, local
   state, and handoff or reuse outcome current through its designated writer.
   A stale `available` entry does not establish that a checkout is free.

4. **Preserve the development contracts.** Keep task ordering, tests, review
   packages, exact result SHAs, and implementation verification in Superpowers.
   Workers return their actual branch, checkout, dirty state, and checks to the
   coordinator. The coordinator owns allocation and serializes integration.
   Read-only `git show`, `git diff`, and `git log` in a code review remain review
   tools; moving Git ownership does not require removing them. If a reviewer
   needs a separate checkout, use the Git provider's allocation and ownership
   procedure. A plan's temporary ledger directory is distinct from a Git
   worktree; keep its document lifecycle with its existing owner.

5. **Switch the published skill catalog after callers are updated.** Remove
   the duplicate `using-git-worktrees` and `finishing-a-development-branch`
   implementations from the Superpowers package that will be distributed.
   If older callers need a transition adapter, make it delegate to the chosen
   provider rather than retain an independent merge or cleanup recipe. Check
   for remaining old fully qualified calls. Installing Git Skills alongside an
   unchanged Superpowers package does not perform this step.

6. **Check behavior in fresh sessions.** Verify a small solo task reuses its
   checkout, parallel writers receive distinct suitable checkouts, interrupted
   work preserves unique commits and local files, and retained resources have
   identifiable owners. Check both a secret staged beneath a clean working file
   and a clean staged file with secret material only in unstaged edits. Check
   that a secret-free private document outside the intended disclosure scope
   is caught before commit creation. Verify that a push reuses valid content
   reviews, while an old unreviewed commit or a wider destination audience
   triggers review of only the missing coverage. Include content added and
   later removed in the outgoing history. Test
   primary-branch integration with and without prior approval, a linear-history
   conflict, and a configured route that bypasses known blocked sandbox DNS.
   Inspect actual refs and files, not only the agent's account of its actions.
   Confirm the loaded catalog and caller paths after a supported package update.

7. **Publish the reviewed package changes through their own delivery policy.**
   Review secrets and disclosure scope before commit creation; verify review
   coverage and the actual destination before push. Obtain
   the decision for each primary-branch update; an installation or migration
   discussion alone does not authorize it. Run the package's release procedure
   only when that procedure or the user requests a release. Preserve existing
   useful branches and worktrees through the switch; installing a new provider
   does not make them abandoned resources.

For a repository using both development and Git plugins, the policy can name
the responsibilities directly. This block is also suitable for a Superpowers
consumer or worker prompt when Git Skills is not installed:

```markdown
Use Superpowers for planning, implementation, debugging, TDD, review, and
verification. Follow the repository's Git workflow for branch and checkout
allocation, commit content and disclosure review, publication verification,
and handoff or integration. Read AGENTS.md and AGENTS.local.md when present.

Before editing or dispatching writers, inspect Git state and ownership; reuse
a suitable branch and checkout. Create a branch when independent history or
review is required. Allocate another checkout when concurrent writers,
unrelated local changes, active processes, or repository policy make the
current checkout unsuitable. Update an existing assignment record through
its designated writer.
Before any commit-producing operation, review the actual candidate tree and
messages for secrets and compatibility with the intended disclosure scope,
including worker and generated commits. If that scope is unspecified, assess
content as potentially public. Preserve unrelated staging and local edits.
Cover retained ancestor history when its review is missing or the intended
audience is wider; a deletion does not make earlier private content public-safe.
Use configured checks or inspect candidate content manually, and bind the
review scope and results to actual commit SHAs in the existing handoff.
Before publishing refs, verify that all outgoing commits and the destination
audience are covered by those reviews. Reuse valid content checks. For older
unreviewed commits or a wider audience, apply the commit-content review to
only the missing coverage without recreating commits solely for evidence.
Content added and later removed remains part of the outgoing history.
At handoff or integration, verify branch or SHA, checkout, dirty state, checks,
allowed history method, and resource owner. Apply existing authorization for
primary-branch changes; obtain the integration decision only when absent.
Retain useful resources and check actual state before reuse or cleanup.

Use an available skill covering the needed Git responsibility, following the
repository's provider preference. If none is available, perform that workflow
with repository policy and available host or Git tools. Do not make plugin
installation a prerequisite. Follow the configured working remote route and
actual host permission controls.
```

This declaration expresses a repository preference. Verify that operational
callers and the loaded package catalog implement it; competing required calls
must be updated at their source.

## Resolve Available Git Skills

When Git Skills is available and selected, the responsibilities above map to
the following skills. These are conditional resolution targets, not required
dependencies in consumer prompts:

| Trigger and responsibility | Available Git Skills target |
| --- | --- |
| Before editing or assigning writers: decide whether to reuse or create a branch or checkout and establish ownership. | `git-skills:using-git-worktrees` |
| Before creating or recreating commits: review actual candidate content, messages, secrets, and intended disclosure scope; fill missing coverage for existing commits when needed. | `git-skills:reviewing-before-commit` |
| Before publishing refs: verify the outgoing commits and actual destination are covered by those content reviews. | `git-skills:reviewing-before-push` |
| At handoff or integration: verify actual state, primary-branch authorization, history method, and resource lifecycle. | `git-skills:finishing-a-development-branch` |

| Session state | Caller behavior |
| --- | --- |
| Git Skills is available and selected by repository policy. | Resolve the required responsibility to its qualified Git Skills target. |
| Git Skills is absent, including when a local preference names it. | Perform the portable contract with repository policy and available tools; do not require installation. |
| Multiple Git providers are loaded. | Follow the configured provider and qualify calls; update older mandatory callers that enforce a conflicting policy. |

Internal references between skills in the Git Skills package can keep qualified
names: invoking that package already establishes their availability. The
portable contract belongs at the consumer boundary.

## Review the Policy Changes

This procedure does not copy settings automatically. The following choices
deserve explicit review when filling the local policy:

| Choice | What changes or needs attention |
| --- | --- |
| Primary-branch authorization | The gate covers local primary-ref changes as well as remote pushes. It honors prior authorization; task-branch commits and resource allocation do not acquire another approval gate. |
| Linear history and source updates | Linear history excludes merge commits introduced from the source branch too. Fast-forward-only does not authorize rebasing or squashing the source; set the task-branch update strategy separately. |
| Commit checks and publication verification | Review secrets and allowed disclosure scope before commit creation, including generated commits, messages, and private documents or data. Unspecified scope is assessed as potentially public; explicit local-only scope still prohibits secrets. Push verifies coverage for the full outgoing history and actual destination, reusing valid reviews. No scanner or hook is installed or enforced by these instructions. |
| Execution route and permissions | A working outside-sandbox clone/fetch route can bypass known blocked DNS. Markdown cannot grant host approval; migrate actual approved scopes, not assumed permission. |
| Local instructions and records | Ensure the repository instructions explicitly load AGENTS.local.md when the plugin is absent. Keep local machine settings and private ownership metadata out of tracked files unless repository policy requires sharing them. |
| Resource ownership | Read-only agents may share a checkout; concurrent writers use separate suitable checkouts. Reuse and retention are valid outcomes. Records need a designated writer and actual state; there is no background orphan monitor. |
| Development artifacts | Dependency setup, tests, review evidence, release decisions, and plan-ledger cleanup stay with their development owner. A plan-ledger directory is not a Git worktree. |
| Installed copies | Source changes do not update an installed cache. Verify the loaded package after its supported update; other Superpowers copies may still require the old Git workflows. |
