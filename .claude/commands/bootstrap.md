# Bootstrap Vault

Set up this vault for a new user. Run once when the vault is fresh.

## Steps

1. **Ask the user:**
   - Their full name and role/title
   - Their team name and manager's name
   - 5-10 key people they work with (name + role + relationship)
   - 3-5 key products, systems, or projects in their domain
   - Any acronyms or glossary terms specific to their org

2. **Generate:**
   - `50_System/entity-registry.md` populated with all named entities
   - Person pages in `20_People/` for the user and each person named (use `70_Templates/Person Note Template.md`)
   - Wiki stub pages in `40_Wiki/` for each product/system/team
   - `40_Wiki/Acronyms and Glossary.md` with their domain terms
   - Update `40_Wiki/index.md` with all new pages

3. **Update CLAUDE.md:**
   - Replace the generic "Who you are" section with the user's actual name, role, team, and reporting line
   - This personalizes all future agent behavior

4. **Confirm** with the user that everything looks correct.

## After bootstrap

Tell the user:
- Drop raw notes, transcripts, or screenshots into `00_Inbox/`
- Run `/ingest` to process them into structured knowledge
- Run `/lint` periodically to keep the vault healthy
