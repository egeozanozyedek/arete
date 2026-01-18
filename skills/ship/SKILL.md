---
name: ship
description: Generate retrievable artifacts from brainstorm sessions. Use after STRESS phase to create ADRs + Plans (technical) or Outlines (conceptual). Spawns parallel architect subagents to generate inline mermaid diagrams.
---

# Ship

Transform brainstorm decisions into retrievable, actionable artifacts.

## Flow

1. **Detect track**: Technical → ADR + Plan | Conceptual → Outline
2. **Generate documents** with natural structure from brainstorm
3. **Identify sections** with component interactions
4. **Spawn parallel architect subagents** (one per section needing diagrams)
5. **Inject mermaid** inline where subagents return diagrams
6. **Add frontmatter** and save:
   - ADRs/Outlines → `context/exports/`
   - Plans → `context/plans/`

## Frontmatter Schema

Minimal. Everything else is discoverable by reading the doc.

```yaml
---
problem: "One-line problem statement"
date: YYYY-MM-DD
---
```

## Technical Track → ADR + Plan

Two complementary documents: ADR captures the decision, Plan captures the implementation.

### ADR (Architecture Decision Record)

The **why** and **what** of the decision. Saved to `context/exports/`.

**Template:** See [`reference/ADR.md`](reference/ADR.md) for the full ADR template with required sections and naming conventions.

### Plan (Implementation Plan)

The **how** of the decision. Saved to `context/plans/`.

**Template:** See [`reference/Plan.md`](reference/Plan.md) for the full Plan template with required and optional sections.

## Conceptual Track → Outline

For non-technical brainstorm sessions.

**Template:** See [`reference/Outline.md`](reference/Outline.md) for the full Outline template with required sections and naming conventions.

## Diagram Generation

For sections describing **component interactions**, spawn the architect subagent.

**Triggers** (keywords indicating architecture):
- Components: "service", "database", "API", "cache", "queue", "client"
- Interactions: "calls", "sends", "receives", "queries", "connects", "flows"

**Process:**
1. Identify all sections with component interactions
2. Spawn parallel architect subagents (one per section)
3. Each subagent receives: section title + section text
4. Each subagent returns: mermaid block or `NO_DIAGRAM`
5. Inject returned mermaid inline after section content

**Cap:** Maximum 3 diagrams per document to avoid over-diagramming.

## File Output

### Technical Track
**ADR Location**: `context/exports/`
**Plan Location**: `context/plans/`

**Naming**:
- ADR: `[problem-slug]-adr-YYYY-MM-DD.md`
- Plan: `[problem-slug]-plan-YYYY-MM-DD.md`

Both files cross-reference each other via frontmatter (`plan:` and `adr:` fields).

### Conceptual Track
**Location**: `context/exports/`

**Naming**: `[problem-slug]-outline-YYYY-MM-DD.md`
