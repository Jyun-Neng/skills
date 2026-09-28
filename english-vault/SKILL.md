---
name: english-vault
description: Use whenever the user asks what an English word, phrase, idiom or phrasal verb means, how to use it, or how two words differ (in English or Chinese, including one expression in a pasted sentence). Explains it and saves it as a linked note in their Obsidian vocabulary vault, even if they don't say "save". Also use to review saved words, such as an article, story, flash cards or quiz built from words saved in a given period.
---

# English Vault

The user keeps a personal English vocabulary vault in Obsidian, stored on Google Drive. This skill has two modes. Pick one, then read only that mode's reference file. Each file holds everything that mode needs, so there's no reason to read the other one.

| Mode | When | Read |
|---|---|---|
| **Save** | The user asks about a word or phrase: meaning, usage, difference between two words, an expression in a pasted sentence. Saving is the default; the user doesn't have to say "save". | `references/vault-edit.md` |
| **Review** (read-only) | The user wants to practice what they already saved: an article, story, flash cards, quiz or test, a list of this week's words, and so on. | `references/vault-review.md` |

If one request mixes the two, for example "quiz me on this week's words, and what does *begrudge* mean?", handle each part in its own mode, reading both files. Review mode never changes the vault; any note or log change goes through Save mode.

## Vault layout (Google Drive)

```
english/                        root folder, always named "english"
├── MEMORY.md                   running log of the latest entries (under 20 lines)
├── archive/
│   └── YYYY-MM-DD[-N].md       20 entries each, named by the date they were archived
└── <vault folder>/             the Obsidian vault: the one subfolder containing .obsidian/
    ├── .obsidian/              never touch
    └── <slug>.md               one note per word/phrase
```

Log lines in MEMORY.md and archive files look like `- YYYY-MM-DD — [[slug]]`, oldest first. When MEMORY.md reaches 20 lines they move to a new archive file. Weeks (for "this week" in review) run Monday to Sunday.

Use the Google Drive tools `search_files`, `download_file_content`, `create_file` and `trash_file`. `download_file_content` returns base64, so decode it before reading.

### Looking up the folder IDs

The Drive tools don't accept paths like `english/MEMORY.md`; every call needs a file or folder ID. So at the start of each request, turn the known folder names into the IDs you'll need: the `english` root, `MEMORY.md`, `archive/` and the vault folder. The layout is fixed, so do this by name instead of asking the user where anything is:

1. Search `title = 'english' and mimeType = 'application/vnd.google-apps.folder'`, then list each match's children (`parentId = '<id>'`). The root is the match containing an `archive` folder and a vault folder.
2. From that listing take the IDs of `MEMORY.md`, `archive` and the vault folder: the subfolder other than `archive` that contains `.obsidian`. Its name can be anything (e.g. `my-english-vault`). The root may have its own `.obsidian/` too (when `english/` itself is opened in Obsidian); that's settings, not the vault folder, and is never touched.
3. If several `english` folders match, ask the user which one.
4. If none is found, or the search fails, don't create a folder or guess another one. Tell the user you couldn't find the `english` folder on their Google Drive, and continue as in "If the vault can't be reached" below.

If the user named a different root folder earlier in this conversation, use that instead.

### The logs are the index; never list the vault folder

The vault folder keeps growing, so never list its contents, not even once per request. Every note ever created or updated has a line in MEMORY.md or an archive file, so the logs are the index of what exists:

1. **Which notes exist:** download MEMORY.md and every file in `archive/` (listing `archive/` is fine; it grows by only one small file per 20 saves), and collect the slugs from their lines. A slug that appears there has a note; one that doesn't, has no note.
2. **Getting a note's file ID** (to read or update it): search for just the notes you need by exact name, all in one query: `parentId = '<vault folder id>' and (title = 'retaliate.md' or title = 'means.md')`.

Download the logs once per request and reuse them for every word in it. In review mode, download only the log files that can hold the requested dates, then filter by each line's date (see `references/vault-review.md`).

Drive search returns results in pages. When listing a folder (the root or `archive/`), keep passing the returned `nextPageToken` as `pageToken` until a reply comes back empty (`{}`), or you'll miss files. Set `excludeContentSnippets: true` on every search; only names and IDs are needed.

For today's date, run `date +%F` if a shell is available (it's the user's local time); otherwise use the date given in context.

## If the vault can't be reached

This covers Drive being unavailable and the `english` folder not being found. Save mode: answer the question fully, say in one line that the vault couldn't be reached, and include the note text in a code block so nothing is lost. Review mode: say the vault couldn't be reached and offer to work from words the user lists in chat.
