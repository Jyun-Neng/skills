# Access through Google Drive

Use the Google Drive tools `search_files`, `download_file_content`, `create_file` and `trash_file`. `download_file_content` returns base64, so decode it before reading.

## Looking up the folder IDs

The Drive tools don't accept paths like `english/MEMORY.md`; every call needs a file or folder ID. So at the start of each request, turn the known folder names into the IDs you'll need: the `english` root, `MEMORY.md`, `archive/` and the vault folder. The layout is fixed, so do this by name instead of asking the user where anything is:

1. Search `title = 'english' and mimeType = 'application/vnd.google-apps.folder'`, then list each match's children (`parentId = '<id>'`). The root is the match containing an `archive` folder and a vault folder.
2. From that listing take the IDs of `MEMORY.md`, `archive` and the vault folder.
3. If several `english` folders match, ask the user which one. If the user named a different root folder earlier in this conversation, search for that name instead.
4. If none is found, or the search fails, don't create a folder or guess another one. Tell the user you couldn't find the `english` folder on their Google Drive, and continue as in "If the vault can't be reached" in SKILL.md.

## Searching and reading

- **Listing a folder** (the root or `archive/`): search `parentId = '<folder id>'`. Results come in pages: keep passing the returned `nextPageToken` as `pageToken` until a reply comes back empty (`{}`), or you'll miss files.
- **Finding notes by exact name**, all in one query: `parentId = '<vault folder id>' and (title = 'retaliate.md' or title = 'means.md')`. This returns the file IDs you need to read or update them.
- Set `excludeContentSnippets: true` on every search; only names and IDs are needed.
- **Reading a file:** `download_file_content` with its ID, then decode the base64.

## Creating a file

Call `create_file` with:
- `title`: the filename, e.g. `<slug>.md`
- `parentId`: the ID of the folder it goes in
- `textContent`: the Markdown
- `contentMimeType`: `text/markdown`
- `disableConversionToGoogleType`: `true`. Without this, Drive converts the file into a Google Doc and Obsidian can't read it.

The same settings apply to notes, MEMORY.md and archive files.

## Replacing a file's content

The Drive connector can't edit a file's content in place. To replace a note or MEMORY.md:
1. `create_file` the new content with the same title and parent (settings above).
2. Only after the create succeeds, `trash_file` the old file. Order matters: if creation fails, the original must still exist. If the trash call fails, tell the user in one line that two copies of the file now exist and the older one can be deleted, since Obsidian would otherwise show a duplicate.
