# Save mode: create and update notes

Every word or phrase the user asks about gets explained in chat **and** saved as a note, so nothing they look up is lost and the vault grows into a linked web of vocabulary they can review.

Skip saving only if the user says not to, or the question isn't really about vocabulary (e.g. proofreading a whole email). In that case just help, and offer in one line to save any notable word.

## Workflow for each request

1. **Identify the entries, one note per word or phrase the user asked about.** A note covers exactly one word or phrase; never bundle two into one note. If they ask about two ("X vs. Y"), that's two notes, each covering its own word; the comparison table goes in only one of them (see "Comparing words"). Don't create notes for words they didn't ask about (e.g. a related adjective you mention in passing). Mention those inline, link them if a note already exists, and let the user ask about them later.
2. **Check for existing notes** using the logs, as described in "The logs are the index" in SKILL.md. The slugs in the logs also tell you which related words already have notes you can link inline. Then look up every word you're about to write by exact name, all at once (see the access file): it finds the notes you'll update, and it catches a note that's missing from the logs (e.g. one the user created by hand), which you should then update instead of duplicating.
3. **Answer in chat** with the explanation itself, in the note's style. Lead with the meaning; don't narrate the saving process.
4. **Write the notes**, new or updated (see "Updating an existing note").
5. **Update the log**: append, then archive if MEMORY.md reaches 20 lines.
6. **Close with one line** listing what was saved, e.g. `Saved: [[retaliate]] (new), [[means]] (updated).`

## Slugs (filenames)

- Lowercase, kebab-case, `.md`: `retaliate.md`, `snuff-out.md`, `means-to-an-end.md`, `make-of.md`.
- Use the base/dictionary form (`revive`, not `reviving`), unless the inflected form is the point.
- Drop leading articles in idioms ("a means to an end" → `means-to-an-end`).
- When the user asks about one specific sense of a word that deserves its own note, add a suffix: `end-goal-sense`, `causative-get`.

## Note format

Plain Markdown, no YAML frontmatter, no H1 title (the filename is the title):

```markdown
**Retaliate** (verb) — to attack, harm, or respond to someone in return for a similar attack or harm they did to you; to get revenge or "hit back."

- "He **retaliated against** his rival after being insulted." (usually paired with "against")
- "She chose not to **retaliate** when he provoked her." (used alone, intransitively)
- "My little brother took my charger, so I **retaliated** by hiding his headphones." (everyday, playful use)

Grammar note: noun form "retaliation" — "**in retaliation for** the insult."

Register note:
- Formal or written: common in news, law and essays ("the army retaliated").
- In daily speech, people usually say "**get back at** someone" or "**hit back**": "I'll **get back at** him for that."

Context: met in an essay on justice — retaliation is doing wrong back to someone who wronged you first.
```

Building blocks. Use the ones that earn their place, not all of them every time:

- **Headline line:** `**Headword** (part of speech) — clear English definition.` Use `(idiom)` for idioms and `(phrasal verb)` for phrasal verbs.
- **Senses:** if the word has several distinct senses, number them with bold labels (`1. **Literal — ...:**`, `2. **Figurative — ...:**`) and nest examples under each.
- **Examples (required, at least one):** quoted sentences with the target in bold, plus a short parenthetical explaining the pattern: the preposition it takes, transitive vs. intransitive, what the phrase equals. Favor natural, everyday situations (friends, work, errands, travel, home) over technical, academic or source-text phrasing, because the point is words the user can actually use in conversation. Don't lead with the sentence the user found the word in; that belongs in the context line.
- **Grammar note / quirks:** e.g. "means" is invariant in number. Derived forms (noun "retaliation", adjective "begrudging") can be mentioned here or in an example, briefly.
- **Register note:** a `Register note:` line followed by a bulleted list, never a run-on sentence: one bullet for where the word sits (formal/written vs. casual/spoken), and one for what a native speaker would say instead in daily speech, if that differs. A comparison with another word appears here only as a pointer bullet to its table; see "Comparing words". Add other bullets only for a real extra point (e.g. sounds old-fashioned, American vs. British). One point per bullet.
- **Comparison table:** only in the one note that holds a comparison; see "Comparing words". Goes right after the register note, before the context line.
- **Context line:** if the user gave the sentence or text where they met the word, end with a line tying the word back to that context. If they didn't, skip it; don't invent one.

Write notes in English. Add a translation or gloss in another language only if the user asks for it. Keep notes focused: roughly 5–25 lines.

### Related words go inline, not in a section

Don't add a separate "Related", "See also", "Related forms" or "Not to be confused with" section. When another word matters (a synonym, a contrast, a look-alike, the idiom it's part of), mention it in the sentence where it's relevant, and link it right there if it has a note: "Close to [[reluctantly]], but with resentment added." / "Pairs with [[means]]: a *means* is the tool, an *end* is the goal." This keeps each connection next to the explanation that makes it meaningful. The one exception is the comparison table for an "X vs. Y" question, below.

