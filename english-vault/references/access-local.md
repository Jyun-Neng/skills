# Access through a local path

The `english` root is a local folder, found as described in SKILL.md (often a symlink to, or a synced copy of, the Google Drive folder). Work on it with ordinary file tools; there are no IDs to look up and nothing to decode. Nothing here calls the Drive tools; if the folder is synced with Google Drive, changes reach it through that sync.

## Finding the vault folder

List the root once: the vault folder is the subfolder other than `archive` that contains `.obsidian/`. If it isn't there, continue as in "If the vault can't be reached" in SKILL.md; don't create one.

## Searching and reading

- **Listing a folder:** list the root or `archive/` directly (e.g. `ls`). Never list the vault folder.
- **Finding notes by exact name:** check just the paths you need, all at once, e.g. `ls <vault>/retaliate.md <vault>/means.md 2>/dev/null`. A path that exists is a note to update, even if its slug is missing from the logs (e.g. one the user created by hand).
- **Reading a file:** read it by path.

## Creating a file

Write the new file by path, as plain UTF-8 Markdown, e.g. `<vault>/<slug>.md` or `archive/<name>.md`. Never overwrite an existing file this way when creating; check that the path is free first.

## Replacing a file's content

Edit the file in place: for a note, apply the merge as edits to the existing text; for MEMORY.md, rewrite it with the new lines. There's no create-then-trash step, and no duplicate copies to clean up.

## If something fails

Don't switch to the Drive tools partway through a request. If a local write fails (e.g. the folder became unreachable), stop and tell the user in one line which files were written and which weren't, and include any unwritten note text in a code block so nothing is lost.
