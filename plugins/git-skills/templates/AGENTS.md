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

Use the responsible workspace, commit-review, and finishing workflows for
actual Git decisions and checks. Existing authorization and host controls
remain authoritative. Record actual resource ownership and reviewed commit
coverage in the existing work record; inspect current Git/host state as needed.
Read root `AGENTS.local.md` when a selected host/path override exists. Local
configuration cannot broaden disclosure, integration, or cleanup permissions.
