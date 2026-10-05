---
name: make-skill-self-improving
description: Use when the user asks to make a skill self-improving, self-updating, or able to learn from its mistakes, or wants a skill to update its own instructions when corrected. Adds an update loop to one named skill, with a chosen review mode, a line budget, and rules that keep learned entries scoped and pruned.
---

# Make a skill self-improving

Add an update loop to one named skill, so that when a run of that skill goes wrong or gets
corrected, the skill's own instructions change and the mistake does not repeat.

Skills that edit themselves tend to accumulate entries, turn one-off corrections into rules,
and absorb one person's taste. This skill adds the loop together with guardrails against that.

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
5. Compute the line budget. `max(1.5 x the target's SKILL.md line count at setup, 100)`.
   Measure it before the loop is added. The budget covers SKILL.md only, not supporting files,
   and the "Updating this skill" section does not count toward it: the budget covers the
   target's own lines plus learned entries.
6. Add the loop (see "What setup adds"). Leave the target's purpose, trigger, voice and
   structure alone. Only add the loop. Create no supporting files and restructure nothing.
7. Report. Show the diff, and the routing table with one line per row on why that file is
   there. Do not commit or push.

## What setup adds

Three pieces, in the target's own voice and heading style:

1. A final workflow step, "Update this skill". It points at the section below and says to
   run it at the end of every run.
2. An "Updating this skill" section containing, in this order:
   - `Mode: automatic` or `Mode: proposals`
   - `Line budget: N`
   - the signal list
   - the routing table
   - the rules
3. A last output item, "Skill updates".

If the target has no numbered workflow or no output list, append a closing step or a closing paragraph instead. Use this text as the section; adapt the headings and voice to the target, but keep every rule's meaning. Replace the mode and budget values with the real ones:

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

A row is added here when a supporting file is created. (Setup: write this sentence only if the
skill has no supporting files, otherwise add one row per file. Delete it once any row exists.)

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
  it, first merge entries. If that is not enough, move detail into a supporting file, add its row to the table above, and
  link it.
- Never change this skill's purpose, its trigger, or its safeguards on risky or outward-facing
  actions. Propose such a change in the reply instead, whatever the mode.
- In `proposals` mode, if nobody can approve (an unattended or one-shot run), report the diff
  and drop the proposal.
- Never commit, push or publish.
```

Rules for the pieces:

- Mode line. Written exactly as `Mode: automatic` or `Mode: proposals`. The user switches by
  editing that one word. Every run reads it. A missing or unreadable mode means `proposals`.
- Line budget line. Written as `Line budget: N` beside the mode line. The user raises it by
  editing it.
- Routing table. Lists only supporting files the target already has at setup. A target with
  none gets the table header, no rows, and the sentence that a row is added when a supporting
  file is created. Never list a file that does not exist.
- Skill updates output item. Every run of the target ends with a "Skill updates" item:
  - clean run: `Skill updates: none`
  - `automatic` mode: one line per edit, `Skill updated: <file>: <what changed>`
  - `proposals` mode: the diff, then the question whether to apply it. Apply nothing until
    the user approves. If the user declines, or nobody can approve (an unattended or one-shot
    run), report the diff and drop the proposal.
  - a general-taste preference: one line pointing at the user's CLAUDE.md or AGENTS.md.

## Supporting files come later

Setup creates none. The first edit that would push SKILL.md over the line budget (after
merging entries) creates one supporting file, adds its routing row, moves the detail into it,
and links it from SKILL.md. Supporting files have no line cap.

## Limits to tell the user

- In automatic mode on a target not tracked by git, edits cannot be undone.
- "Tracked by git" only helps if someone reads the diff.
- The loop learns from signals in the conversation. It does not measure whether the skill got
  better, and it does not prune entries for being unused.

Works the same in Claude Code and Codex. Use no tool-specific features.
