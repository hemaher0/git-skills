# git-skills

A Codex plugin marketplace for Git workflows. It provides skills for branch and
workspace management, commit review, push verification, and integration.

| Skill | Role |
| --- | --- |
| [using-git-worktrees](plugins/git-skills/skills/using-git-worktrees/SKILL.md) | Decide separately whether a task needs a branch or another checkout; assign concurrent writers without creating redundant resources. |
| [finishing-a-development-branch](plugins/git-skills/skills/finishing-a-development-branch/SKILL.md) | Verify handoffs and branch work, confirm authorization before changing the primary branch, and retain or clean up resources deliberately. |
| [reviewing-before-commit](plugins/git-skills/skills/reviewing-before-commit/SKILL.md) | Review actual candidate content and messages for secrets and intended disclosure scope before creating or recreating a commit. |

For example, a solo edit in a suitable checkout uses that checkout. A review
policy may require a branch without another worktree. Concurrent writers in
one repository use separate suitable checkouts; the coordinator verifies each
handoff before changing a shared target. A pull request may keep its branch
and checkout for follow-up rather than trigger immediate cleanup.

## Install for a project

Configure the project plugin as described below. The skills follow existing
project Git instructions; the optional policy template helps record rules
when the project needs them.
Complete applicable project configuration using the steps below, preserving
existing instructions and established choices.

Keep the marketplace and enablement in the target project using the files
below. Write these project files directly: `codex plugin marketplace add` and
`codex plugin add` save user-level configuration in `~/.codex/config.toml`;
running them from a project directory does not make them project-scoped.
The plugin browser also saves user-level enablement choices.
Neither route is a step in this project-only procedure.

Reuse a suitable source checkout when present. For a first setup, run these
commands from the **target project's root**, with Git access to this repository:

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

### Repository Git policy

Writing root `AGENTS.local.md` is a required installation step.

1. Read existing project instructions and Git policy, the
   [project policy template](plugins/git-skills/templates/AGENTS.md) and the
   [local configuration template](plugins/git-skills/templates/AGENTS.local.md).
   Keep portable policy in its existing home, using the project template for
   applicable project choices.
2. Create the local file from its template, or merge the Git section into the
   existing file. Replace applicable placeholders with actual host transport
   or worktree path values. Preserve established settings and other packages'
   sections. Remove fields that do not apply.
3. Where settings are maintained elsewhere, reference their actual source and
   verify its contents. Keep reusable procedures in
   [using-git-worktrees](plugins/git-skills/skills/using-git-worktrees/SKILL.md),
   [reviewing-before-commit](plugins/git-skills/skills/reviewing-before-commit/SKILL.md)
   and [finishing-a-development-branch](plugins/git-skills/skills/finishing-a-development-branch/SKILL.md).
   Inspect branch, remote, checkout and actual permissions from Git and the
   host; keep ownership and review coverage in existing work records.
4. Connect the local file to root instructions using the procedure below.

If root `AGENTS.md` exists, preserve it and add this instruction unless it
already reads or resolves to the local file:

```markdown
Read and follow root AGENTS.local.md when it exists.
```

If `AGENTS.md` is absent, the recommended connection is a relative symbolic
link from the project root, after writing `AGENTS.local.md`:

```bash
ln -s AGENTS.local.md AGENTS.md
```

Preserve existing files and links and avoid self-references. If
`AGENTS.override.md` takes precedence, ensure it reads the local file.

Before completing installation, read the completed file and any referenced
configuration. Verify that applicable values are filled, no placeholders
remain, configured paths resolve, and effective instructions read the local
file. A generic "use defaults" statement does not replace filled settings.
Check actual marketplace paths/name, skill availability and policy resolution.

Ordinary Git work follows the existing project policy and actual host controls.
Resolve essential facts, rules and authorization for the intended operation;
unrelated unused template fields do not block that operation. Configuration
files do not grant permissions.

After completing the repository Git policy above, open the project as trusted
and start a **new Codex session**; restart the desktop app when needed. Codex
uses the project configuration during local marketplace discovery and refresh.
Verify that the plugin's skills are available in that project session.
Project configuration is loaded only for trusted projects.

Codex may keep plugin files in its shared `~/.codex/plugins/cache/`; that cache
location does not determine enablement scope. Existing user-level enablement
remains a separate setting; adding project settings does not remove it.
See the [official project plugin configuration guide](https://developers.openai.com/plugins/build/plugins#enable-or-disable-a-plugin-for-a-repo).

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

## Requirements

Git is required for repository operations. A host-managed workspace tool can
be used when available; it is not required. A requested pull request needs an
available repository-host integration. No other skill plugin is required.
Follow applicable project instructions and any explicit policy setup requirements.

## Boundaries

- Skill selection alone does not create a worktree, branch, commit, merge, pull
  request, or push. Reuse a suitable workspace and task branch first.
- Read the target repository's branch, workspace, review, and cleanup policy,
  including `AGENTS.local.md` when present or required. Verify rules material
  to the actual operation rather than requiring every optional template field.
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
- Use the project's existing checks before integration.

These skills were adapted from the MIT-licensed
[Superpowers project](https://github.com/obra/superpowers). The original
copyright and permission notice is in [LICENSE](plugins/git-skills/LICENSE).
