# git-skills

A Codex plugin marketplace for Git workflows. It provides skills for branch and
workspace management, commit review, push verification, and integration.

| Skill | Role |
| --- | --- |
| [using-git-worktrees](plugins/git-skills/skills/using-git-worktrees/SKILL.md) | Decide separately whether a task needs a branch or another checkout; assign concurrent writers without creating redundant resources. |
| [finishing-a-development-branch](plugins/git-skills/skills/finishing-a-development-branch/SKILL.md) | Verify handoffs and branch work, obtain a decision before changing the primary branch, and retain or clean up resources deliberately. |
| [reviewing-before-commit](plugins/git-skills/skills/reviewing-before-commit/SKILL.md) | Review actual candidate content and messages for secrets and intended disclosure scope before creating or recreating a commit. |
| [reviewing-before-push](plugins/git-skills/skills/reviewing-before-push/SKILL.md) | Verify that outgoing commits have valid content reviews covering the actual destination and refs. |

For example, a solo edit in a suitable checkout uses that checkout. A review
policy may require a branch without another worktree. Concurrent writers in
one repository use separate suitable checkouts; the coordinator verifies each
handoff before changing a shared target. A pull request may keep its branch
and checkout for follow-up rather than trigger immediate cleanup.

## Install for a project

Use a repository marketplace and project configuration. Run these commands from
the **target project's root**, with Git access to this repository:

```bash
mkdir -p .agents/vendor .agents/plugins .codex
git clone --branch main https://github.com/hemaher0/git-skills.git .agents/vendor/git-skills
```

Create or merge the following into the target project's
`.agents/plugins/marketplace.json`:

```json
{
  "name": "project-skills",
  "plugins": [
    {
      "name": "git-skills",
      "source": {
        "source": "local",
        "path": "./.agents/vendor/git-skills/plugins/git-skills"
      },
      "policy": {
        "installation": "AVAILABLE",
        "authentication": "ON_INSTALL"
      },
      "category": "Productivity"
    }
  ]
}
```

Keep existing marketplace entries. When using multiple skill repositories, add
their entries to the same `plugins` array. Paths resolve from the target
project's root. If its marketplace already has a different `name`, keep that
name and use it in the configuration keys below.

Merge this setting into the target project's `.codex/config.toml`:

```toml
[plugins."git-skills@project-skills"]
enabled = true
```

Open the target project as a trusted project in Codex, restart the app if using
the desktop client, and start a **new Codex session**. Project configuration is
loaded only for trusted projects. The marketplace source and enablement belong
to this project; Codex may still store installed copies in its shared cache.
See the [official repository marketplace and project configuration guide](https://developers.openai.com/plugins/build/plugins).

## Update or remove from a project

From the target project's root, update its source checkout:

```bash
git -C .agents/vendor/git-skills pull --ff-only
```

Restart the app if using the desktop client and start a new Codex session so
the local plugin is refreshed.

To disable it for this project, set
`plugins."git-skills@project-skills".enabled = false` in
`.codex/config.toml`, using the project's actual marketplace name. To remove
the project setup, remove that configuration entry and only the `git-skills`
entry from `.agents/plugins/marketplace.json`. Keep other plugins' entries.
The source checkout can be removed separately once it is no longer needed.

## Configure repository Git policy

The template is available in the project checkout at
`.agents/vendor/git-skills/plugins/git-skills/templates/AGENTS.local.md`.
Use the [AGENTS.local.md template](plugins/git-skills/templates/AGENTS.local.md)
to fill in the `Git Policy` section in the target repository's root
`AGENTS.local.md`. If that file already exists, add the section without
replacing its other settings. Fill or remove each applicable placeholder.
The template records local choices about branch and worktree creation,
parallel writers, sandbox DNS, clone/fetch transport, permission escalation,
commit disclosure scope, primary-branch history shape, allowed merge methods,
integration approval, and checks for secrets and disclosure before commit
creation. Push checks verify coverage for the actual outgoing refs and audience.
A placeholder supplies no rule; a filled
field cannot grant a host permission that has not actually been approved.

Reference the local file from the repository's `AGENTS.md` if agents without
this plugin also need to read it. Keep rules shared by the whole team in the
repository's tracked instructions; use `AGENTS.local.md` for local choices.
The Git skills read the local policy when present, and continue using
repository instructions when it is absent.

When Superpowers also supplies Git workflows, follow the
[migration procedure](docs/migrating-from-superpowers.md) to assign one Git
provider and update its callers. Use its portable, responsibility-based
contract in development skills and worker prompts: an available Git skill
covers the relevant responsibility, and repository policy plus host or Git
tools covers it when the plugin is absent. The guide includes conditional
skill mappings and policy changes to review. Installing another provider
alone does not change existing fully qualified Superpowers calls.

## Requirements

Git is required for repository operations. A host-managed workspace tool can
be used when available; it is not required. A requested pull request needs an
available repository-host integration. No other skill plugin is required.

## Boundaries

- Skill selection alone does not create a worktree, branch, commit, merge, pull
  request, or push. Reuse a suitable workspace and task branch first.
- Read the target repository's branch, workspace, review, and cleanup policy,
  including a local `AGENTS.local.md` when present.
  Do not assume a default branch name, worktree location, or ownership from
  another repository.
- Routine clone/fetch operations and commits on an assigned branch follow the
  task and repository policy. A known blocked DNS route is not repeatedly
  probed; use the configured working transport. A change to the primary branch
  needs an explicit user integration decision, whether by local Git or a pull
  request merge. Previously granted authorization does not need repeating.
- Before creating or recreating any commit, check its actual candidate content
  and message for secrets and compatibility with its intended disclosure scope.
  Preserve unrelated staging and local edits. Private remotes do not exempt
  commits from these checks. If the scope is unspecified, assess content as
  potentially public. These instructions do not install or enforce a Git hook.
- Before any push, verify that every outgoing commit and the actual destination
  are covered by those reviews. Reuse valid content checks. Older unreviewed
  commits or a destination outside their reviewed scope need the commit-review
  workflow to fill that gap before publication. A final-tree diff can hide
  material added and removed in earlier commits. This verification applies to
  task branches too and is separate from primary-branch integration approval.
- Retain a useful branch or worktree for review or reuse. Do not force cleanup
  at the end of every task. Track who owns newly created resources and inspect
  real Git state before reusing or deleting one after an interrupted task.
- These skills run when invoked. They do not provide a background monitor for
  chats that end unexpectedly; a host lifecycle integration is needed for
  continuous orphan detection.
- The Git workflows do not require `dev-skills` or `research-skills`. Their
  callers may use these skills when installed, but can follow the host and
  repository's Git procedure without installing this plugin.
- Use the project's existing checks before integration. This plugin does not
  define development tests, research evidence, release versions, or schema
  migration policy.
- When a project configures topic work items, forward actual code branch names,
  SHAs, merge or abandonment outcomes, and checks to an available compatible
  document workflow. If none is installed, use the project's local
  `.docs-schema` and normal file tools.
  Document records stay on the document repository's existing main branch;
  only code work may need a new branch.

These skills were adapted from the MIT-licensed
[Superpowers project](https://github.com/obra/superpowers). The original
copyright and permission notice is in [LICENSE](plugins/git-skills/LICENSE).
