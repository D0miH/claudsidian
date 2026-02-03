---
name: audit_links
description: Validate vault graph consistency and structural integrity
---

## 🔍 Audit Links Skill

Fast audit of vault structure to catch:
- Orphan papers (not linked to any project)
- Orphan projects (not linked to any MOC)
- Broken bidirectional links (paper→project without reciprocal project→paper)
- Forbidden concept links (e.g., `[[Attention Mechanism]]`)
- Papers linking directly to MOCs (forbidden)
- Malformed wiki links

---

## CRITICAL RULES

**Vault constraints (non-negotiable):**
1. Every paper in `Reading/` must link to ≥1 project in `Projects/`
2. Every project in `Projects/` must link to ≥1 MOC in `MOCs/`
3. Bidirectional links: If paper→project exists, project→paper must exist
4. Papers **never** link directly to MOCs
5. Papers link **only** to projects and other papers
6. Concept/method/technique links forbidden (e.g., `[[Transformer]]`, `[[Attention Mechanism]]`)

---

## WORKFLOW

### 1. Get audit scope
Ask user:
- **Full audit?** (scan entire vault)
- **Targeted audit?** (specific project, paper, or MOC)
- **Quick scan?** (only orphans + broken bidirectional links)

### 2. Run lightweight scan

**For papers in `Reading/`:**
- Extract all `[[wiki links]]` from each paper
- Verify each link points to a project in `Projects/` or paper in `Reading/`
- Flag: Papers with zero project links (orphan papers)
- Flag: Papers linking directly to `MOCs/` files (forbidden)
- Flag: Concept links like `[[Attention]]`, `[[Transformers]]` (forbidden)

**For projects in `Projects/`:**
- Extract all `[[wiki links]]` from each project
- Verify each links to MOC or paper
- Flag: Projects with zero MOC links (orphan projects)

**For bidirectional consistency:**
- For each paper→project link found: verify reciprocal project→paper link exists
- Flag: Missing reciprocal links

### 3. Compile report
Group findings by violation type:
1. **Orphan papers** (not linked to any project)
2. **Orphan projects** (not linked to any MOC)
3. **Broken bidirectional links** (paper→project without project→paper)
4. **Forbidden concept links** (in papers)
5. **Forbidden paper→MOC links**
6. **Malformed links** (broken wiki syntax)

Include:
- File name
- Problematic link/content
- Suggested fix

### 4. Present findings
- Show violations by category
- Prioritize by severity (orphans > broken bidirectional > concept links)
- Offer fixes: "Should I remove this link?" / "Should I add reciprocal link?"

### 5. Apply fixes
Only with user approval:
- Remove orphan papers/projects (ask first)
- Fix broken bidirectional links
- Remove forbidden concept links
- Remove paper→MOC links

---

## TOKEN EFFICIENCY

**Optimizations:**
- Use grep patterns to find links fast: `\[\[.*\]\]`
- Don't read full file contents unless necessary
- Process files by type (papers, projects, MOCs)
- Early exit on first issue per file
- Report summaries, not line-by-line details

**Avoid:**
- Reading every file in full (extract links only)
- Deep analysis of content (only validate structure)
- Redundant checks across multiple passes

---

## OUTPUT RULES

- Concise summary first
- Group violations by type
- Each violation: file name + issue + suggested fix
- No prose, bullets only
- Fast scan report (~100-200 tokens)

---

## SELF-CHECK

✅ Did I scan all papers for orphans?
✅ Did I check all projects for MOC links?
✅ Did I verify bidirectional consistency?
✅ Did I identify concept links in papers?
✅ Did I find paper→MOC direct links?
✅ Report grouped by violation type?
✅ Did I get user approval before fixing?
