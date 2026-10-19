# Set up decision records in workspace instructions

Status: Accepted

## Context

Consequential choices lose their reasoning, rejected options and assumptions in agent
conversations. A manually invoked recording skill would require the user to remember to
capture each choice. The practice should serve knowledge workers doing research, business,
creative or technical work, without this repository's author or private tooling being present.

## Decision

Ship `setup-decision-records` as a standalone setup skill. It adds ongoing recording and
retrieval rules to workspace agent instructions, using a portable section template. Call the
artifacts decision records so the practice applies beyond architecture and software.

The agent proposes a record when a settled choice has lasting consequences, skips routine
execution choices and confirms the decision and reasoning with the user before recording it
as accepted. Records distinguish known rationale and alternatives from missing information.

Use one canonical instruction section. Preserve an existing canonical choice; otherwise
prefer `AGENTS.md` when separate `AGENTS.md` and `CLAUDE.md` files exist, and put a read
instruction in the other file. Linked files share one physical target and keep their links.
This avoids maintaining duplicate rules that can drift. Repeated setup updates the existing
section and reference while preserving unrelated content and adopting existing conventions.

Use one readable file per decision and a compact index. New collections use local write-time
`YYYY-MM-DD-HH-MM-slug.md` filenames. Different slugs may share a minute; a complete filename
collision gets a distinct suffix without overwriting history. The agent maintains a plain
Markdown index when creating or superseding records. Adopt existing index automation rather
than creating a competing mechanism.

At the start of substantive work, read the index and relevant records, searching when the
index is inconclusive. Explain relevant past reasoning when it informs advice or conflicts
with a proposal. A confirmed replacement gets a new record linked to its predecessor; mark
the predecessor superseded while preserving its reasoning and update the index.

## Rationale

Setup puts the practice in instructions future sessions already load, so capture does not
depend on remembering a command. Confirmation prevents suggestions or declined proposals
from becoming accepted history. Selective retrieval retains useful context without loading
the entire collection. A Markdown index needs no generator, hooks or installation in the
adopting workspace. Keeping the behavior in one canonical file avoids conflicting copies.

## Alternatives

A manually invoked recording command was considered and rejected because it relies on the
user remembering to invoke it. Separate duplicated rule sections were considered during
scoping; Maria chose one canonical section with a reference from the other file. For the
index, Maria chose agent-maintained Markdown rather than requiring a generator or hooks.
No other rejected alternatives were recorded.

## Consequences

Later agents can consult accepted reasoning and explain when advice conflicts with it.
Users can reconsider choices without losing their history. Prompting too often would burden
users, so the installed rules limit proposals to consequential choices. The practice depends
on agents loading and following workspace instructions; setup cannot enforce compliance.
No separate database, scheduled review, capture-only companion skill or private runtime
dependency is introduced.
