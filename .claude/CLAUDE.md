# CLAUDE.md

Guidelines for Claude when interacting with this Obsidian vault.

---

## Purpose
Personal research knowledge base for:
- paper reading
- atomic research ideas
- project planning and tracking
- graph-based idea discovery for new research ideas

---

## Paper Access Rules

**When searching for papers online:**
- **ALWAYS** try to find the arXiv HTML version of the paper
- **DO NOT** download PDFs
- Use the arXiv HTML interface for reading and extracting information
- arXiv HTML URLs typically follow the pattern: `https://arxiv.org/html/[paper-id]`
- Only use PDFs as a last resort if no other option is available

---

## Vault Structure

Flat, link-based system. No nested folders.
```
Projects/   → atomic research ideas
Reading/    → paper notes
MOCs/       → research-area hubs
Templates/  → templates
```

Structure emerges from `[[wiki links]]`, not folders.

---

## Knowledge Flow (CRITICAL)
```
Paper → Project → MOC
```

- papers = evidence  
- projects = ideas  
- MOCs = research areas  

Papers must never link directly to MOCs.

---

## Note Types

### Paper Notes (`Reading/`)
Purpose: understanding only.
- summarize and interpret papers
- no idea ownership
- no area structure

Key sections:
- TLDR (One Sentence)
- Background
- Problem Statement
- Key Idea
- High-Level Approach
- Technical Details / Analysis
- Main Results / Observations
- Interpretation
- Limitations / Open Questions
- Connections to Other Work
- Relevance for My Research

Rules:
- link to projects (in "Linked Research Projects")
- link to influential papers (in "Connections to Other Work") using exact, full paper titles
- link to papers even if they don't exist in the vault yet (they may be added later)
- always use title case for paper titles
- do NOT create projects by default
- reuse existing project links whenever possible
- use `Templates/Paper Note.md` template

---

### Project Notes (`Projects/`)
Purpose: atomic research ideas.
- one idea per note
- actionable, reusable, testable

Key sections:
- Seed Idea
- Current Working Idea
- Research Question
- Hypothesis / Claim
- Context & Motivation
- Possible Approaches
- Research Ideas & Variants
- Related Work
- Relationship to Prior Work
- Possible Experiments / Analyses
- Open Questions & Risks
- Next Actions
- Relevant Papers

Rules:
- do NOT create projects by default
- reuse existing project links whenever possible
- use `Templates/Project.md` template
- Projects may evolve, merge, or die.

---

### Exploratory Projects
If a paper does not fit any existing project:
- create a minimal exploratory project
- status: `seed`
- keep it very small
- link it to at least one MOC

No paper should remain unlinked to a project. No orphan paper notes that link to no project.

---

### MOCs (`MOCs/`)
Purpose: organize projects into research areas.
MOCs are not concept notes and not bibliographies.

May contain:
- project links (primary)
- a few canonical or survey papers (optional)
- sub-area MOCs

Must NOT contain:
- exhaustive paper lists
- incremental papers
- summaries or analysis

Paper appears in an MOC only if:
1. foundational / canonical  
2. survey or taxonomy  
3. motivated multiple projects  

If relevant through only one project → do not add paper to MOC.

---

## Linking Rules
- Paper → Project (in "Linked Research Projects")
- Project → Paper (in "Relevant Papers")
- Project → MOC
- Paper → MOC ❌

**Bidirectional linking:**
- Every paper must link to at least one project
- When a paper is linked to a project, the project must reciprocally link back to that paper in its "Relevant Papers" section
- This maintains consistency across the graph

Every project must link to at least one MOC.

---

## Writing Rules
- ultra concise
- bullet points preferred
- fragments OK
- no essays
- clarity > grammar
- >5 bullets → split

---

## Linking Philosophy
- link ideas, not words
- reuse links before creating new ones
- if unsure → do NOT create a project
- create projects only for:
  - reusable mechanisms
  - repeated phenomena
  - testable hypotheses
  - emerging research directions

---

## Metadata

### Paper
```yaml
url: 
code: 
type: paper | blog
authors:
year: yyyy
status: to_read
```

### Project
```yaml
type: project
status: seed | idea | active | writing | finished
```

### MOC
```yaml
type: moc
tags: [moc, index]
```

---

## Prohibited
❌ subfolders  
❌ standalone concept notes  
❌ ontologies  
❌ tag-based semantics  
❌ paper dumping in MOCs  
❌ orphan notes  
❌ essay-style internal writing  
❌ change, add or delete files in `Meetings`
❌ never search or scan all notes or all files

## Additional Rules
✅ First read `.claude/vault_index.md` to identify relevant notes for the current task
✅ Only open notes you explicitly need
✅ Only open notes that are directly linked from the index or which are explicitly requested by the user.
✅ When unsure where to look: Vault_Index → relevant MOC → 1–3 projects
---

## Mental Model
- Papers → evidence  
- Projects → thinking  
- MOCs → structure  
- Links → reasoning  
- Graph → research map
