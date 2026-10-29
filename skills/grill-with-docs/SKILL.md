---
name: grill-with-docs
description: A relentless interview to sharpen a plan or design, which also creates docs (ADR's and glossary) as we go. Trigger when the user asks for "/grill-with-docs" or wants a design grilling session that updates GLOSSARY.md and ADRs inline.
---

Read and execute both [`../grilling/SKILL.md`](../grilling/SKILL.md) ("grilling") and [`../domain-modeling/SKILL.md`](../domain-modeling/SKILL.md) ("domain-modeling") simultaneously:

1. **Grilling (`../grilling/SKILL.md`):** Map the plan/design as a **design tree** and work the **frontier** in rounds using the `❓ **Q1** ... ➡️ <recommended answer>` format, wording each question so "yes" accepts your recommendation, and looking up environment/codebase facts yourself rather than asking the user.
2. **Domain Modeling (`../domain-modeling/SKILL.md`):** During each round, challenge fuzzy or conflicting terms against `GLOSSARY.md`, cross-reference claims with code, update `GLOSSARY.md` inline as terms crystallize (using [`../domain-modeling/GLOSSARY-FORMAT.md`](../domain-modeling/GLOSSARY-FORMAT.md)), and offer ADRs in `docs/adr/` when all 3 gates hold: Hard to reverse, Surprising without context, and Result of a real trade-off (using [`../domain-modeling/ADR-FORMAT.md`](../domain-modeling/ADR-FORMAT.md)).