### Comparing words: one table per group of similar words

When the user asks how two or more words differ, the full comparison is a table that lives in **one** note only. Every other note in the comparison gets a pointer bullet in its register note. Similar words form a group (e.g. *arrange*, *orchestrate*, *organize*), and a group shares one table, so the comparison has one home and never goes out of sync.

**Finding an existing table.** Read the notes of the words being compared (you read them anyway to update them). Each one either holds a table, holds a pointer bullet naming the note that does, or has neither.

**Which note gets the table:**
- **Any of the words is already in a table:** add the new words as columns to that table, in the note where it already lives, and add a row if a new difference matters. Update the bold title line to name all the words. A table never moves once created.
- **None is in a table yet:** create the table in the newest note. A note created in this request is newer than any existing note; if several are created now, use the word the user named last. If all the notes already exist, use the one created most recently (its first log line is the latest).
- **Size limit:** a table holds at most 4 words. If adding would make a fifth, start a new table for the new comparison in the newest note instead.
- **Words in two different tables:** don't merge the tables. Put the new comparison in the table of the word the user named first, if it has room.

**The table** starts with a bold title line, has one column per word and one row per way they differ. Pick 3–6 rows that really differ (meaning, scale, connotation, register, typical pattern, example); keep cells short. The note's own word is bold, the other words are wikilinks:

```markdown
**Arrange vs. orchestrate**

| | **arrange** | [[orchestrate]] |
|---|---|---|
| Meaning | sort out details so something happens | coordinate many moving parts toward one result |
| Scale | simple, everyday plans | big or complex efforts |
| Connotation | neutral | can hint at secret manipulation |
| Register | everyday | fairly formal |
| Example | "**arrange** a meeting" | "**orchestrate** an office move" |
```

**The pointer bullet** goes in the register note of each other note in the comparison, with no copy of the table's content:

```markdown
Register note:
- Fairly formal.
- Differs from [[arrange]]: see the comparison table in [[arrange]].
```

Name every word this note is compared with in that table: `- Differs from [[arrange]] and [[organize]]: see the comparison table in [[arrange]].` If the note has no register note yet, add `Register note:` with the pointer as its bullet. If the note already has a comparison with those words written out (e.g. an older "Compare [[arrange]]" bullet or line), replace it with the pointer bullet, since the table now holds that content. This is the one case where an update removes existing text.

Only notes the user asked about in this request get a new or changed pointer bullet. When a word joins a table, the other notes already pointing to it stay as they are; their pointer still leads to the right table.

### Wikilinks

Link inline with `[[slug]]`, or with `[[slug|display text]]` when the displayed word differs (`[[end-goal-sense|end]]`). Link only to notes whose slug appears in the logs or that you are creating in this same request, so the vault has no dangling links.

## Creating a note

Create `<slug>.md` in the vault folder, as the access file describes.

## Updating an existing note

To update:
1. Read the existing note to get its exact current text.
2. Merge: keep everything already there and add the new sense, examples, context or inline links where they fit. Add each new point as a new bullet in the block it belongs to (e.g. a new register point becomes a new bullet under `Register note:`), never by extending an existing sentence or paragraph. Don't rewrite or reorder what the user already has; the only exception is replacing a written-out comparison with a pointer bullet (see "Comparing words").
3. Replace the note's content with the merged text, as the access file describes.

Every update gets a log line, including a pointer bullet added for a comparison. A re-asked word is a sign it hasn't stuck yet, and the repeated log line records that.

## MEMORY.md log and archive

Each line is `- YYYY-MM-DD — [[slug]]` (em dash, date in the user's local time zone). Add one line per note created or updated in this request, appended at the bottom so the log reads oldest to newest.

MEMORY.md holds at most 20 lines. Archiving is by count, not by date:

1. Use the MEMORY.md read for workflow step 2 of this request, never a copy from an earlier turn. If it doesn't exist (first save, or it was deleted), start from an empty list.
2. Append today's new lines.
3. While the list has 20 or more lines, move the oldest 20 into a new archive file. Anything past 20 stays in MEMORY.md (e.g. 19 lines + 3 new = 20 archived, 2 kept).
4. Write the remaining lines as the new MEMORY.md. If none remain, still write MEMORY.md as an empty file so the layout stays intact.

**Archive file names** use the date the archive is made, not the dates of the entries: `archive/YYYY-MM-DD.md`. Check the `archive/` listing from workflow step 2; if that name is taken (an earlier archive today), add a counter: `YYYY-MM-DD-2.md`, then `YYYY-MM-DD-3.md`, and so on. An archive file is always a new file, never merged into an existing one.

Create the archive file first and replace MEMORY.md's content second (both as the access file describes), so a failure midway never loses lines.

When an archive is made, mention it in the closing line, e.g. `Saved: [[retaliate]] (new). Archived 20 entries to archive/2026-09-28.md.`
