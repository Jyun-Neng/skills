---
name: english-vault
description: Use whenever the user asks what an English word, phrase, idiom or phrasal verb means, how to use it, or how two words differ (in English or Chinese, including one expression in a pasted sentence). Explains it and saves it as a linked note in their Obsidian vocabulary vault, even if they don't say "save". Also use to review saved words, such as an article, story, flash cards or quiz built from words saved in a given period.
---

# English Vault

The user keeps a personal English vocabulary vault in Obsidian, stored on Google Drive. This skill has two modes. Pick one, then read only that mode's reference file. Each mode file holds everything that mode needs apart from file access (see below), so there's no reason to read the other one.

| Mode | When | Read |
|---|---|---|
| **Save** | The user asks about a word or phrase: meaning, usage, difference between two words, an expression in a pasted sentence. Saving is the default; the user doesn't have to say "save". | `references/vault-edit.md` |
| **Review** (read-only) | The user wants to practice what they already saved: an article, story, flash cards, quiz or test, a list of this week's words, and so on. | `references/vault-review.md` |

If one request mixes the two, for example "quiz me on this week's words, and what does *begrudge* mean?", handle each part in its own mode, reading both files. Review mode never changes the vault; any note or log change goes through Save mode.

## Choosing how to reach the vault

The vault can be reached two ways. Decide once at the start of each request, then read that access file too; it says how to find, read and write files, which the mode files leave to it.

| Access | When | Read |
|---|---|---|
| **Local** | You have shell or file tools, and a local copy of the `english` folder is found (see below). | `references/access-local.md` |
| **Google Drive** | Anything else: no shell or file tools (e.g. claude.ai), or no local copy found. | `references/access-drive.md` |

Look for a local copy in these places, in order, and use the first that matches:

1. **A path the user gave in this conversation** for their `english` folder.
2. **The working directory itself**, when the conversation was started inside the `english` folder.
3. **`english/` inside the working directory**, when the conversation was started in its parent folder.

A candidate matches only if it is a readable directory containing `archive/` and a subfolder with `.obsidian/` (the vault folder). This rules out an unrelated folder and a symlink whose target is missing. "Working directory" means the folder the conversation started in (Claude Code's primary working directory), not one an earlier command moved into. Candidates 2 and 3 can be checked in one command run from there; it prints the local root, or nothing:

```sh
for d in "$PWD" "$PWD/english"; do [ -d "$d/archive" ] && ls -d "$d"/*/.obsidian >/dev/null 2>&1 && { echo "$d"; break; }; done
```

If the user gave a path and it doesn't match, tell them in one line what's missing and ask whether to use Google Drive instead. If nothing was found otherwise, use Google Drive and say so in one line the first time in a conversation, e.g. "No local copy of your `english` folder found here, so I'm using Google Drive."

## Vault layout

```
english/                        root folder, always named "english"
├── MEMORY.md                   running log of the latest entries (under 20 lines)
├── archive/
│   └── YYYY-MM-DD[-N].md       20 entries each, named by the date they were archived
└── <vault folder>/             the Obsidian vault: the one subfolder containing .obsidian/
    ├── .obsidian/              never touch
    └── <slug>.md               one note per word/phrase
```

The vault folder is the subfolder other than `archive` that contains `.obsidian`. Its name can be anything (e.g. `my-english-vault`). The root may have its own `.obsidian/` too (when `english/` itself is opened in Obsidian); that's settings, not the vault folder, and is never touched. Other files in the root (e.g. a `CLAUDE.md`) aren't part of the vault; ignore them.

Log lines in MEMORY.md and archive files look like `- YYYY-MM-DD — [[slug]]`, oldest first. When MEMORY.md reaches 20 lines they move to a new archive file. Weeks (for "this week" in review) run Monday to Sunday.

### The logs are the index; never list the vault folder

The vault folder keeps growing, so never list its contents, not even once per request. Every note ever created or updated has a line in MEMORY.md or an archive file, so the logs are the index of what exists:

1. **Which notes exist:** read MEMORY.md and every file in `archive/` (listing `archive/` is fine; it grows by only one small file per 20 saves), and collect the slugs from their lines. A slug that appears there has a note; one that doesn't, has no note.
2. **Finding a specific note** (to read or update it): look up just the notes you need by exact name, all at once, as the access file describes.

Read the logs once per request and reuse them for every word in it. In review mode, read only the log files that can hold the requested dates, then filter by each line's date (see `references/vault-review.md`).

For today's date, run `date +%F` if a shell is available (it's the user's local time); otherwise use the date given in context.

## If the vault can't be reached

This covers Drive being unavailable and the `english` folder not being found either way. Save mode: answer the question fully, say in one line that the vault couldn't be reached, and include the note text in a code block so nothing is lost. Review mode: say the vault couldn't be reached and offer to work from words the user lists in chat.
