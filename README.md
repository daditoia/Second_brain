# LLM-Powered Second Brain

A structured knowledge vault that uses an LLM agent (Claude Code) as its processing engine. Drop raw notes into an inbox, trigger the agent, and get structured, interlinked knowledge out.

Based on Andrej Karpathy's "LLM OS" concept, extended with typed templates, entity resolution, operational logs, and an explicit ingest workflow.

---

## Quick start

### 1. Clone this repo

```bash
git clone https://gitlab.devops.telekom.de/davide.cazzola/second_brain.git my-vault
cd my-vault
```

### 2. Open in your editor

Works with [Obsidian](https://obsidian.md) (recommended for wikilinks and graph view), VS Code, or any Markdown editor.

### 3. Install Claude Code

You need [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (CLI or VS Code extension). This is the processing engine that turns raw notes into structured knowledge.

### 4. Bootstrap your vault

Open the vault in Claude Code and run:

```
/bootstrap
```

The agent will ask you:
- Your name and role
- Your team and manager
- 5-10 key people you work with
- Key products/systems in your domain
- Domain-specific acronyms

It then generates your personalized entity registry, starter person pages, and wiki stubs.

### 5. Start using it

Drop meeting transcripts, screenshots, or raw notes into `00_Inbox/`. When ready:

```
/ingest
```

---

## Architecture

```
00_Inbox/          Raw drops. Immutable input. Never modified by the agent.
10_Meetings/       One processed note per meeting.
20_People/         One profile per person.
30_Logs/           Action Register, Open Questions, Decision Log.
40_Wiki/           Synthesis layer: entity pages, concepts, index, changelog.
50_System/         Agent config: entity registry, naming rules.
60_Archive/        Processed inbox items.
70_Templates/      Markdown templates the agent uses during ingest.
80_Deliverables/   Authored output: PRDs, proposals, guides.
99-Attachments/    Images and embedded assets. Do not modify.
```

### Design principles

- **Flat files, local-first.** Everything is Markdown. No vendor lock-in.
- **Templates enforce structure.** The agent fills in a skeleton, not free-form prose.
- **Entity registry is the single source of truth.** Every person, product, and concept has one canonical name.
- **Logs are append-only infrastructure.** Decisions, open questions, and actions are tracked across all meetings.
- **Human in the loop.** The agent never processes without your explicit trigger. It surfaces conflicts for you to resolve.

---

## Commands

The vault ships with Claude Code slash commands in `.claude/commands/`. These are the core workflows:

| Command | What it does |
|---------|--------------|
| `/bootstrap` | One-time setup. Asks about your role, team, and key people, then generates entity registry and starter pages. |
| `/ingest` | Processes everything in `00_Inbox/`. Classifies items, presents a plan, waits for confirmation, then writes structured output across meetings, people, wiki, and logs. |
| `/lint` | Health-checks the vault. Finds orphan pages, broken links, naming violations, stale data, and missing cross-references. Reports issues and asks which to fix. |

### How commands work

Claude Code slash commands are markdown files in `.claude/commands/`. When you type `/ingest` in a Claude Code session, the agent reads the command file and follows its instructions. No plugins or extensions needed.

You can add your own commands by creating new `.md` files in `.claude/commands/`.

---

## The ingest workflow

```
1. Read every unprocessed file in 00_Inbox/.
2. Identify type: meeting, person info, topic info, decision, or mixed.
3. Raise questions if content is ambiguous or contradicts existing knowledge.
4. Present a plan and wait for confirmation.
5. Write output files using the correct template.
6. Update all affected files: person pages, wiki entities, logs.
7. Append to 30_Logs/ (actions, open questions, decisions).
8. Append to 40_Wiki/log.md (audit trail).
9. Update 40_Wiki/index.md if new pages were created.
10. Move processed inbox file to 60_Archive/.
11. Surface any naming conflicts or discrepancies.
```

### What the agent asks about vs. does silently

| Always asks first | Proceeds without asking |
|---|---|
| Same person under two names | Adding new info to existing pages |
| Decision contradicts a prior decision | Creating stub pages for new entities |
| Person's role appears to have changed | Appending to logs |
| Inbox conflicts with wiki in non-trivial way | Moving processed files to archive |

---

## What makes this different

| Traditional notes / Karpathy LLM OS | This system |
|---|---|
| Free-form storage | Typed templates with enforced structure |
| Implicit recall | Explicit operational logs (actions, decisions, questions) |
| Single-file updates | Cross-file updates on every ingest |
| Model decides structure | Human-defined templates, model fills them |
| Always-on processing | Explicit trigger: you say when to process |
| No conflict handling | Agent surfaces contradictions before writing |

The key insight: the LLM is not a database. It is a **processing engine** that transforms unstructured input into structured, interlinked knowledge. You own the structure. The LLM does the labor.

---

## Daily workflow

```
Morning:     Drop yesterday's meeting transcripts into 00_Inbox/
Trigger:     /ingest — agent runs the full ingest workflow
Review:      Agent surfaces conflicts or ambiguities. You answer.
Query:       "What do we know about X?"
             "What actions are open for person Y?"
             "What decisions have we made about Z?"
Author:      "Write a PRD for initiative X" — agent uses templates + context
Periodic:    /lint — finds dead links, orphan pages, stale data
```

---

## Customization

- Edit `CLAUDE.md` to change agent behavior, add domain-specific glossary terms, or adjust writing style rules.
- Edit templates in `70_Templates/` to match your note-taking style.
- Add new log types to `30_Logs/` if your workflow needs them.
- Add new commands in `.claude/commands/` for your own workflows.

---

## What to drop in the inbox

The agent handles diverse input formats:

- **Meeting transcripts** — from Plaud, Otter, Teams, or manual notes. Speaker labels help but aren't required.
- **Screenshots** — Teams call participant lists, org charts, Slack threads. The agent reads images.
- **Raw notes** — bullet points, free-form text, copy-pasted Slack conversations.
- **Announcements** — reorg emails, role changes, new-hire intros.

The agent classifies and routes each item to the right output structure.

---

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (CLI or VS Code extension)
- [Obsidian](https://obsidian.md) (recommended) or any Markdown editor
- Git (optional, for version history)

---

## License

MIT. Use it however you want.
