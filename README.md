# git-skills

Reusable Codex skills for Git workspace selection, commit review, branch
handoff, publication, integration, and resource retention.

| Skill | Role |
| --- | --- |
| [using-git-worktrees](plugins/git-skills/skills/using-git-worktrees/SKILL.md) | Decide separately whether a task needs a branch or another checkout; assign concurrent writers without creating redundant resources. |
| [reviewing-before-commit](plugins/git-skills/skills/reviewing-before-commit/SKILL.md) | Review actual candidate content and messages for secrets and intended disclosure scope before creating or recreating a commit. |
| [finishing-a-development-branch](plugins/git-skills/skills/finishing-a-development-branch/SKILL.md) | Verify handoffs and branch work, confirm authorization before changing the primary branch, and retain or clean up resources deliberately. |

The native skill source is `plugins/git-skills/skills/`. Keep the whole source
checkout so each selected skill can resolve its packaged files and this
repository can be updated in place. These locations and symlink behavior follow
the [official Codex skill discovery guidance](https://learn.chatgpt.com/docs/build-skills).

## Install for a project

Run the following from the target project's root. Reuse an existing clean
source checkout instead of cloning over it.

```bash
mkdir -p .agents/vendor .agents/skills
git clone --branch main https://github.com/hemaher0/git-skills.git .agents/vendor/git-skills
```

Choose one of the following alternatives. To expose selected skills, create
only the links the project needs; for example:

```bash
for skill in using-git-worktrees reviewing-before-commit; do
  destination=".agents/skills/$skill"
  if [ -e "$destination" ] || [ -L "$destination" ]; then
    printf 'Preserve existing path: %s\n' "$destination" >&2
    continue
  fi
  ln -s "../vendor/git-skills/plugins/git-skills/skills/$skill" "$destination"
done
```

To expose all three skills instead:

```bash
for source in .agents/vendor/git-skills/plugins/git-skills/skills/*; do
  [ -f "$source/SKILL.md" ] || continue
  skill="${source##*/}"
  destination=".agents/skills/$skill"
  if [ -e "$destination" ] || [ -L "$destination" ]; then
    printf 'Preserve existing path: %s\n' "$destination" >&2
    continue
  fi
  ln -s "../vendor/git-skills/plugins/git-skills/skills/$skill" "$destination"
done
```

These are relative links from `.agents/skills` to the project-local source.
The guard preserves an existing file, directory, valid symlink, or dangling
symlink and skips that name. Inspect conflicts deliberately; do not add `-f`
or overwrite them.

Verify every link you selected from the project root:

```bash
test -L .agents/skills/using-git-worktrees
test -d .agents/skills/using-git-worktrees
readlink .agents/skills/using-git-worktrees
```

Repeat those checks with each selected skill name. Ask Codex to use the skill
by its frontmatter name, such as `$using-git-worktrees`. Confirm that the name
appears in the skill catalog for a session opened in this project. Codex
normally detects skill changes automatically; if the catalog remains stale,
start a new session or restart the app.

### Project configuration when needed

Native skill discovery does not require a plugin marketplace file or Codex
plugin configuration. Existing repository instructions remain authoritative.
If the project needs to record Git choices that its existing instructions,
Git configuration, CI, or host policy do not already supply, merge only the
applicable fields from
[the project policy template](plugins/git-skills/templates/AGENTS.md) and
[the local configuration template](plugins/git-skills/templates/AGENTS.local.md)
into its established instruction files. Preserve existing settings, omit
inapplicable fields, and make sure the effective root instructions read any
local file they rely on. The templates are configuration aids, not additional
skills or prerequisites for ordinary Git work.

### Update or remove from a project

Update the project-local source checkout from the project root:

```bash
git -C .agents/vendor/git-skills pull --ff-only
```

The relative links continue to use the updated skill folders. Verify the
selected links and catalog again after an update.

To remove a selected skill, unlink only its project-local link, for example:

```bash
unlink .agents/skills/using-git-worktrees
```

Remove the other selected links the same way if desired. Before removing the
source checkout, confirm that no remaining project link or other local setup
uses it and preserve any local source changes:

```bash
git -C .agents/vendor/git-skills status --short
```

If that output is empty and the checkout has no remaining consumers, remove
that exact source directory:

```bash
rm -rf -- .agents/vendor/git-skills
```

Do not remove `.agents/skills`, `.agents/vendor`, or unrelated contents owned
by the project.

## Install globally

A global installation makes the selected names available to Codex sessions
for this user. Clone a separate source checkout under `$HOME/.agents`; do not
reuse a project-local source checkout.

```bash
mkdir -p "$HOME/.agents/vendor" "$HOME/.agents/skills"
git clone --branch main https://github.com/hemaher0/git-skills.git "$HOME/.agents/vendor/git-skills"
```

Choose one of the following alternatives. To expose selected skills, create
only the links wanted globally; for example:

```bash
for skill in using-git-worktrees reviewing-before-commit; do
  destination="$HOME/.agents/skills/$skill"
  if [ -e "$destination" ] || [ -L "$destination" ]; then
    printf 'Preserve existing path: %s\n' "$destination" >&2
    continue
  fi
  ln -s "../vendor/git-skills/plugins/git-skills/skills/$skill" "$destination"
done
```

To expose all three skills instead:

```bash
for source in "$HOME"/.agents/vendor/git-skills/plugins/git-skills/skills/*; do
  [ -f "$source/SKILL.md" ] || continue
  skill="${source##*/}"
  destination="$HOME/.agents/skills/$skill"
  if [ -e "$destination" ] || [ -L "$destination" ]; then
    printf 'Preserve existing path: %s\n' "$destination" >&2
    continue
  fi
  ln -s "../vendor/git-skills/plugins/git-skills/skills/$skill" "$destination"
done
```

The targets are relative to `$HOME/.agents/skills`. The guard preserves and
skips any existing file, directory, valid symlink, or dangling symlink. Inspect
a conflict instead of adding `-f`.
Verify every selected link, for example:

```bash
test -L "$HOME/.agents/skills/reviewing-before-commit"
test -d "$HOME/.agents/skills/reviewing-before-commit"
readlink "$HOME/.agents/skills/reviewing-before-commit"
```

Ask Codex to use the skill by its frontmatter name, such as
`$using-git-worktrees`, and confirm that name in the current host's skill
catalog. Codex normally detects changes automatically;
start a new session or restart the app if the catalog remains stale.
This global installation does not create or replace repository instructions;
each project's own Git policy and actual host controls still govern its work.

Update the global source checkout with:

```bash
git -C "$HOME/.agents/vendor/git-skills" pull --ff-only
```

To remove a globally selected skill, unlink only its global link, for example:

```bash
unlink "$HOME/.agents/skills/reviewing-before-commit"
```

After removing every global link that uses this source and preserving any
local changes, inspect the source checkout:

```bash
git -C "$HOME/.agents/vendor/git-skills" status --short
```

If that output is empty and the checkout has no remaining consumers, remove
that exact source directory:

```bash
rm -rf -- "$HOME/.agents/vendor/git-skills"
```

Do not remove `$HOME/.agents/skills`, `$HOME/.agents/vendor`, or unrelated
packages' contents.

Project and global discovery are independent. A project skill with the same
frontmatter name does not mask or merge with a global one; both may appear.
Keep only the intended copy in scope or inspect the displayed source before
use when duplicate names exist. Global source updates affect every session
using its links, while each project-local source can remain on its own version
and expose a different subset.

## Optional plugin compatibility

This repository also retains a native plugin package at `plugins/git-skills/`,
with its manifest at `plugins/git-skills/.codex-plugin/plugin.json`. A host
that supports managed Codex plugins can install or enable that package through
its plugin workflow as an alternative distribution route. Plugin enablement
and its shared cache are separate from the project and global `.agents/skills`
links above; enabling a plugin may expose another copy of the same skill names.
Follow the host's plugin scope and update controls rather than treating a cache
path as proof that a skill is available.

## Requirements and boundaries

Git is required for repository operations. A host-managed workspace tool and
a repository-host integration may be used when available, but no companion
skill package is required.

- Installing or selecting a skill does not create a worktree, branch, commit,
  merge, pull request, or push. It does not grant host permissions or authorize
  publication or a primary-branch change.
- Read the target repository's branch, workspace, review, disclosure, delivery,
  and cleanup policy. Resolve actual branches, remotes, worktrees, ownership,
  permissions, and authorization from repository and host state.
- Before creating or recreating any commit, inspect the actual candidate tree
  and message for secrets and compatibility with the intended disclosure
  scope. Before a push, verify every outgoing commit and destination has
  matching review coverage; a final-tree diff can omit earlier content.
- Preserve unrelated staging and local edits. Retain useful branches and
  worktrees for review or reuse, and inspect actual state before cleanup.
- Use the project's existing checks before integration. Obtain an integration
  decision before changing its primary branch unless existing authorization
  already covers that exact operation.

These skills were adapted from the MIT-licensed
[Superpowers project](https://github.com/obra/superpowers). The original
copyright and permission notice is in
[LICENSE](plugins/git-skills/LICENSE).
