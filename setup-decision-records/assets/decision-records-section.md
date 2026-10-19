<!-- decision-records:section:start -->
## Decision records

Keep consequential choices and their reasoning in `{{collection}}`; consult `{{index}}` to
find them. This practice covers all work in this workspace, including research, business,
creative and technical decisions. Keep ongoing behavior here and decision-specific reasoning
in the records.

Before substantive work, consult the index and read relevant records. If the index is
inconclusive, search the collection for the topic, alternatives and related terms, then read
the matches that matter. Do not load the whole collection by default. When a past decision
materially informs advice or conflicts with a proposal, briefly explain its reasoning and
link to its record. Past choices can be reconsidered when circumstances change.

When a settled choice has lasting consequences and its reasoning would otherwise be lost,
propose a decision record. Skip routine execution choices. Show the proposed decision and
reasoning, including the material alternatives and consequences, for user confirmation
before writing it as accepted. Discussion, an agent suggestion or a declined proposal is not
acceptance. If the user corrects the proposal, confirm the corrected content. Never invent
missing rationale or rejected alternatives; ask when the gap matters and state honestly
when no alternatives were discussed.

After confirmation, write one readable Markdown file per decision using the workspace's
record convention: {{record convention}}. Include context, decision, rationale, alternatives,
consequences and status. For a new collection, use local write-time
`YYYY-MM-DD-HH-MM-descriptive-slug.md`. Read the actual local clock when writing. Different
slugs may share a minute. If the complete filename exists, choose a distinct suffix; never
overwrite a record or invent a future timestamp to avoid a collision. Preserve an existing
collection's naming convention when it differs.

Maintain the index whenever creating or superseding a record: {{index maintenance}}. For a
new collection, use a plain Markdown index with a link, title, status and one-sentence scope
for each record. Adopt existing index automation instead of maintaining a competing index.

When changing an accepted decision, propose the replacement and reasoning for confirmation,
then create a new record linked to its predecessor. Mark the predecessor superseded using
{{supersession convention}}, link it to the replacement and update the affected index entries.
Preserve the earlier reasoning; do not silently rewrite accepted rationale. A declined
replacement leaves the accepted record and index status unchanged.
<!-- decision-records:section:end -->
