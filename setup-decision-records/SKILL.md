---
name: setup-decision-records
description: Set up a lasting decision-recording practice in workspace agent instructions. Use when a user wants their agents to remember consequential choices and consult past reasoning across sessions, for research, business, creative or technical work.
---

# Set up decision records

Install an ongoing practice in the workspace's agent instructions. Future sessions must be
able to follow it without this skill, another installed skill, or special tooling.

## Inspect before editing

Establish the intended workspace from the user's request and current location. Ask if the
boundary is ambiguous. Inspect its root `AGENTS.md`, `CLAUDE.md`, linked instruction files,
and any decision rules, records or index they identify. Follow existing workspace guidance.
Look for an established collection if instructions identify none; do not assume software-style
ADRs are the only decision records. Read relevant examples and any index maintenance rules.

Resolve the instruction files' physical targets and identities before editing. Symlinks and
hard links to one file count as one physical file. Preserve links and edit the target once;
do not use a replacement operation that breaks a hard link. Ask before editing a target
outside the intended workspace, or repairing a dangling link.

Adopt the existing collection, naming convention, record format, index or generator, and
canonical instruction file. Ask about incompatible rules or competing practices you cannot
resolve from the workspace. Do not create a parallel collection or index. Do not migrate
history or fabricate accepted decisions during setup. If an established collection lacks an
index, index existing records using their actual content and status, leaving records intact.

For a new practice, use `decisions/`, `decisions/README.md`, and the filename convention in
the template. Create an empty index with columns for record link/title, status and one-sentence
scope. Use relative links that resolve from the index. No scripts, hooks, database or scheduled
ceremony are required.

## Install one section

Choose the canonical file this way, preserving an established choice first:

- If only one instruction file exists, use it.
- If both exist separately and neither is established as canonical, use `AGENTS.md`.
- If they are aliases, use their shared target without changing either link.
- If neither exists, create `AGENTS.md`. Do not create a second instruction file for support.

Read [the installed-section template](assets/decision-records-section.md). Adapt its locations,
filename and record conventions, index maintenance, and supersession convention to what you
found. Replace every placeholder with concrete workspace-relative paths or instructions.
The installed text must include all ongoing behavior; never point it back to this skill.

Use the template's markers to add or update one section. If either file already contains
these markers or equivalent decision rules, reconcile them into the canonical section.
Preserve unrelated instructions. Remove or replace only redundant decision rules that this
setup consolidates; ask if changing an existing rule's meaning is necessary. Do not leave
contradictory legacy rules active.

If `AGENTS.md` and `CLAUDE.md` are separate physical files, add or update only this reference
in the other file, substituting the canonical filename and a working relative link:

```markdown
<!-- decision-records:reference:start -->
Read and follow the [Decision records section](AGENTS.md#decision-records) before substantive
work and whenever making or reconsidering a consequential decision.
<!-- decision-records:reference:end -->
```

Resolve the reference from its containing file. Use a relative path to the canonical file if
it is elsewhere in the workspace. Keep the section heading stable so its anchor works.
Aliases need no reference to themselves. If the canonical choice changes on a later setup,
move the section and update the reference rather than leaving either behind.

## Verify and report

Inspect the resulting files and diff. Check one full section across physical files, a working
reference where needed, preserved unrelated content and links, concrete collection/index
locations, and no duplicate index or conflicting rules. Repeating setup must update the same
section and reference in place.

Report the files changed, where records and their index live, the adopted conventions and
any unresolved conflict. Setup alone does not authorize accepting a decision, committing,
pushing or publishing. Explain that future agents must actually load the workspace
instructions for the practice to take effect.
