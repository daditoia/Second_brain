# Agent Instructions — Second Brain Vault

## Bootstrap mode

If `50_System/entity-registry.md` is empty or contains only the template header, enter bootstrap mode:

1. Ask the user:
   - Their full name and role/title
   - Their team name and manager's name
   - 5-10 key people they work with (name + role + relationship)
   - 3-5 key products, systems, or projects in their domain
   - Any acronyms or glossary terms specific to their org
2. Generate:
   - `50_System/entity-registry.md` populated with all named entities
   - Person pages in `20_People/` for the user and each person named
   - Wiki stub pages in `40_Wiki/` for each product/system/team
   - `40_Wiki/Acronyms and Glossary.md` with their domain terms
   - Update `40_Wiki/index.md` with all new pages
3. Confirm with the user that everything looks correct before proceeding.

After bootstrap, switch to normal operating mode below.

---

## Folder structure

| Folder | Purpose |
|---|---|
| 00_Inbox/ | Raw unprocessed drops. Immutable input. Never modify. |
| 10_Meetings/ | One processed meeting note per meeting. LLM-maintained. |
| 20_People/ | One profile per person. LLM-maintained. |
| 30_Logs/ | Action Register, Open Questions, Decision Log. Operational layer. |
| 40_Wiki/ | Synthesis layer: entity pages, concept pages, index, log. |
| 50_System/ | Agent config and bookkeeping. |
| 60_Archive/ | Processed inbox items and deprecated notes. |
| 70_Templates/ | Markdown templates for meetings, people, PRDs. Used during ingest. |
| 80_Deliverables/ | Authored work output: PRDs, proposals, strategy docs, briefs. |
| 99-Attachments/ | Images and embedded assets. Do not modify. |

---

## Entity registry

Before creating any wikilink or new page, check **50_System/entity-registry.md** for the canonical name.

- People: always `[[First Name Last Name]]`
- Products, teams, concepts: `[[Entity Name]]` (e.g. `[[ProductX]]`, `[[Team Alpha]]`)
- Never use partial names or nicknames in wikilinks
- When a wikilink target does not yet exist as a file, create a stub page in `40_Wiki/`
- When a new canonical name is established, add it to `50_System/entity-registry.md`

---

## Ingest workflow

Processing is always triggered manually by the user. When they ask to process inbox items:

1. **Read** every unprocessed file in `00_Inbox/` fully before writing anything.
   - When an inbox file contains embedded images (`![[filename.png]]`), read the image from `99-Attachments/`. These often contain names, org charts, or data tables.
2. **Identify** what type each item is: meeting, person info, topic info, decision, or mixed.
3. **Raise questions** if content is ambiguous or contradicts existing knowledge.
4. **Write** output files using the correct template (see `70_Templates/`).
5. **Update** all affected files: person pages, wiki entity pages, logs.
6. **Append** to `30_Logs/` (action items, open questions, decisions as appropriate).
7. **Append** an entry to `40_Wiki/log.md`.
8. **Update** `40_Wiki/index.md` if new pages were created.
9. **Move** the processed inbox file to `60_Archive/`.
10. **Surface** any naming conflicts or discrepancies found during processing.

---

## File naming conventions

- Meeting notes: `YYYY-MM-DD Meeting with [Person Name].md`
- People: `[First Name] [Last Name].md` (full name, title case)
- Wiki pages: descriptive title, title case (e.g. `ProductX.md`, `Quarterly Planning.md`)

---

## Entity resolution and naming conflicts

When the same person or concept appears under different names across files:

1. **Surface the conflict** to the user before merging.
2. After confirmation, do a find-and-replace across all files.
3. Log the merge in `40_Wiki/log.md`.
4. Update `50_System/entity-registry.md` with the canonical name.

---

## What to surface vs. what to do silently

**Always ask before acting:**
- Same person appearing under two different names
- A decision in one note contradicts a decision in another
- A person's role or reporting line appears to have changed
- Content in Inbox conflicts with existing wiki knowledge

**Proceed without asking:**
- Adding new information to existing pages
- Creating new stub pages for unresolved wikilinks
- Updating action item status
- Appending to logs
- Moving processed files to `60_Archive/`

---

## Per-file update rules

**When ingesting a meeting:**
- Create meeting note in `10_Meetings/`
- Update each attendee's page in `20_People/` (add to Meetings section; update role, context if changed)
- Update or create wiki pages for entities discussed
- Append new action items to `30_Logs/Action Register.md`
- Append new open questions to `30_Logs/Open Questions.md`
- Append decisions to `30_Logs/Decision Log.md`

**When ingesting person info:**
- Update or create person page in `20_People/`
- Verify canonical name in `50_System/entity-registry.md`

**When an open question gets answered:**
- Mark it resolved in `30_Logs/Open Questions.md` (strike through and note resolution)
- Update the relevant wiki page if the answer changes a fact

---

## Log and index formats

**40_Wiki/log.md** — append-only, one entry per operation:
```
## [YYYY-MM-DD] [type] | [title]
```
Types: `ingest` | `query` | `lint` | `merge`

**40_Wiki/index.md** — one line per wiki page, grouped by category:
```
- [[Page Name]] — one-line summary
```

**30_Logs/** must always be linked from `40_Wiki/index.md`:
- [[Action Register]]
- [[Open Questions]]
- [[Decision Log]]

---

## Writing style (applies to ALL output)

1. **Expand acronyms on first use.** Full term with acronym in parentheses.
2. **Simple words.** "utilize" = "use". "in order to" = "to". "due to the fact" = "because".
3. **No vague adverbs.** Remove "almost," "significantly," "nearly," or replace with a number.
4. **Subject-verb-object.** Short sentences. One idea per sentence.
5. **No em dashes.** Use periods, commas, or rewrite.
6. **No AI giveaways.** Banned: "leverage", "elevate", "empower", "unlock", "harness", "game-changer", "next-level". No filler openers. No rhetorical questions answered by themselves.
7. **No relative dates in persisted files.** Always use ISO format (YYYY-MM-DD).

Write like a person talking to a colleague. Direct, specific, no performance.

---

## Action Register rules

- Keep lean: only high-priority, recent items.
- Prune anything older than one week with no live deadline.
- Group by owner.
- Each item links to its source meeting or conversation.
