---
name: export
description: Generate retrievable artifacts from brainstorm sessions. Use after REFINE phase to create ADRs (technical) or Outlines (conceptual). Spawns parallel architect subagents to generate inline mermaid diagrams.
---

# Export

Transform brainstorm decisions into retrievable, actionable artifacts.

## Flow

1. **Detect track**: Technical → ADR | Conceptual → Outline
2. **Generate document** with natural structure from brainstorm
3. **Identify sections** with component interactions
4. **Spawn parallel architect subagents** (one per section needing diagrams)
5. **Inject mermaid** inline where subagents return diagrams
6. **Add frontmatter** and save to `context/exports/`

## Frontmatter Schema

Minimal. Everything else is discoverable by reading the doc.

```yaml
---
problem: "One-line problem statement"
date: YYYY-MM-DD
---
```

## Technical Track → ADR

Single rich document. Structure flows naturally from brainstorm.

**Required sections:**
- Context (why now, constraints, forces)
- Decision (what was chosen, how it addresses context)
- Consequences (positive, negative, mitigations)

**Include if discussed:**
- Data Model
- Interface
- Error Handling
- Configuration

```markdown
---
problem: "..."
date: YYYY-MM-DD
---

# [Title from brainstorm problem statement]

**Status**: Proposed | Accepted | Superseded by [link]

## Context
[Why, constraints, forces at tension]

## Decision
[What was chosen, how it addresses context]

[Mermaid diagram if architecture discussed]

## Consequences
**Positive:** [Specific gains]
**Negative:** [Trade-offs accepted]
**Mitigations:** [How negatives will be managed]

## Data Model
[If discussed - tables, fields, types]

## Interface
[If discussed - endpoints, methods, params]
```

## Conceptual Track → Outline

**Required:**
- Audience (who, what they know, what they care about)
- Objective (think/feel/do after)

Structure flows naturally from brainstorm content.

```markdown
---
problem: "..."
date: YYYY-MM-DD
---

# [Title]

**Audience**: [Who, background, concerns]
**Objective**: [Desired outcome]

[Sections flow from brainstorm - no rigid template]
```

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

**Location**: `context/exports/`

**Naming**: `[descriptive-problem]-adr-YYYY-MM-DD.md` or `[descriptive-problem]-outline-YYYY-MM-DD.md`

Single file per session.
