---
name: to-spec
description: "Turn the current conversation into a spec and Dual-Write it to docs/specs/NNNN-slug.md and GitHub Issues (with label 'ready-for-agent'): no interview, just synthesis of what you've already discussed."
---

This skill takes the current conversation context and codebase understanding and produces a specification. Do NOT interview the user; just synthesize what you already know.

## ARX Dual-Write Tracker Convention (Pre-Configured — Never Ask for `/setup-matt-pocock-skills`)

All ARX repositories use **Dual-Write Spec Storage**:
1. **Primary Git-Tracked Spec:** Save to `docs/specs/NNNN-slug.md` (creating `docs/specs/` lazily and incrementing `NNNN` sequentially: `0001-...`, `0002-...`).
2. **GitHub Issue Mirror (Automatic when GitHub remote exists):** If `git remote get-url origin` points to GitHub, automatically publish the spec via `gh issue create --title "Spec NNNN: <Title>" --body-file docs/specs/NNNN-slug.md --label "ready-for-agent"` (creating the `ready-for-agent` label first if needed).

## Process

1. Explore the repo to understand the current state of the codebase, if you haven't already. Use the project's domain glossary vocabulary (`GLOSSARY.md`) throughout the spec, and respect any ADRs (`docs/adr/`) in the area you're touching.

2. Sketch out the seams at which you're going to test the feature. Existing seams should be preferred to new ones. Use the highest seam possible. If new seams are needed, propose them at the highest point you can. The fewer seams across the codebase, the better - the ideal number is one.

3. Write the spec using the template below to `docs/specs/NNNN-slug.md`, and if a GitHub remote is present, publish it to GitHub Issues with the `ready-for-agent` label.

<spec-template>

## Problem Statement

The problem that the user is facing, from the user's perspective.

## Solution

The solution to the problem, from the user's perspective.

## User Stories

A LONG, numbered list of user stories. Each user story should be in the format of:

1. As an <actor>, I want a <feature>, so that <benefit>

<user-story-example>
1. As a mobile bank customer, I want to see balance on my accounts, so that I can make better informed decisions about my spending
</user-story-example>

This list of user stories should be extremely extensive and cover all aspects of the feature.

## Implementation Decisions

A list of implementation decisions that were made. This can include:

- The modules that will be built/modified
- The interfaces of those modules that will be modified
- Technical clarifications from the developer
- Architectural decisions
- Schema changes
- API contracts
- Specific interactions

Do NOT include specific file paths or code snippets. They may end up being outdated very quickly.

Exception: if a prototype produced a snippet that encodes a decision more precisely than prose can (state machine, reducer, schema, type shape), inline it within the relevant decision and note briefly that it came from a prototype. Trim to the decision-rich parts, not a working demo, just the important bits.

## Testing Decisions

A list of testing decisions that were made. Include:

- A description of what makes a good test (only test external behavior at public seams, not implementation details)
- Which module seams will be tested
- Prior art for the tests (i.e. similar types of tests in the codebase)

## Out of Scope

A description of the things that are out of scope for this spec.

## Further Notes

Any further notes about the feature.

</spec-template>
