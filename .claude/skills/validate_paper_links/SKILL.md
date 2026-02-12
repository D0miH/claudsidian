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

### 0. Read last-run timestamp
Read `.claude/vault_maintanance.md` and note the `validate_paper_links:` timestamp. When validating multiple papers, prioritize papers in `Reading/` that were modified after this timestamp.

### 1. Get paper and note details

Read the paper note and extract:
- **Paper URL:** From frontmatter (arXiv HTML URL preferred)
- **Paper title:** From frontmatter or file name
- **Authors:** From frontmatter (if available)
- **Year:** From frontmatter (if available)

### 2. Fetch references section from arXiv HTML

**Access paper:**
- Use paper URL from frontmatter (typically `https://arxiv.org/html/[paper-id]v[version]`)
- Use WebFetch with prompt focused **only on references section**
- Skip all paper content (abstract, introduction, methods, results, etc.)
- Extract **only** the bibliography/references section

**Extract citation data:**
- Parse reference list to get: titles, authors, years
- Build structured reference list from bibliography section
- Reference section is clearly marked on arXiv HTML pages

**WebFetch optimization:**
- Prompt: "Extract ONLY the bibliography/references section. Ignore all other paper content (abstract, introduction, methods, results). Return reference titles, authors, and years."
- This signals to skip 90% of paper content and focus only on references
- Saves tokens by not parsing paper text

**Why arXiv HTML:**
- ✅ Direct access to paper bibliography
- ✅ Pre-formatted references (easy to extract)
- ✅ No API rate limiting or authentication
- ✅ Reliable and consistent structure across papers
- ✅ No JavaScript rendering issues
- ✅ Can optimize WebFetch to skip content (token savings)

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
- Count how many times the paper is cited in the current paper's text
- Referenced paper with `≥2` citations in this paper = influential ✓
- Referenced paper cited once but foundational (cited in abstract/intro)? = influential ✓
- Referenced paper cited only once in methods/results (low-impact section)? = LOW influence (candidate for removal)
- If influence is borderline → ask user for judgment

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

### 7. Update vault_maintanance.md
After completing validation (regardless of whether fixes were applied):
- Run `date` via Bash to get the current system time
- Read `.claude/vault_maintanance.md`
- Update `validate_paper_links:` line to current date and time in format `DD-MM-YYYY HH:MM`
- Reset `papers_since_validate_links:` to `0`
- Write the updated file

**Example update:**
```
validate_paper_links: 10-02-2026 14:35
```

---

## TOKEN EFFICIENCY

**Optimizations:**
- **arXiv HTML** for direct bibliography access (single WebFetch call)
- **References-only extraction** via targeted WebFetch prompt
- Skip all paper content (abstract, intro, methods, results)
- Bibliography section clearly marked and easy to extract
- No API calls, no rate limiting, no authentication needed
- Direct title + author list from bibliography
- Build simple title→reference mapping

**Why this is efficient:**
- 1 WebFetch call to arXiv HTML (direct paper access via frontmatter URL)
- **Targeted prompt skips 90% of paper content** (saves major tokens)
- Bibliography section is well-formatted and clearly delimited
- No API dependencies or rate limiting concerns
- Minimal parsing needed (just extract reference titles)
- No ambiguity in reference data

**Workflow:**
1. Read paper note frontmatter for arXiv HTML URL
2. WebFetch the arXiv HTML page with prompt: "Extract ONLY bibliography/references section. Skip all other content."
3. Extract bibliography/references section
4. Build reference list from bibliography
5. Validate links against reference list

**Avoid:**
- Semantic Scholar (JavaScript rendering, rate limiting issues)
- PDF parsing
- Regex patterns for extraction (bibliography is already structured)
- Processing full paper text (use targeted prompt instead)

**Expected cost:** ~100-150 tokens per paper (1 optimized WebFetch call + validation)

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

✅ Did I fetch the arXiv HTML page from the frontmatter URL?
✅ Did I extract all references from the bibliography section?
✅ Did I extract reference metadata (title, authors, year)?
✅ Did I check each link against the bibliography references?
✅ Did I assess influence (citation count/context in paper)?
✅ Did I validate Title Case formatting against bibliography titles?
✅ Did I check file name convention (: → -)?
✅ Did I group findings by severity?
✅ Did I get user approval before removing links?
✅ Did I verify all remaining links are valid?
✅ Did I update vault_maintanance.md with current timestamp?
