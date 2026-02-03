# CLAUDEsidian
AI-enhanced Obsidian research vault for academic paper reading and project management.

---

## 📁 What's in This Vault?

This vault integrates Claude AI with Obsidian for academic research. Key files:

- **`README.md`** — This file; setup and usage guide (project root)
- **`.claude/CLAUDE.md`** — Core instructions for Claude on how to work with this vault (structure, linking rules, knowledge flow)
- **`.claude/settings.local.json`** — Permission settings for Claude's terminal and web access
- **`.claude/vault_index.md`** — Quick reference of active MOCs and projects in the vault
- **`.claude/skills/read_paper/SKILL.md`** — Automated paper-to-notes workflow (converts papers into structured Obsidian notes)

---

## 🎯 Purpose

Transform academic research workflows:
- **Read papers** → Claude extracts structure, ideas, and connections
- **Generate projects** → Each paper spawns atomic research ideas
- **Build knowledge graph** → Links emerge through `[[wiki linking]]`, not folders
- **Discover research directions** → MOCs synthesize areas from connected projects

Core philosophy: **Papers → Projects → MOCs**

---

## Vault Structure

Your vault needs these core folders:

```
vault/
├── README.md         # This file
├── .claude/          # Claude configuration
├── Projects/         # Atomic research ideas
├── Reading/          # Paper notes
├── MOCs/            # Maps of Content (research area hubs)
├── Templates/       # Note templates
```

**Required folders:**
- **`Projects/`** — One file per research idea, extracted from papers
- **`Reading/`** — One file per paper, structured notes
- **MOCs/`** — Research area overviews, linking related projects
- **`Templates/`** — Contains `Paper Note.md` and `Project.md` templates

The structure is intentionally flat. Deep nesting is discouraged—links create the structure, not folders.

---

## 🛠️ Setup

### Prerequisites
- [Obsidian](https://obsidian.md/) installed
- [Claude Code](https://claude.ai/download) desktop app
- Claude Pro or API access

### Installation

1. **Open this vault in Obsidian:**
   ```
   Open folder as vault → Select "Uni" folder
   ```

2. **Open vault folder in Claude Code:**
   ```
   File → Open Folder → Select your folder
   ```
   Or drag and drop the "Uni" folder into Claude Code

3. **Verify setup:**
   - The `.claude` folder should be visible in the file tree
   - Claude automatically reads `CLAUDE.md` as custom instructions
   - Test by asking: "What's the structure of this vault?"

---

## 💡 How to Use

### Reading Papers with Claude

1. **Start a conversation** in Claude Code
2. **Ask Claude to read a paper:**
   ```
   Read paper: [paper title or arXiv URL]
   ```
   or 
   ```
   /read_paper [arXiv URL]
   ```
3. **Claude will:**
   - Fetch the paper (preferring arXiv HTML)
   - Create a structured note in `Reading/`
   - Generate 1-3 project ideas in `Projects/`
   - Update links and connections

### Creating Research Projects

Projects are **atomic research ideas** extracted from papers:
```
Ask Claude: "Create a project about [your idea or paper]"
```

Claude follows the template in `Templates/Project.md`

### Managing MOCs (Maps of Content)

MOCs synthesize research areas:
```
Ask Claude: "Update the Alignment & Safety MOC with recent projects"
```

### Daily Workflow

1. Read papers through Claude
2. Review generated project notes
3. Refine and expand ideas
4. Let MOCs emerge organically from links

---

## 📖 Key Concepts

- **Flat structure:** No deep folders; links create structure
- **Atomic notes:** One idea per project note
- **Knowledge flow:** Papers feed projects, projects cluster into MOCs
- **No direct paper→MOC links:** Ideas must pass through projects first
- **Wiki linking:** Aggressive `[[linking]]` to papers and projects only

---

## 🔧 Customization

Edit these files to adjust behavior:

- **`CLAUDE.md`** — Modify vault structure rules and workflows
- **`settings.local.json`** — Add/remove permissions for Claude
- **`skills/read_paper/SKILL.md`** — Customize paper reading workflow
- **`Templates/`** — Change note templates

---

## ⚙️ Technical Details
.claude/CLAUDE.md` as custom instructions and uses the skills defined in `.claude/
The `.claude` folder is automatically detected by Claude Code when opening this workspace. Claude reads `CLAUDE.md` as custom instructions and uses the skills defined in `skills/` for specialized workflows.

Permissions in `settings.local.json` allow Claude to:
- Search vault structure with bash commands
- Fetch papers from arXiv and OpenReview
- Extract text from PDFs when needed
- Execute task management commands

---

## 📝 Example Session

```
You: "Read the paper 'Attention Is All You Need'"

Claude:
1. Fetches arXiv HTML version
2. Creates Reading/Attention Is All You Need.md
3. Generates Projects/Efficient Attention Mechanisms.md
4. Links new notes to existing research
5. Confirms completion

You: "What projects link to transformers?"

Claude:
1. Searches vault for relevant project links
2. Lists connected research ideas
3. Suggests new directions
```

---

## 🚀 Quick Start

```
1. Open this vault folder in Claude Code
2. Start a conversation
3. Say: "Read paper: [arXiv URL]"
4. Watch your research vault grow
```

---

Built for researchers who want AI-assisted knowledge management in Obsidian.