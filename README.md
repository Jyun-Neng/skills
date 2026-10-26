# skills

Personal [Agent Skills](https://docs.claude.com/en/docs/claude-code/skills) for Claude.

| Skill | What it does |
|---|---|
| [english-vault](english-vault/SKILL.md) | Explains English words and phrases and saves them as linked notes in an Obsidian vocabulary vault, read from a local copy when one is found or from Google Drive otherwise. Also builds reviews such as quizzes, flash cards and stories. |
| [gmail-inbox-review](gmail-inbox-review/SKILL.md) | Goes through the Gmail inbox thread by thread: labels, trashes noise, flags follow-ups, drafts replies and writes a report. |

## Layout

Each skill is a folder with a `SKILL.md` (YAML frontmatter with `name` and `description`, then instructions). It can also have a `references/` folder for material that loads only when needed.

## Install

**Claude Code:** copy or symlink a skill folder into `~/.claude/skills/`:

```sh
ln -s "$PWD/english-vault" ~/.claude/skills/english-vault
```

**Claude.ai:** zip the skill folder and upload it under Settings → Capabilities → Skills.
