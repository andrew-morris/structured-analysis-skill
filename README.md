# Structured Analysis Skill for Claude Code

AI-augmented [Structured Analytic Techniques](https://www.cia.gov/resources/csi/static/Tradecraft-Primer-apr09.pdf) (SATs) from US Intelligence Community doctrine — implemented as an interactive Claude Code skill.

14 techniques across 5 analytical phases, with automated evidence gathering, three-layer self-correction, and mandatory citation enforcement.

## What is Structured Analysis?

Structured Analytic Techniques are rigorous methods developed by the US Intelligence Community to combat cognitive bias in high-stakes analysis. They emerged from decades of intelligence failures — Pearl Harbor, Iraq WMD — where the problem was never lack of information, but lack of structured challenge to dominant assumptions.

This skill brings SATs to Claude Code as an interactive `/analyze` command. Ask a question, and the skill orchestrates evidence gathering, technique selection, structured execution, self-correction, and a cited final report — all grounded in the [CIA Tradecraft Primer (2009)](docs/background/Tradecraft-Primer-apr09.pdf) and modern empirical updates.

## Quick Start

### Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and configured
- (Optional) [Firecrawl](https://firecrawl.dev) MCP server for enhanced OSINT web scraping

### Installation

1. Clone the repository:

```bash
git clone https://github.com/blevene/structured-analysis-skill.git
cd structured-analysis-skill
```

2. Add the skill to your Claude Code project. In your project's `.claude/settings.local.json`, add the skill path:

```json
{
  "permissions": {
    "allow": []
  }
}
```

Then symlink or copy the `skills/structured-analysis/` directory into your project's skill directory, or reference it directly in your Claude Code configuration.

3. (Optional) Set up Firecrawl for OSINT evidence gathering:

```bash
claude mcp add firecrawl -e FIRECRAWL_API_KEY=your-api-key -- npx -y firecrawl-mcp
```

Get an API key at [firecrawl.dev](https://firecrawl.dev). The skill falls back to WebSearch/WebFetch if Firecrawl is unavailable.

4. Verify the skill is available:

```
/analyze
```

### First Analysis

```
/analyze What are the strategic implications of quantum computing for national cybersecurity?
```

The skill will automatically:
- Assess the problem characteristics
- Select appropriate techniques (e.g., Key Assumptions Check + ACH + What If?)
- Gather evidence from conversation context, local files, and web sources
- Execute each technique with structured protocols
- Self-correct through three validation layers
- Present findings with a human review gate before finalization

## Usage

### Modes

| Command | Mode | Description |
|---------|------|-------------|
| `/analyze` | **Adaptive** | Auto-selects techniques based on problem characteristics |
| `/analyze ach` | **Direct** | Run a single named technique |
| `/analyze --guided` | **Guided** | Walk through all analytical phases step by step |
| `/analyze --resume <id>` | **Resume** | Continue or update a previous analysis |

### Flags

| Flag | Effect |
|------|--------|
| `--lean` | Abbreviated technique set (Problem Restatement + KAC Quick + Inconsistencies Finder) |
| `--no-osint` | Disable web research — use only conversation context and local files |

Flags combine: `/analyze --guided --no-osint` runs all phases without web research.

### Technique Names

Use these with direct mode (`/analyze <name>`):

| Name | Technique | Phase |
|------|-----------|-------|
| `customer-checklist` | Customer Checklist | Launch |
| `issue-redefinition` | Issue Redefinition | Launch |
| `restatement` | Problem Restatement (Lean) | Launch |
| `brainstorm` | Structured Brainstorming | Exploration |
| `kac` | Key Assumptions Check | Diagnostic |
| `ach` | Analysis of Competing Hypotheses | Diagnostic |
| `inconsistencies` | Inconsistencies Finder | Diagnostic |
| `cross-impact` | Cross-Impact Matrix | Diagnostic |
| `what-if` | What If? Analysis | Challenge |
| `premortem` | Premortem + Self-Critique | Challenge |
| `counterfactual` | Counterfactual Reasoning | Foresight |
| `narratives` | Contrasting Narratives | Foresight |
| `bowtie` | Bowtie Analysis | Decision Support |
| `opportunities` | Opportunities Incubator | Decision Support |

### Example: Direct ACH Analysis

```
/analyze ach
```

Produces an Analysis of Competing Hypotheses matrix like:

```
# Analysis of Competing Hypotheses: Ransomware Attribution

## Hypotheses
| ID | Hypothesis                        | Source            |
|----|-----------------------------------|-------------------|
| H1 | State-sponsored APT group         | OSINT reporting   |
| H2 | Criminal ransomware syndicate     | FBI flash alert   |
| H3 | Insider threat with external tool | User-provided     |
| H4 | Copycat using leaked tooling      | Analyst-derived   |

## Diagnosticity Matrix
| Evidence               | H1 | H2 | H3 | H4 | Diagnostic Value |
|------------------------|----|----|----|----|------------------|
| C2 infrastructure      | C  | C  | N  | C  | LOW              |
| Ransom note language   | I  | C  | N  | C  | HIGH             |
| Attack timing (holiday)| C  | C  | I  | C  | HIGH             |
| No data exfiltration   | I  | N  | C  | N  | HIGH             |

## Preliminary Findings
- Most plausible: H2 (Criminal syndicate) — fewest inconsistencies
- Confidence: MODERATE
- Key sensitivity: If "no exfiltration" evidence changes, H1 becomes viable
```

Every claim traces back to a cited source. Every judgment traces to technique output.

### Example: Lean Analysis

```
/analyze --lean
```

Runs in under 15 minutes:
1. **Problem Restatement** — rewrite the question 3 ways to break anchoring
2. **KAC Quick** — list and bin the top 5 assumptions
3. **Inconsistencies Finder** — search for evidence contradicting the lead hypothesis

## Techniques

### By Analytical Phase

**Launch** — Define the problem correctly before solving it.

| Technique | Bias Mitigated | Effort |
|-----------|---------------|--------|
| Customer Checklist | Type III Error (wrong problem) | Minutes |
| Issue Redefinition | Anchoring | Minutes |
| Problem Restatement | Anchoring (rapid reframing) | 5 min |

**Diagnostic** — Test assumptions and evaluate hypotheses.

| Technique | Bias Mitigated | Effort |
|-----------|---------------|--------|
| Structured Brainstorming | Premature closure, groupthink | 60-90 min |
| Key Assumptions Check | Status quo bias, institutional inertia | 15 min-2 hr |
| ACH | Confirmation bias, anchoring | Hours-days |
| Inconsistencies Finder | Confirmation bias (lean ACH) | 15-30 min |
| Cross-Impact Matrix | Linear thinking, missed interactions | Hours |

**Challenge** — Stress-test your conclusions.

| Technique | Bias Mitigated | Effort |
|-----------|---------------|--------|
| What If? Analysis | Status quo bias, failure of imagination | Hours-days |
| Premortem + Self-Critique | Overconfidence, blind spots | 30 min-2 hr |

**Foresight** — Model alternative futures.

| Technique | Bias Mitigated | Effort |
|-----------|---------------|--------|
| Counterfactual Reasoning | Rationality attribution, historical inevitability | Hours |
| Contrasting Narratives | Narrative bias, emotional reasoning | Hours |

**Decision Support** — Evaluate options and risks.

| Technique | Bias Mitigated | Effort |
|-----------|---------------|--------|
| Bowtie Analysis | Linear risk thinking, single-point failure focus | Hours |
| Opportunities Incubator | Threat fixation | Hours |

## Reference Library

The `docs/library/` directory contains a comprehensive knowledge base synthesizing 60+ years of SAT doctrine. This is the theoretical foundation backing every technique protocol.

### Entry Point

Start with **[00-prime.md](docs/library/00-prime.md)** — the master index that synthesizes all source materials into a single navigable reference covering axioms, taxonomy, selection logic, and modern practice.

### Library Index

| File | Content | Use When |
|------|---------|----------|
| [00-prime.md](docs/library/00-prime.md) | Master synthesis and navigation guide | Starting point for everything |
| [01-tradecraft-primer-2009.md](docs/library/01-tradecraft-primer-2009.md) | Original CIA 2009 doctrine | Need foundational technique details or history |
| [02-tradecraft-primer-analysis.md](docs/library/02-tradecraft-primer-analysis.md) | Expert analysis, ICD 203 mapping | Need compliance mapping or expert guides |
| [03-practical-guides.md](docs/library/03-practical-guides.md) | Reusable prompts, checklists | Need quick-reference or the analysis prompt template |
| [04-agile-rigor-update.md](docs/library/04-agile-rigor-update.md) | 2020-2026 evolution, Lean SATs, HMT | Need modern methodology or AI integration |
| [05-66-techniques-taxonomy.md](docs/library/05-66-techniques-taxonomy.md) | All 66 techniques by family | Need to look up any technique |
| [06-decision-matrix.md](docs/library/06-decision-matrix.md) | Selection framework and rubrics | Need to choose which technique to use |
| [07-axioms-and-laws.md](docs/library/07-axioms-and-laws.md) | Foundational principles | Need theoretical grounding |
| [08-updates-and-optimizations.md](docs/library/08-updates-and-optimizations.md) | Post-2009 doctrine changes, cyber | Need chronological evolution |
| [09-core-techniques.md](docs/library/09-core-techniques.md) | The essential 8 techniques | Need the minimum viable technique set |

### Background Materials

The `docs/background/` directory contains the primary source document ([CIA Tradecraft Primer, 2009](docs/background/Tradecraft-Primer-apr09.pdf)) and secondary analyses that informed the library.

## Architecture

### System Flow

```
Question → Orchestrator → Evidence Collector → Technique(s) → Self-Correction → Report
```

### Orchestrator

The [orchestrator protocol](skills/structured-analysis/protocols/orchestrator.md) handles mode detection, technique selection, and workflow management. In adaptive mode, it uses a 12-question rubric to match problem characteristics to appropriate techniques, then confirms the recommendation with the user before proceeding.

### Evidence Collector

The [evidence collector](skills/structured-analysis/protocols/evidence-collector.md) gathers evidence across three tiers:

| Tier | Source | Citation Format |
|------|--------|----------------|
| **1** | Conversation context | `[User-provided, session context]` |
| **2** | Local files | `[filename:line_range]` |
| **3** | OSINT (web research) | `[Source](URL) — Retrieved: YYYY-MM-DD` |

OSINT uses three parallel search agents (background/context, contrarian/critical, recent developments) to reduce confirmation bias in evidence gathering. Firecrawl MCP is the primary scraping tool with WebSearch/WebFetch as fallback.

### Self-Correction (3 Layers)

| Layer | When | What |
|-------|------|------|
| **1** | After each technique (silent) | Protocol compliance — all steps completed? All template sections filled? |
| **2** | Before report (silent) | Analytical self-critique — assumption audit, evidence balance, confidence calibration |
| **3** | Before finalization | Human review gate — presents summary, incorporates feedback |

### Report Generator

The [report generator](skills/structured-analysis/protocols/report-generator.md) synthesizes technique outputs into a final assessment with confidence ratings, a monitoring plan with leading indicators, and a complete citation registry.

### Citation Enforcement

Every claim in every artifact must be cited. No exceptions. OSINT is never presented as fact — always "according to [source]". Four citation methods:

- **OSINT**: `[Source](URL) — Retrieved: YYYY-MM-DD`
- **FILE**: `[filename:line_range]`
- **USER**: `[User-provided, session context]`
- **ANALYSIS**: `[Derived via technique_name]`

## Project Structure

```
structured-analysis-skill/
├── skills/structured-analysis/
│   ├── analyze.md                    # Skill entry point
│   ├── protocols/
│   │   ├── orchestrator.md           # Mode routing and selection logic
│   │   ├── evidence-collector.md     # Evidence gathering and OSINT
│   │   ├── report-generator.md       # Report synthesis
│   │   └── techniques/              # 14 technique execution protocols
│   │       ├── ach.md
│   │       ├── bowtie-analysis.md
│   │       ├── contrasting-narratives.md
│   │       └── ... (11 more)
│   └── templates/
│       ├── report-template.md        # Final report structure
│       ├── evidence-registry-template.md
│       ├── monitoring-plan-template.md
│       ├── meta-template.md
│       ├── techniques/              # 14 technique artifact templates
│       │   ├── ach-matrix.md
│       │   ├── bowtie.md
│       │   └── ... (12 more)
│       └── sections/                # Reusable report components
│           ├── header.md
│           ├── confidence-scale.md
│           ├── citation-block.md
│           └── judgment-table.md
├── docs/
│   ├── library/                     # Reference knowledge base (10 files)
│   ├── background/                  # Source materials (CIA Primer + analyses)
│   └── plans/                       # Design and implementation documents
├── helper_scripts/
│   └── extract_docx_text.py         # Utility to extract text from DOCX files
└── LICENSE                          # Apache 2.0
```

## Contributing

### Adding a New Technique

1. **Create the protocol** at `skills/structured-analysis/protocols/techniques/<name>.md` following the SETUP → PRIME → EXECUTE → ARTIFACT → FINDINGS → HANDOFF structure used by existing protocols.

2. **Create the template** at `skills/structured-analysis/templates/techniques/<name>.md` defining the artifact structure with `{{PLACEHOLDER}}` tokens.

3. **Add to the routing table** in `skills/structured-analysis/protocols/orchestrator.md` with the invocation name, protocol path, template path, and phase.

4. **Add library references** to the protocol header linking to relevant `docs/library/` files.

### Extending the Reference Library

Library files in `docs/library/` follow a numbered sequence. The master index at `00-prime.md` should be updated to reference any new material. All claims in library files should cite their source (primary document, edition, or research paper).

### Helper Scripts

The `helper_scripts/` directory contains utilities for working with source materials:

- `extract_docx_text.py` — Extracts text from DOCX files in `docs/background/gemini-analysis/`. Run with `uv run helper_scripts/extract_docx_text.py`.

## License

[Apache 2.0](LICENSE)
