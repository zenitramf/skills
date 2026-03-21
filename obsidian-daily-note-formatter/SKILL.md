---
name: obsidian-daily-note-formatter
description: format rough obsidian daily notes into clean markdown and, when possible, fetch the daily note with obsidian cli for today or a user-specified date before rewriting it in place. use when the user has rapid journal notes, fragmented bullets, sections separated by divider lines, task or todo markers, or shorthand topic and person headings that need to be converted into structured obsidian notes with h1 headings, subheadings, wikilinks, and tasks-plugin compliant checkboxes due the next day.
---

Format rough daily notes into polished Obsidian markdown.

## Workflow

1. Determine the source note.
   - Default to today's daily note.
   - If the user specifies a date, use that date instead.
   - If Obsidian CLI access is available in the environment, get the note contents from the daily note before transforming.
   - If the note text is already provided directly in chat, use that text.

2. Parse logical boundaries.
   - Treat `---` and `<hr>` as hard boundaries between separate note ideas.
   - Treat lines ending in `>` as a strong signal that a heading is being introduced.
   - Treat `/` as a sub-context delimiter when it obviously separates a person or discussion context.
   - If the input is inconsistent or free-form, make the best sensible structural guess.

3. Rewrite the note.
   - Replace the original content with the cleaned markdown.
   - Do not preserve the raw input unless the user explicitly asks.
   - Do not add a document title above the note.
   - Use only content headings that belong to the note itself.

4. If the note came from Obsidian CLI, write the transformed content back so the original note is replaced.

## Output rules

### Heading rules
- Every top-level note section must start with `# `.
- Main headings must use this exact pattern whenever a topic can be inferred:
  - `# Topic | Person | Heading`
  - `# Topic | Heading`
- Valid topic labels are only:
  - `Idea`
  - `Conversation`
  - `Task`
  - `Todo`
  - `Meeting`
  - `Note`
  - `Reminder`
- Do not add a separate title heading for the whole document.
- Use `##` and deeper headings only for true substructure inside a top-level section.

### Person field rules
- Use the person field when a specific person is central and obvious from the input.
- Normalize obvious person names into title case.
- When a heading is about a person-specific discussion, prefer a descriptive heading such as:
  - `# Conversation | Hunny | Discussion with Hunny`
  - `# Task | Luis | Check in with Luis`
- If no person is central, omit the person slot and use `# Topic | Heading`.

### Topic classification rules
- `Idea`: concepts, brainstorms, things to explore, plans, or inspirations.
- `Conversation`: discussions, things talked about with someone, or discussion notes.
- `Task`: action items that should become tasks.
- `Todo`: lightweight action items that should become tasks.
- `Meeting`: meeting notes or agenda-like content.
- `Note`: informational observations, journaling, devotionals, logs, food notes, or general notes that are not tasks.
- `Reminder`: things to remember that should not become tasks; add `#remind-me` in the section body.

## Task conversion rules
- If the source marker is `task>` or `todo>`, convert the actionable items into Obsidian Tasks plugin checkboxes.
- Use this format:
  - `- [ ] Task text 📅 YYYY-MM-DD`
- The due date must be the next day relative to the note date being processed.
- If the note date is unknown, use the next day relative to today.
- Keep supporting bullets under the task section when useful.
- If a task section contains one sentence that is clearly a single action, convert it into one checkbox item.
- If there are several action bullets, convert each action into its own checkbox item.
- Do not tag task sections with `#remind-me`.

## Reminder rules
- For reminder content that is explicitly not a task, classify it under `Reminder`.
- Include `#remind-me` in the body of that section.
- Do not convert reminders into checkboxes unless the input clearly says `task>` or `todo>` or is unmistakably an action item.

## Delimiter and structure rules
- `topic text>` usually signals the start of a new section.
- `topic / person` or `topic>person/` usually implies a sub-context.
- Example:
  - Input: `idea get info on website>discuss with Ryan/`
  - Output:
    - `# Idea | Website Info`
    - `## Ryan | Discussion with Ryan`
- If a person-specific sub-context is better represented as a full top-level section, prefer clarity over rigid parsing.

## WikiLink rules
- Add Obsidian WikiLinks only when the related word is obvious and likely to represent a reusable note target.
- Good candidates:
  - People names such as `[[Hunny]]`, `[[Luis]]`, `[[Ryan]]`
  - Named projects, recurring places, repeated internal concepts
  - Clearly referenced books, notes, or entities that are likely to deserve their own page
- Avoid over-linking common words.
- Do not WikiLink every noun.
- Use natural judgment and keep links sparse and helpful.

## Style rules
- Clean up spelling, capitalization, and punctuation.
- Preserve the user's meaning.
- Keep bullets concise.
- Group related bullets beneath the most sensible heading.
- When scripture or devotional observations appear, keep them as notes unless they clearly become tasks or reminders.
- Food logs, quick logs, and factual observations usually belong under `Note`.

## Default formatting behavior
- Prefer one top-level section per logical idea chunk.
- Under a top-level section, use bullets for supporting details.
- Convert shorthand into readable phrasing.
- Make best-guess organizational choices when delimiters are missing or messy.

## Examples

### Example 1
Input:
```text
convo talk with hunny>
- point 1
- point 2
```

Output:
```markdown
# Conversation | Hunny | Discussion with Hunny
- [[Hunny]]
- Point 1
- Point 2
```

### Example 2
Input:
```text
idea get info on website>discuss with Ryan/
- point 1
- point 2
```

Output:
```markdown
# Idea | Website Info
## Ryan | Discussion with Ryan
- [[Ryan]]
- Point 1
- Point 2
```

### Example 3
Input:
```text
task ticktick>
- reach out to luis, to see how hes doing.
```

Assume note date is 2026-03-21.

Output:
```markdown
# Task | Luis | Check in with Luis
- [ ] Reach out to [[Luis]] to see how he's doing. 📅 2026-03-22
```

### Example 4
Input:
```text
reminder call bank about card maybe later
```

Output:
```markdown
# Reminder | Bank Card Follow-up
#remind-me
- Call the bank about the card later.
```

## Final check before returning
- Every top-level section starts with `# `.
- No document-level title was added.
- Main headings use `Topic | Person | Heading` or `Topic | Heading`.
- Tasks from `task>` or `todo>` are in Tasks plugin format with next-day due dates.
- Reminders include `#remind-me` and are not converted to tasks unless clearly actionable task input.
- Obvious related entities are WikiLinked, but links are not overused.
- The transformed note fully replaces the original content.
