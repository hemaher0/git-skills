# Project Git Policy

<!-- Merge only actual project exceptions/choices into effective instructions,
or reference an existing Git policy. Omit fields already governed by repository
configuration, CI, host policy, or skill defaults. Preserve established values;
this template is not a complete-policy prerequisite for every Git operation. -->

## Git Policy

- Existing Git policy source, if maintained elsewhere: `<project reference>`
- Branch/workspace requirements and naming exceptions: `<established project rule>`
- Concurrent-writer exception, if permitted: `<project rule and index owner>`
- Assignment/ownership record location, if designated: `<existing record or host attachment>`
- Direct commits to the primary branch: `<allowed conditions or prohibited>`
- Commit message convention, if additional rules apply: `<project rule>`
- Secret handling and disclosure policy: `<allowed/prohibited content, audiences and destination rules, or existing policy reference>`
- Decision owner for uncertain content classification: `<project role or existing procedure>`
- Required checks: `<existing index-aware checks/CI configuration or project procedure>`
- Delivery path and publication conditions: `<PR/local integration/direct update and established authorization rules>`
- History shape and allowed integration/task-update methods: `<per-delivery-path project rules>`
- Published-history and resource-retention exceptions: `<existing project rules>`

When the current host lists matching workspace, commit-review, or branch-
finishing skills, use them for the decisions they own. Do not infer skill
availability from files in a vendor tree or cache. If a companion skill is
unavailable, follow this project's established Git procedure and ordinary Git
tools: inspect actual workspace and ownership state; review each candidate tree
and message for secrets and disclosure scope; verify outgoing commits and
their destination; obtain required authorization before changing the primary
branch; and retain or remove resources only after checking their real state.
Existing authorization and host controls remain authoritative. Record actual
resource ownership and reviewed commit coverage in the existing work record.
Read and follow root `AGENTS.local.md` when it exists. Local configuration
cannot broaden disclosure, integration, or cleanup permissions.
