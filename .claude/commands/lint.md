# Wiki Lint

Health-check the entire vault. Run periodically to keep the knowledge graph clean and compounding.

## What to check

Work through each category below. Read the relevant files as needed. Produce a structured report first, then ask which fixes to apply before touching any file.

### 1. Orphan pages
Pages in 40_Wiki/ or 20_People/ with no inbound wikilinks from any other file. These are disconnected from the graph and will never be discovered naturally.

### 2. Broken wikilinks
Wikilinks that point to a file that does not exist and has no stub. Check all files in 10_Meetings/, 20_People/, 40_Wiki/, 30_Logs/, 80_Deliverables/.

### 3. Naming violations
Any use of partial names (e.g. `[[Alex]]` instead of `[[Alex Chen]]`) instead of full canonical names. Check 50_System/entity-registry.md for the canonical form of every name.

### 4. Stale or contradictory claims
Facts that appear in multiple files with conflicting values. E.g. a person's role, reporting line, or team described differently in two places. Surface both versions with file references.

### 5. Missing pages for mentioned concepts
Entities or concepts mentioned repeatedly in meeting notes or wiki pages that do not yet have their own page in 40_Wiki/. Flag as candidates. Do not create automatically.

### 6. Missing cross-references
Pages that discuss the same entity but don't link to each other. E.g. a meeting note mentions a product but lacks the wikilink, or a person page should link to a wiki entity page but doesn't.

### 7. Index gaps
Entries in 40_Wiki/ that are not listed in 40_Wiki/index.md.

### 8. Open questions that may now be answered
Scan 30_Logs/Open Questions.md. For each open item, check whether the answer has since appeared in any meeting note or wiki page. If so, flag it for resolution.

### 9. Data gaps
Important facts clearly missing but fillable. E.g. a person stub with no role, a team page with no members listed.

## Output format

Produce a report with one section per category. For each issue, include:
- The file(s) involved (with path)
- A one-line description of the problem
- A suggested fix

Example:
```
## Orphan pages
- 40_Wiki/Platform Engineering.md — no inbound links found. Suggest linking from person pages of team members.

## Broken wikilinks
- 10_Meetings/2026-03-15 Meeting with Alex Chen.md → [[API Migration]] — no page exists. Suggest creating stub in 40_Wiki/.
```

After the report, ask: "Which of these should I fix now?"

## After fixes are approved

For each approved fix:
- Apply the change
- Append an entry to 40_Wiki/log.md:
  `## [YYYY-MM-DD] lint | [short description of what was fixed]`
- Update 50_System/entity-registry.md if canonical names changed
- Update 40_Wiki/index.md if new pages were created or removed
