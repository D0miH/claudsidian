---
name: validate_paper_links
description: Validate paper note links against actual paper references and influence
---

## ✅ Validate Paper Links Skill

Validate all `[[Paper Title]]` links in a paper note against the actual paper's references and citation patterns.

**Goal:** Prevent:
1. Links to papers NOT referenced in the current paper (hallucination)
2. Links to papers with incorrect titles or formatting
3. Links to low-influence papers (cited once, not foundational)
4. Broken file names (`:` not exchanged to `-`)

---

## CRITICAL RULES

**A link should exist only if:**
1. ✅ The paper IS referenced in the current paper's bibliography/references
2. ✅ The paper is INFLUENTIAL (cited ≥2 times OR explicitly foundational/cited in intro/abstract)
3. ✅ Title is in exact Title Case matching the reference
4. ✅ File name properly formats `:` as `-` (e.g., `Attention - Is All You Need`)

**A link should be removed if:**
- ❌ Paper is NOT in the current paper's references
- ❌ Paper is cited only once in methods/results (low influence)
- ❌ Title formatting is incorrect
- ❌ File name violates naming convention

---

## WORKFLOW

### 1. Get paper and note details

Ask user:
- **Paper note file name:** (e.g., `Sparse Autoencoders Find Highly Interpretable Features in Language Models`)
- **Paper URL:** (arXiv HTML URL preferred: `https://arxiv.org/html/[paper-id]`)
  - If user provides arXiv ID, construct URL: `https://arxiv.org/html/[id]`

### 2. Fetch paper and extract references

**Access paper:**
- Open paper URL using WebFetch
- Extract all citations/references from the paper
- Build reference list: paper titles + authors + citation counts

**Extract citation data:**
- Reference list (title, authors, year)
- Citation frequency (how many times cited in text)
- Citation context (intro, abstract, methods, results, related work, etc.)

### 3. Read paper note

- Open paper note from `Reading/[filename].md`
- Extract all `[[Paper Title]]` links from "🔗 Connections to Other Work" section
- Build link list with current formatting

### 4. Validate each link

**For each `[[Paper Title]]` in the note:**

**Check 1: Reference exists?**
- Does this paper appear in the current paper's bibliography?
- If NO → FLAG: "Not referenced in paper" (candidate for removal)
- If YES → PASS

**Check 2: Influence level**
- Count citations in text: `≥2` = influential
- Is it foundational (cited in abstract/intro)? = influential
- Single citation in methods/results? = LOW influence (candidate for removal)
- If influence unclear → ASK user

**Check 3: Title formatting**
- Compare link title against reference list title
- Is it Title Case?
- Are colons `:` present in reference but `-` in link?
- Are special characters consistent?
- If mismatch → FLAG: "Title mismatch" or "Formatting error"

**Check 4: File name validity**
- Expected file name: exact title with `:` → `-`
- Is current link pointing to correct file name?
- Example: `[[Attention - Is All You Need]]` should match file `Attention - Is All You Need.md`
- If mismatch → FLAG: "File name doesn't match link"

### 5. Generate validation report

Group findings by severity:

**🔴 CRITICAL (remove immediately):**
- Papers not in references
- Papers cited only once in low-importance sections

**🟡 NEEDS FIXING:**
- Title case errors
- File name formatting errors (`:` not converted to `-`)

**🟢 OK:**
- Papers properly referenced
- Properly formatted
- Appropriately influential

**❓ NEEDS USER JUDGMENT:**
- Papers cited ~2 times (borderline influence)
- Foundational papers cited rarely but in key sections

### 6. Apply fixes

With user approval:
- **Remove links:** Delete `[[Paper Title]]` lines from "🔗 Connections to Other Work"
- **Fix titles:** Correct Title Case and formatting to match references
- **Fix file names:** Ensure `:` → `-` conversion
- **Update note:** Save corrected paper note
- **Verify:** Re-scan to confirm all links now valid

---

## TOKEN EFFICIENCY

**Optimizations:**
- WebFetch single paper + extract citations (efficient)
- Use grep on reference section to count mentions: `grep -i "paper_title"`
- Skip deep content analysis—only scan references section
- Build simple title→count mapping

**Avoid:**
- Reading full paper content (references section only)
- Fuzzy title matching (use exact references)
- Checking every single word (only validate links)

**Expected cost:** ~300-500 tokens per paper (WebFetch + validation)

---

## OUTPUT RULES

- Grouped by severity (CRITICAL → needs fixing → OK)
- File name / Link text / Issue / Suggested fix
- No prose, bullets only
- Example:

```
🔴 CRITICAL (remove):
- [[Paper Not In References]] - Not in bibliography → REMOVE

🟡 NEEDS FIXING:
- [[attention is all you need]] - Title case error → Fix to [[Attention Is All You Need]]
- [[My Paper: A Study]] - File name formatting → Should be [[My Paper - A Study]]

🟢 OK (keep):
- [[Transformers are All You Need]] ✓
```

---

## SELF-CHECK

✅ Did I fetch the actual paper from the web?
✅ Did I extract all references from the paper?
✅ Did I check each link against references?
✅ Did I assess influence (citation count + context)?
✅ Did I validate Title Case formatting?
✅ Did I check file name convention (: → -)?
✅ Did I group findings by severity?
✅ Did I get user approval before removing links?
✅ Did I verify all remaining links are valid?
