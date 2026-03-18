---
name: prd-to-issues
description: break a product requirements document into independently grabbable clickup subtasks using tracer-bullet vertical slices. use when the user wants to convert a prd into implementation tickets, create clickup subtasks from a prd, break down a prd into work items, or plan vertical-slice delivery with hitl versus afk classification and dependency ordering.
---

# PRD to Issues

Convert an implementation PRD into independently grabbable ClickUp subtasks using thin vertical slices.

## Workflow

Follow this sequence:

1. Locate and read the parent PRD
2. Explore the codebase or surrounding context when that would improve the breakdown
3. Draft thin vertical slices
4. Review the proposed slices with the user and iterate
5. Create the ClickUp subtasks in dependency order

## 1. Locate the parent PRD

Ask the user for the PRD ClickUp task number or URL.

If the PRD content is not already available in the conversation, fetch it using the user's ClickUp tooling, including comments when available.

Always ground the breakdown in the actual PRD content. Do not infer missing requirements if the PRD is incomplete. Instead, call out ambiguity and keep the affected slices as HITL if needed.

## 2. Explore implementation context when useful

If the existing codebase, architecture, or surrounding systems matter for the breakdown, inspect them before finalizing slices.

Use this context to:

- avoid slices that conflict with current architecture
- identify natural seams for deep, testable modules
- recognize hidden dependencies that the PRD did not spell out
- keep each slice end-to-end rather than layer-local

Skip deep exploration when the PRD is self-contained and the user only wants a first-pass task breakdown.

## 3. Draft tracer-bullet vertical slices

Break the PRD into thin vertical slices.

Each slice must be a narrow but complete path through the relevant layers. Prefer slices that are independently demoable, testable, or otherwise verifiable.

### Vertical slice rules

- each slice must cut through the full behavior, not just one technical layer
- each completed slice must be demoable or verifiable on its own
- prefer many thin slices over a few broad slices
- avoid separate "backend only", "frontend only", or "schema only" subtasks unless the work is truly a standalone slice
- when uncertain, split further until a single engineer could comfortably grab the work without owning a huge ambiguous ticket

### HITL vs AFK

Label every slice as one of:

- **HITL**: requires human interaction, decision, approval, review, or other synchronous input
- **AFK**: can be implemented, tested, and merged without additional human interaction

Prefer AFK over HITL whenever the work can be framed that way honestly.

### Dependency rules

Track blockers between slices. Keep the graph as simple as possible.

- only add a blocker when a slice truly cannot proceed first
- prefer parallelizable slices when feasible
- create foundational blockers first so downstream subtasks can reference real ClickUp task numbers later

## 4. Present the proposed breakdown for review

Present the draft as a numbered list.

For each slice, include exactly these fields:

- **Title**
- **Type**: HITL or AFK
- **Blocked by**
- **User stories covered**

Then explicitly ask the user to review:

- whether the granularity is right
- whether dependencies are correct
- whether any slices should be split or merged
- whether the HITL and AFK labels are correct

Iterate until the user approves the breakdown.

Do not create ClickUp subtasks before the user approves, unless the user clearly instructs you to skip review and proceed directly.

## 5. Create ClickUp subtasks

After approval, create the subtasks in dependency order so blocker references can use real task numbers.

Do not close, overwrite, or otherwise modify the parent PRD task.

Use this template for every subtask:

```markdown
## Parent PRD

#<prd-issue-number>

## What to build

A concise description of this vertical slice. Describe the end-to-end behavior, not layer-by-layer implementation. Reference specific sections of the parent PRD rather than duplicating content.

## Acceptance criteria

Use clear Gherkin Acceptance Criteria.
Ensure all Gherkin acceptance criteria are fully Cucumber-compliant and compatible with .feature files.

```gherkin
Feature: [feature name]
  Scenario: [scenario name]
    Given ...
    When ...
    Then ...
```

## Blocked by

- Blocked by #<issue-number>

Or `None - can start immediately` if there are no blockers.

## User stories addressed

Reference by number from the parent PRD:

- User story 3
- User story 7

```

## Quality bar for subtasks

Each created subtask should:
- describe observable end-to-end behavior
- be independently grabbable by an engineer
- have clear acceptance criteria
- reference the parent PRD instead of copying it
- identify real blockers only when necessary
- map back to explicit user stories from the PRD

## Handling ambiguity

If the PRD is missing needed detail:
- surface the ambiguity clearly
- propose the smallest reasonable slice set
- mark decision-heavy work as HITL
- ask for approval before creating subtasks

## Output style

Keep the review breakdown concise but concrete.

When creating tasks, keep the task bodies structured and consistent. Use exact user story references from the parent PRD whenever possible.
