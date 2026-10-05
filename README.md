# agent-skills

Skills I've written for coding agents (Claude Code, Codex) and use in my own work. Each one lives in its own folder with a `SKILL.md` describing when it triggers and what it does.

## Skills

### make-skill-self-improving

Adds an update loop to one skill, so that when a run of that skill goes wrong or gets corrected, the skill's own instructions change and the mistake doesn't repeat.

Anyone can tell an agent to edit its own skill. The trouble comes later: entries pile up, one-off corrections turn into universal rules, one person's taste gets baked into a shared skill, and edits land with no review. This skill exists to stop that. The guardrails are the point:

- **Lessons are scoped to their evidence.** Each entry states its condition ("In repos with a `pnpm-lock.yaml`, use pnpm"). A later matching case in a new context widens the entry instead of adding a sibling.
- **Wrong entries are deleted.** A run that proves an entry wrong removes it. No counter-entries, no caveats.
- **Only real signals trigger an edit.** A user correction, a failed step whose retry succeeded, a guess the user confirmed or rejected, or a stated preference about the output. A clean run reports `Skill updates: none`.
- **SKILL.md has a line budget.** The larger of 1.5 times its size at setup and 100 lines. Past that, entries are merged or detail moves into a supporting file.
- **Personal taste stays out.** Preferences about the task's output go into the skill. General taste (terseness, emojis, explanation depth) does not. The reply points you at your own `CLAUDE.md` or `AGENTS.md`.
- **Nothing is committed or published.** The loop only edits files, and never changes the skill's purpose, trigger, or safeguards on risky or outward-facing actions. It proposes those changes instead.

#### Two review modes

You pick one at setup, and can switch later by editing one word.

- **proposals** (the default, also used when you don't answer or no one is there to ask): the diff appears in the reply and is applied only after you approve.
- **automatic**: the skill edits its own files and reports each edit as one `Skill updated:` line naming the file and the change.

#### What setup adds to a target

A final workflow step ("Update this skill"), an "Updating this skill" section, and a last output item ("Skill updates"). The section looks like this:

```markdown
## Updating this skill

Mode: proposals
Line budget: 100

Run this after every run. Change the skill only when one of these signals happened:
...

| Lesson is about | Goes in |
| --- | --- |
| (one row per supporting file the skill already has) | |

Rules:
- Write each lesson at the narrowest scope the evidence supports, with its condition in the entry.
- If a run proves an entry wrong, delete it.
...
```

Setup leaves the target's purpose, trigger, voice, and structure alone. It stops, changing nothing, on skills owned by a plugin or package manager (an update would overwrite what was learned), on one-off skills, and on skills that already have an update loop.

#### Limits

- In automatic mode on a skill not tracked by git, edits have no undo. Setup warns once, then respects your choice. It never offers `git init`.
- "Tracked by git" only helps if someone reads the diff. The skill does not check that anyone does.
- It learns from signals in the conversation. It does not measure whether the skill improved, and it does not remove entries for going unused.
