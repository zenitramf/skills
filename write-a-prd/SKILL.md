---
name: prd-clickup-obsidian
description: create or update implementation prds that must be anchored to a clickup task and mirrored into obsidian. use when the user wants a prd written, refined, or stored as a clickup-backed project document, especially when the workflow must verify a clickup task id or create a new backlog task before generating an obsidian markdown file with yaml metadata including the clickup_url.
---

# PRD ClickUp Obsidian

## Overview

Create implementation-focused PRDs only after a ClickUp task is confirmed or created. Mirror the final PRD into Obsidian through the Obsidian CLI under `1_Project/<logical-project-name>/<prd-name>-implementation-prd.md` and include the ClickUp task URL as a YAML property named `clickup_url`.

## Required workflow

Follow this sequence in order. Do not skip the ClickUp guard.

1. Understand the problem and proposed solution well enough to write a high-quality PRD.
2. Verify there is a ClickUp task URL to anchor the PRD.
3. Write or update the ClickUp task description with the PRD.
4. Generate the matching Obsidian note with YAML frontmatter containing `clickup_url`.
5. Report back with both artifacts or clearly state what blocked completion.

## ClickUp guard

Never draft the final PRD in Obsidian first.

Before finalizing anything, ensure one of these is true:

- The user gave a ClickUp task ID or ClickUp URL that you can use.
- You successfully created a new ClickUp task in the SOAR Backlog/Idea Sink folder and now have its URL.

If neither is true, stop and ask for the missing ClickUp reference or explain that task creation is required before continuing.

When a task reference is missing, ask directly whether the task already exists and request the ID or URL.

When a task exists:

- Resolve the task reference.
- Overwrite the task description with the new PRD content.
- Preserve the task itself rather than creating a duplicate.

When a task does not exist:

- Create a new task in the SOAR Backlog/Idea Sink folder.
- Use a concise implementation-oriented task title.
- Do not continue until you have the created task URL.

If the current tool environment cannot read or write ClickUp, state that limitation plainly and do not pretend the guard has passed.

## Discovery workflow

Use only the amount of questioning needed to reach a shared understanding.

1. Ask for a detailed description of the problem, current pain, users affected, and any solution ideas if that information is not already clear.
2. Explore the relevant repository or codebase when available to verify assumptions and understand the current implementation.
3. Walk the design tree branch-by-branch until the important decisions are resolved.
4. Identify the modules that will be built or modified.
5. Prefer deep modules: encapsulate substantial behavior behind simple, stable, testable interfaces.
6. Confirm which modules should receive tests.

Do not ask repetitive questions after the needed information is already available.

## Module planning guidance

For each substantial implementation area, capture:

- module responsibility
- public interface or contract
- dependencies and downstream consumers
- whether it should become a deep module
- testing approach based on external behavior

Actively look for opportunities to separate orchestration from reusable domain logic.

## PRD output requirements

Use this structure exactly.

## Problem Statement

The problem that the user is facing, from the user's perspective.

## Solution

The solution to the problem, from the user's perspective.

## User Stories

Provide a long, numbered list of user stories in this format:

1. As an <actor>, I want a <feature>, so that <benefit>

Make the list extensive enough to cover primary flows, edge cases, administration, observability, migration, and failure handling when relevant.

## Implementation Decisions

Include:

- modules that will be built or modified
- interfaces or contracts that will change
- technical clarifications from the developer
- architectural decisions
- schema changes
- api contracts
- specific interactions

Do not include file paths or code snippets.

## Testing Decisions

Include:

- what makes a good test: verify external behavior rather than implementation details
- which modules will be tested
- prior art in the codebase when available

## Out of Scope

Describe what is intentionally excluded.

## Further Notes

Capture remaining context, rollout notes, dependencies, risks, and open questions that should remain visible.

## Obsidian note requirements

After the ClickUp task is confirmed or created, generate an Obsidian markdown file using the Obsidian CLI.

Place it at:

- `1_Project/<logical-project-name>/<prd-name>-implementation-prd.md`

Naming rules:

- Choose a logical project folder name from the feature, product area, or business context.
- Choose a PRD file name that is concise and human-readable.
- Always suffix the file name with `-implementation-prd.md`.
- Prefer kebab-case for generated file and folder names unless the local convention clearly differs.

The note must begin with YAML frontmatter that includes at least:

```yaml
---
clickup_url: <full clickup task url>
---
```

Then place the PRD body beneath the frontmatter.

If the Obsidian CLI is unavailable in the environment, say so explicitly and provide the exact markdown content and intended destination path instead of claiming the note was created.

## Completion checklist

Do not mark the task complete until all applicable items are true:

- problem and solution are understood
- key design decisions are resolved enough for implementation
- module plan is captured
- testing decisions are captured
- clickup task url exists
- clickup task description has been updated or created
- obsidian note has been generated with `clickup_url` yaml

## Response expectations

Be explicit about guard status.

At the end, summarize:

- ClickUp task used or created
- Obsidian path used or intended
- unresolved questions, if any
- any tool limitations that prevented full completion
