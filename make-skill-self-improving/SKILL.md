---
name: make-skill-self-improving
description: Use when the user asks to make a skill self-improving, self-updating, or able to learn from its mistakes, or wants a skill to update its own instructions when corrected. Adds an update loop to one named skill, with a chosen review mode, a line budget, and rules that keep learned entries scoped and pruned.
---

# Make a skill self-improving

Add an update loop to one named skill, so that when a run of that skill goes wrong or gets
corrected, the skill's own instructions change and the mistake does not repeat.

"Setup" means one run of this skill on one target skill. Say "setup", not "install".

## Setup

Work on the one skill the user named. If they named none, ask which. Do not touch any other
skill.

1. Read the target. Read its `SKILL.md` and every file it links to.
2. Fit check. Stop, change nothing, and explain, if the target is any of these:
   - Owned by a plugin or package manager. Resolve the real path first, so a symlink into a
     plugin or package directory counts. Say that an update of the plugin or package would
     overwrite the learned edits.
   - One-off. Never expected to run again. Say there is nothing to learn for a next run.
   - Already looping. It already has its own update loop. Say so.

   Size never stops setup. A 12-line skill is fine.
3. Ask the review mode. Ask the user to pick one:
   - `automatic`: the skill edits its own files when a signal fires and reports each edit.
     It never commits.
   - `proposals`: the skill shows the diff in its reply and applies it only once the user
     approves.

   If the question is unanswered, or the session is unattended (no one to ask), use
   `proposals`.
4. Untracked target in automatic mode. If the target is not tracked by git (its `SKILL.md` is
   not in `git ls-files`, or there is no repo) and the mode is `automatic`, warn once that its
   edits have no undo, then respect the choice. Never offer `git init`. Do not check git again
   on later edits.
5. Compute the line budget. `max(1.5 x the target's SKILL.md line count now, 100)`, measured
   before the loop is added. The "Updating this skill" section is excluded from the count, and
   supporting files are uncapped.
6. Add the loop (see "What setup adds"). Leave the target's purpose, trigger, voice and
   structure alone. Only add the loop. Create no supporting files and restructure nothing.
7. Report. Show the diff, and the routing table with one line per row on why that file is
   there. Do not commit or push.

## What setup adds

A final workflow step, "Update this skill", that says to run the section below at the end of
every run; the "Updating this skill" section (mode, budget, signals, routing table, rules, in
that order); and a last output item, "Skill updates". If the target has no numbered workflow or
output list, append a closing step or a closing paragraph instead. Use the text below as the
section, adapt headings and voice to the target, keep every rule's meaning, and replace the mode
and budget values on the two lines under the heading with the real ones, and change nothing else.

```markdown
## Updating this skill

Mode: proposals
Line budget: 100

Run this after every run. Change the skill only when one of these signals happened:

- the user corrected the output or an approach
- a step failed and a retry succeeded
- the user confirmed or rejected a guess you made
- the user stated a preference about the task's output

A later correction in the same conversation counts. If none happened, report
"Skill updates: none" and do not look for lessons.

| Lesson is about | Goes in |
| --- | --- |

A row is added here when a supporting file is created.

Rules:

- Write each lesson at the narrowest scope the evidence supports, with its condition in the
  entry ("In repos with a `pnpm-lock.yaml`, use pnpm"). If a later signal in a new context
  matches an existing entry, widen that entry. Do not add a sibling.
- If a run proves an entry wrong, delete it. No counter-entries, no caveats.
- Record a retry lesson only when the retry succeeded. Record the approach that worked, with
  the condition under which the first attempt failed.
- Put a lesson next to the step it changes. If the skill has no workflow step to attach it to,
  keep it in a "Learned" list.
- Only entries this loop added may be merged, widened or deleted. A lesson that contradicts
  an original instruction is proposed in the reply, in either mode, and not applied.
- Record only the lesson: no personal data, no account of what happened in the run.
- A preference about the task's output goes in this skill. A preference about general taste
  (terseness, emojis, how much to explain) does not: do not record it, and tell the user to
  put it in their own CLAUDE.md or AGENTS.md. Test: would a different user of this skill want
  it too? If yes, it is about the task.
- Keep SKILL.md within the line budget, not counting this section. If an edit would exceed
  it, first merge entries. If that is not enough, create one supporting file, move detail into
  it, add its row to the table above, and link it from SKILL.md.
- Never change this skill's purpose, its trigger, or its safeguards on risky or outward-facing
  actions. Propose such a change in the reply instead, whatever the mode.
- Never commit, push or publish.

Skill updates (end every run with this output item):

- clean run: `Skill updates: none`
- in automatic mode: one line per edit, `Skill updated: <file>: <what changed>`
- in proposals mode: the diff, then ask whether to apply it. Apply nothing until the user
  approves. If the user declines, or nobody can approve (an unattended or one-shot run), report
  the diff and drop the proposal.
- a general-taste preference: one line pointing at the user's CLAUDE.md or AGENTS.md.
```

Notes for setup:

- Write the mode line exactly as `Mode: automatic` or `Mode: proposals`. The user switches by
  editing that one word. A missing or unreadable mode means proposals.
- The routing table lists only supporting files the target already has, never one that does
  not exist. Write the "A row is added here..." sentence only if the target has no supporting
  files; otherwise add one row per file. Delete the sentence once any row exists.

## Limits to tell the user

- In automatic mode on a target not tracked by git, edits cannot be undone.
- "Tracked by git" only helps if someone reads the diff.
- The loop learns from signals in the conversation. It does not measure whether the skill got
  better, and it does not prune entries for being unused.

Works the same in Claude Code and Codex. Use no tool-specific features.
