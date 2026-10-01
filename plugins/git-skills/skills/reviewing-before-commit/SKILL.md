---
name: reviewing-before-commit
description: Use before creating or recreating Git commits to review candidate content, messages, secrets, and intended disclosure scope; also when existing commits lack a valid content review
---

# Reviewing Before Commit

Decide whether content may enter repository history before creating the
commit. Check both secrets and whether the content may be disclosed within
the commit's intended scope. This applies to task branches and private
repositories as well as the primary branch. Routine branch commits follow
the user's existing authorization; this review does not introduce another
permission question or authorize a push.

## Establish the Candidate

Read the repository's `AGENTS.md`, optional `AGENTS.local.md`, secret-handling
and disclosure policy, and required commit checks. Establish the intended
disclosure scope from existing instructions: public, named private audiences
and destinations, or explicitly local-only. A missing remote does not itself
establish a local-only policy. If the scope is unspecified, assess the content
as potentially public rather than inventing a private audience; resolve a
material classification question only when needed.

Identify the branch, intended parents, actual staged tree, and intended commit
message. A direct primary-branch commit also needs the integration decision in
`git-skills:finishing-a-development-branch`.

Account for the history retained through the intended parents. If adopting a
wider disclosure scope or inheriting unreviewed history, verify that ancestry
under the same scope before creating the candidate commit. Reuse a known
covered publication base and prior reviews; use the existing-commit mode below
only for missing coverage. Removing private content from the candidate tree
does not make its earlier commits suitable for a wider audience.

Inspect `git diff --cached`, staged paths, and the complete contents of
relevant staged blobs. Read from the index, for example with `git show :path`,
or use an index-aware scanner. The working file can differ from the staged
file in either direction: a safe working copy can hide a staged secret, and
an unstaged secret does not mean a clean staged version contains it. An
initial commit has no HEAD; review the index without assuming HEAD exists.

Run configured formatting or staging steps before this review. Account for
the tree the commit command will actually create: `git commit -a` and path
arguments can change the candidate beyond the currently staged tree. Prefer
committing the reviewed index with an explicit message. Preserve unrelated
staging and unstaged changes.

## Review Secrets and Disclosure Before Creating the Commit

Check the candidate tree and message for repository-prohibited credentials,
tokens, private keys, passwords, and other secret material. Use the required
scanner when configured, and inspect meaningful context. A scanner that reads
only working files does not verify the index; use a snapshot of the candidate
tree if that scanner needs files on disk. Inspect referenced Git LFS content
when a staged pointer would introduce it. Scanner success supports the review
but does not prove that every value is appropriate to record.

Check the candidate content and messages against the intended disclosure
scope: internal documents, personal data, non-public research or customer
material, generated artifacts, and other repository-restricted content. A
secret-free candidate can still contain material outside that scope. A private
remote or task branch does not automatically authorize such content. An
explicit local-only policy may allow private documents in local history; it
does not exempt secrets from the repository's commit prohibition.

Do not reproduce suspected secret values in reports, shared logs, or commit
messages. Report paths, categories, and the decision needed. Do not install a
scanner or a hook merely because this skill was selected.

If the candidate contains a prohibited secret or content outside its allowed
disclosure scope, do not create that commit. Exclude or sanitize the affected
staged content without discarding unrelated work, then review the revised
candidate. For example, restaging a sanitized
working file may fix its staged blob; editing only the working file does not.
If classification is unclear, obtain the configured secret or disclosure
decision. If the value is already committed, identify the affected history
without printing it and follow the authorized remediation procedure. A later deletion
does not remove it from an earlier commit.

## Cover Generated Commits and Commit the Reviewed State

Amend, merge, cherry-pick, rebase, and squash can also create commits. Inspect
the candidate content and messages, including retained messages during an
amend. Use a reviewable candidate or the project's per-commit checking route
for an operation that generates commits automatically. Review new conflict
resolutions; checking only the final tree does not cover unsafe intermediate
commits. Content removed by a later generated commit still needs review before
the earlier commit is created. Prior checks may be reused for exactly unchanged
content and messages within the same or a covered disclosure scope.

Keep required hooks enabled. If staging, formatting, conflict resolution, or
message editing changes the reviewed candidate, repeat the affected review
before committing. Record the reviewed candidate tree and message identity,
disclosure scope, and checks in the existing task record or handoff; do not
create another registry solely for this review. Create only the reviewed
commit, then verify its actual tree and message against that candidate and
report its SHA and remaining local state. Bind the review result to that
actual commit so publication can reuse it.

## Review Existing Commits Only When Coverage Is Missing

An imported or older commit may lack a review, or a new destination may expose
it to an audience outside its reviewed scope. Apply the same content and
message checks to those existing commits without recreating them merely to
record a review. Inspect their actual trees, changes, messages, and referenced
LFS content; a clean final-tree diff does not cover material added and removed
in earlier commits. Reuse valid reviews for unchanged commits and covered
audiences. Record the checked SHAs and scope in the existing handoff or record.

If prohibited material is already committed, preserve unrelated work and use
the authorized remediation procedure. Keep it unpublished until the history
meets policy; deleting it only from the latest tree is insufficient.
