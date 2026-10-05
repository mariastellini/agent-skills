# agent-skills

Skills I've written for coding agents (Claude Code, Codex) and use in my own work. Each one lives in its own folder with a `SKILL.md` describing when it triggers and what it does.

## Skills

- [make-skill-self-improving](make-skill-self-improving/SKILL.md): Adds an update loop to one skill, so its own instructions change when a run gets corrected. Lessons are scoped, wrong ones deleted, and the file is capped by a line budget.
