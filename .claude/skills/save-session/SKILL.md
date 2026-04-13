---
name: save-session
description: Save the current Claude conversation as an Obsidian-compatible markdown session note in the docs vault.
argument-hint: "[theme] [slug]"
allowed-tools: Read, Write, Edit, Bash(mkdir *), Glob, AskUserQuestion
# base-path: (not configured yet — will be set on first run)
---

# Save Session

Save the current conversation as a session note in an Obsidian vault.

## Configuration (First Run)

Before doing anything else, check the frontmatter of **this file** for `base-path`.

- If `base-path` is **commented out, missing, or empty**, ask the user:
  > "Where is your Obsidian docs vault? Please provide the absolute path (e.g., `/Users/you/Documents/docs`)."
- Once the user provides the path, **update this SKILL.md file's frontmatter** to set `base-path: <their-path>` (uncommented) so it persists for future runs.
- Use `$BASE` to refer to this path in all steps below.

## File Path Convention

`$BASE/sessions/<theme-name>/YY-MM-DD-<slug>.md`

- **Theme**: A short kebab-case name for the project area (e.g., `auth-system`, `data-pipeline`, `ui-refactor`)
- **Slug**: A short kebab-case description of what was done (e.g., `setup-jwt-middleware`, `fix-token-refresh`)

## Arguments

- If both `$0` (theme) and `$1` (slug) are provided, use them directly
- If only `$0` is provided, use it as the theme and infer the slug from the conversation
- If no arguments are provided, infer both from the conversation and **ask the user to confirm before saving**

## Steps

1. **Resolve `$BASE`** — read this SKILL.md's frontmatter for `base-path`. If not configured, ask the user and save it (see Configuration above)
2. **Determine theme and slug** from arguments or conversation context
3. **Confirm with the user** if theme/slug were inferred (skip if both were passed as arguments)
4. **Create the theme directory** if it doesn't exist: `mkdir -p $BASE/sessions/<theme-name>`
5. **Read the template** at `$BASE/templates/session.md` and use it as the structure for the session file
6. **Create the session file** at `$BASE/sessions/<theme-name>/YY-MM-DD-<slug>.md` using the template, replacing all `{{placeholders}}` with actual values
7. **Fill in all sections** based on the conversation:
   - **Goal**: What the user wanted to accomplish
   - **Summary**: Brief overview of what was done
   - **Key Decisions**: Important choices and reasoning
   - **Code Changes**: Table of files modified and what changed
   - **Open Items**: Only unresolved discussion points or questions that need follow-up. Do NOT add generic items like "verify the changes", "test end-to-end", or "confirm feature flag is registered". If there are no unresolved items, leave it empty.
   - **Notes**: Context that would help if this file is loaded in a future Claude session
   - **Related Sessions**: Auto-populated (see step 8)

8. **Link related sessions**:
   - List existing files in `$BASE/sessions/<theme-name>/` to find sessions under the same theme
   - Add wikilinks to all found sessions in the **Related Sessions** section of the new note
   - Also populate the `related` frontmatter array with the same links
   - If no existing sessions share the theme, leave both empty

9. **Update the index** at `$BASE/index.md`:
   - Add a new row to the table before the comment lines
   - Format: `| YY-MM-DD | ticket | theme | [[sessions/theme/YY-MM-DD-slug]] | brief description |`

## Rules

- Keep summaries concise but include enough context to be useful when loaded in a future session
- Use Obsidian-compatible markdown (wikilinks, YAML frontmatter, tags)
- The notes section is critical — write it as if you're briefing a future Claude session that has no context. Only include information directly relevant to the session's theme, not unrelated work done in the same conversation.
- Always use the actual current date, not a placeholder
- If a ticket/branch name is identifiable from the conversation (e.g., DOMO-479299), include it in the `ticket` frontmatter field. If not, leave it empty.
