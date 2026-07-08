# Ingest Inbox

Process all unprocessed files in 00_Inbox/ into structured vault output.

## Pre-flight

1. List all files in `00_Inbox/`.
2. If empty, report "Inbox is empty — nothing to process." and stop.
3. Read every file fully. For files containing embedded images (`![[filename.png]]`), also read the image from `99-Attachments/filename.png` — these often contain Teams participant names, org charts, or data tables.
4. Read `50_System/entity-registry.md` for canonical name resolution.

## Classify

For each inbox item, determine its type:

| Type | Signals |
|------|---------|
| meeting | Timestamps, speaker labels, agenda items, transcript format |
| person | Info about a specific person (role, org chart, new hire) |
| topic | Product, concept, initiative, or entity info |
| decision | Explicit decision record or announcement |
| mixed | Multiple types — will be split into separate outputs |

## Present plan

Before writing anything, present a single combined plan:

```
## Ingest plan

### [filename 1]
- **Type:** meeting
- **Output:** 10_Meetings/YYYY-MM-DD Meeting with [Person Name].md
- **People:** [[Person A]], [[Person B]] (new — will create page)
- **Entities:** [[Entity X]], [[Entity Y]]
- **Actions/Decisions/Questions:** [count of each extracted]
- **Flags:** [naming conflicts, ambiguities, contradictions, or "none"]

### [filename 2]
...

---
**Questions:**
1. [Any ambiguity or conflict that needs resolution]

Proceed?
```

Wait for confirmation before writing.

## Process

### Meetings
1. Read `70_Templates/Meeting Note Template.md`. Create note in `10_Meetings/`.
2. Filename: `YYYY-MM-DD Meeting with [Person Name].md`
3. For each attendee: check entity registry, resolve aliases, update or create page in `20_People/` (use `70_Templates/Person Note Template.md` for new pages).
4. For each entity discussed substantively: update or create wiki page in `40_Wiki/`.

### Person info
1. Read `70_Templates/Person Note Template.md`. Update existing or create new page in `20_People/`.
2. Add canonical name to entity registry if new.

### Topic / entity info
1. Update or create wiki page in `40_Wiki/`.
2. Add to entity registry if new.

### Decisions
1. Append to `30_Logs/Decision Log.md`.
2. Update related wiki or person pages.

## Post-processing

After all output files are written:

1. **Action Register** — Append new high-priority action items to `30_Logs/Action Register.md`, grouped by owner. Ask before adding if priority is unclear.
2. **Open Questions** — Append unresolved questions to `30_Logs/Open Questions.md`.
3. **Decision Log** — Append decisions to `30_Logs/Decision Log.md`.
4. **Wiki log** — Append to `40_Wiki/log.md`:
   `## [YYYY-MM-DD] ingest | [short title]`
   Include a brief summary of files created, pages updated, and items added to logs.
5. **Wiki index** — Update `40_Wiki/index.md` if new wiki pages were created.
6. **Entity registry** — Update `50_System/entity-registry.md` for new canonical names.
7. **Archive** — Move each processed inbox file to `60_Archive/`.

## Edge cases

- **Unknown last names:** Flag in plan. Create page with first name only, add registry note.
- **Duplicate meetings:** Merge into existing note in `10_Meetings/` instead of creating duplicate.
- **Contradictions:** Raise in plan summary. Do not overwrite without confirmation.
- **Transcription errors:** Resolve using entity registry and context. Flag uncertain resolutions.
- **Embedded images:** Extract visible names from screenshots. Cross-reference with entity registry.
- **Multiple meetings in one file:** Split into separate meeting notes.

## Writing rules

Follow CLAUDE.md writing style: expand acronyms on first use, simple words, short sentences, subject-verb-object, no em dashes, no AI giveaways.
