# Review mode: read-only practice from the vault

The user wants to review words they already saved: an article or story, flash cards, a quiz or test, a list of what they learned, and so on. The vault is their record, and a review task has no reason to change it.

## Read-only rule

Never create, change or trash anything inside the `english` folder in this mode: not MEMORY.md, `archive/` or any note, even if something looks off (e.g. MEMORY.md has 20 or more lines). Archiving happens the next time Save mode runs.

Deliver the result in chat. If the user asks to keep the output (an article, a quiz), save it somewhere outside the `english` folder, and ask where if they haven't said. If the user asks about a new word mid-review, that part is Save mode: read `references/vault-edit.md` for it.

## Step 1: Pick the words from the logs

Read the logs before any note. They're small and list every saved word with its date, which is usually enough to choose the word set.

- **Recent words:** MEMORY.md holds only the latest entries (fewer than 20), so even "this week" may reach into `archive/`.
- **Archive files:** list `archive/` once. A file is named by the date it was archived (`2026-09-28.md`, `2026-09-28-2.md` for a second archive that day), not by the dates of its entries. It holds the 20 entries saved since the previous archive, so it can reach back weeks: `2026-09-27.md` may start with an entry from August. Its name only tells you its newest possible date.
- **Which files to read** for a range from *A* to *B*: sort the archive files by date (a `-2` file comes after the plain one of the same day). Read every file named *A* or later, up to and including the first one named after *B*, since that one may still start with entries from before *B*. Skip files named before *A*; everything in them is older. Always include MEMORY.md; it's small and holds the newest entries.
- **Then check the content:** open those files and keep only the lines dated *A* to *B*. Pick words by each line's date, never by which file it's in.
- **Scope:** use the range the user gave ("this week", "last week", "September", "yesterday"). If they gave none, start with the most recent words (yesterday, then the rest of this week, then last week) and go further back only if you need more to reach the count the task calls for.
- A slug that appears on several log lines was looked up more than once, which means it hasn't stuck yet. Include those first when you have to choose.

If the chosen range is empty, say so and offer the nearest range that has words.

## Step 2: Read notes only when the task needs them

Read a note when the output depends on what the user saved about that word:

- **Flash cards, quizzes, tests:** read the notes. The answers must match the sense the user saved, not some other dictionary sense.
- **Articles and stories:** read notes for words with several senses, or where you're unsure which sense was saved. Common single-sense words can be used from the log alone.
- **"What did I save this week?" style lists:** the log is enough. Open notes only if the user asks for meanings.

To find the notes, don't list the vault folder; look up all the chosen slugs together by exact name (see the access file). If a logged slug has no note, skip it and mention it in one line at the end.

## Output guidance

Adapt to what the user asks for. These defaults apply when they don't specify:

- **Article or story:** an everyday topic (work, friends, errands, travel, home), with each target word in **bold** and used in the sense saved in its note. End with a line listing the words used.
- **Flash cards:** front is the word or phrase; back is the headline definition plus one example sentence. Write new examples rather than copying the note's, so the user practices the meaning instead of recognizing a sentence they've seen.
- **Quiz or test:** mix formats (multiple choice on meaning, fill in the blank, choose the right preposition or pattern, use the word in a sentence). Put the answer key at the end, or ask one question at a time if the user wants to answer interactively. When grading the user's own sentences, check them against the note's sense and patterns and explain any mistake briefly.

Write everything in English unless the user asks otherwise. Keep practice sentences natural and conversational, in line with the vault's focus on words the user can actually use.
