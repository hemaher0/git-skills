# Local Agent Instructions

<!--
Copy the Git Policy section into the target repository's root AGENTS.local.md.
If that file already exists, preserve its other sections. Replace or remove
every placeholder before relying on that field. An unfilled field supplies no
rule; use the user's instructions and the repository's tracked policy instead.
This file records local preferences. It cannot grant sandbox, host, remote, or
GitHub permissions that have not actually been approved.
Do not put credentials or tokens here.
Ensure the repository's AGENTS.md instructs agents to read AGENTS.local.md
when present; this template does not install a loader.
-->

## Git Policy

### Repository

- Purpose: `<code, canonical document repository, or other>`
- Primary branch: `<actual branch name>`
- Publication remote: `<remote name or none>`
- Git workflow provider preference: `<available provider chosen by this repository, or no preference; this is not an installation requirement>`

### Workflow responsibilities

Apply the repository's Git workflow before branch or checkout allocation,
before creating or recreating commits, before remote publication, and at
handoff or integration. Use an available skill covering the needed
responsibility according to the provider preference, and qualify its name
when multiple packages overlap. If that provider is absent, perform the same
workflow with repository policy and available host or Git tools; do not
require installation or skip its checks.

Inspect state and ownership before assigning writers; reuse suitable
resources. Review actual candidate content and messages for secrets and
allowed disclosure scope before commit creation. Before publication, verify
that the actual outgoing commits and destination are covered by those reviews;
apply the commit-review workflow only to missing or changed scope. Verify
handoff state, integration authorization and method, and resource ownership
before integration, reuse, or cleanup.
Existing user authorization and actual host permission controls still apply.

### Workspaces and branches

- Starting workspace: `<current checkout, existing worktree, or project rule>`
- New branch creation: `<when separate history is required, or prohibited>`
- Branch base and naming, if allowed: `<rule or not applicable>`
- New worktree creation: `<when another checkout is needed, or prohibited>`
- Reuse of free branches and worktrees: `<rule>`
- Resource assignment records: `<existing record or host attachment; designated writer; update owner, actual branch/SHA, and local state on allocation and handoff>`
- Worktree ownership and retention: `<how ownership is recorded and when to keep, archive, or remove>`

### Parallel agents

- Concurrent writers: `<whether each needs a separate checkout; define any exception>`
- Workspace allocator and integration owner: `<coordinator, named role, or project rule>`
- Worker handoff: `<branch or commit SHA, workspace path, dirty state, checks, and other required evidence>`

### Remote Git and execution

- Sandbox DNS for Git remotes: `<available, blocked, or unknown>`
- Clone and fetch transport: `<sandbox Git, approved outside-sandbox Git, host integration, or other exact route>`
- Git remote authentication: `<SSH, credential helper, host integration, or other route; never a secret>`
- Previously approved command scopes: `<exact approved Git command prefixes or none; this line does not create approval>`
- Permission escalation requests: `<none, only named operations, or exact project rule>`
- After a known DNS or permission failure: `<alternate route or stop rule; do not retry the same blocked route>`

### Commits and delivery

- Direct commits to the primary branch: `<allowed conditions or prohibited>`
- Commit message convention: `<rule or no additional convention>`
- Secret material prohibited in commits: `<categories and paths, including commit-message rules>`
- Required pre-commit secret checks: `<index-aware scanner commands and review scope, or manual staged-content review>`
- Uncertain secret classification: `<person or project rule that resolves whether a value may enter a commit>`
- Commit disclosure scope: `<public, named private audiences and intended destinations, or explicitly local-only; if unspecified assess content as potentially public>`
- Content permitted in commits: `<data classes and paths allowed or prohibited for each disclosure scope; local-only does not exempt secrets>`
- Required pre-commit disclosure checks: `<candidate-content, message, and referenced-data review for the intended scope, including generated commits>`
- Uncertain commit disclosure decision: `<person or project rule that resolves whether content may enter a commit within that scope>`
- Commit review evidence: `<existing task record, handoff, or check results binding reviewed content/messages and disclosure scope to actual commit SHAs; do not create another registry>`
- Push destination and conditions: `<rule>`
- Push remote visibility and audience: `<public, private with named audience, or unknown; specify each destination>`
- Required pre-push verification: `<exact outgoing refs/SHAs and validity of existing commit reviews for the actual destination; use commit review only for missing coverage or changed scope>`
- Review and delivery path: `<PR, local integration, direct update, or other project rule>`
- Primary-branch history shape: `<linear history required, or merge commits allowed>`
- Allowed integration methods: `<fast-forward, rebase then fast-forward, squash, merge commit, cherry-pick, or exact allowed set; specify per delivery path if needed>`
- Updating a task branch before integration: `<rebase onto target, merge target into branch, or other rule>`
- Primary-branch integration decision: `<where explicit user authorization for a merge, fast-forward, cherry-pick, direct commit or push, or PR merge is recorded; how prior authorization is recognized>`
- Required checks before commit, push, or integration: `<commands or project rule>`

### History and cleanup

- Published history rewrite: `<rule>`
- Branch and worktree cleanup: `<when cleanup is useful; protect work that is still in use or has unique changes>`
