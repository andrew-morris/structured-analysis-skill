# Structured Analysis Skill for Claude Code

AI-augmented [Structured Analytic Techniques](https://www.cia.gov/resources/csi/static/Tradecraft-Primer-apr09.pdf) (SATs) from US Intelligence Community doctrine — implemented as an interactive Claude Code skill.

18 techniques across 6 analytical phases, with automated evidence gathering, three-layer self-correction, and mandatory citation enforcement.

## What is Structured Analysis?

Structured Analytic Techniques are rigorous methods developed by the US Intelligence Community to combat cognitive bias in high-stakes analysis. They emerged from decades of intelligence failures — Pearl Harbor, Iraq WMD — where the problem was never lack of information, but lack of structured challenge to dominant assumptions.

This skill brings SATs to Claude Code as an interactive `/analyze` command. Ask a question, and the skill orchestrates evidence gathering, technique selection, structured execution, self-correction, and a cited final report — all grounded in the [CIA Tradecraft Primer (2009)](docs/background/Tradecraft-Primer-apr09.pdf) and modern empirical updates.

## Quick Start

### Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and configured
- (Optional) [Firecrawl](https://firecrawl.dev) MCP server for enhanced OSINT web scraping

### Install via Plugin (Recommended)

```bash
# Add the marketplace
/plugin marketplace add blevene/structured-analysis-skill

# Install the plugin
/plugin install structured-analysis@blevene
```

That's it. Run `/analyze` to start your first analysis.

### Install via Git Clone

If you prefer to work from source (or want to contribute):

```bash
git clone https://github.com/blevene/structured-analysis-skill.git
cd structured-analysis-skill
```

Then open Claude Code in the cloned directory — the skill is discovered automatically from the `skills/` directory.

### Manual Install (Non-Claude Code)

The skill files are plain Markdown — they work with any AI assistant that supports structured prompting. To use them manually:

1. Clone the repository (see above)
2. Open `skills/structured-analysis/SKILL.md` — this is the skill entry point with all instructions
3. Feed the contents of `SKILL.md` and `protocols/orchestrator.md` into your AI assistant as system context
4. For each technique, provide the relevant protocol file from `protocols/techniques/` and template from `templates/techniques/`
5. The `docs/library/` directory contains the full reference knowledge base if your assistant needs theoretical grounding

The key files to provide as context:
- `skills/structured-analysis/SKILL.md` — orchestration instructions
- `skills/structured-analysis/protocols/orchestrator.md` — mode routing and technique selection
- `skills/structured-analysis/protocols/evidence-collector.md` — evidence gathering process
- `skills/structured-analysis/templates/report-template.md` — output structure
- `docs/library/00-prime.md` — master reference for all techniques

### Optional: OSINT Setup

The skill gathers web evidence automatically. Without any setup, it uses Claude Code's built-in `WebSearch` and `WebFetch` tools. For faster, more reliable OSINT with richer content extraction, set up [Firecrawl](https://firecrawl.dev):

```bash
claude mcp add firecrawl -e FIRECRAWL_API_KEY=your-api-key -- npx -y firecrawl-mcp
```

Then whitelist the tools in `.claude/settings.local.json`:

```json
{
  "permissions": {
    "allow": [
      "mcp__firecrawl__firecrawl_search",
      "mcp__firecrawl__firecrawl_scrape",
      "WebSearch",
      "WebFetch"
    ]
  }
}
```

This whitelists both Firecrawl (primary) and WebSearch/WebFetch (fallback) so evidence collection runs without permission prompts. If Firecrawl isn't configured, the skill falls back to WebSearch/WebFetch automatically and suggests Firecrawl setup after the analysis.

Without any web tools, the skill still works — it just uses conversation context and local files only. Use `--no-osint` to skip web research explicitly.

### First Analysis

```
/analyze What are the strategic implications of quantum computing for national cybersecurity?
```

The skill will:
1. Assess the problem and select appropriate techniques
2. Gather evidence from conversation context, local files, and web sources
3. Execute each technique with structured protocols
4. Self-correct through three validation layers
5. Auto-remediate any HIGH-severity analytical weaknesses (if found)
6. Present findings with a human review gate before finalization

Output is a structured report with cited key judgments, confidence levels, a monitoring plan, and full evidence registry. See [`example_analysis/`](example_analysis/) for a complete sample output.

### Context-Aware Invocation

The skill reads conversation history before starting. If you've been discussing a problem, sharing files, or pasting data, just run `/analyze` — the skill infers your intent from context:

```
User: I'm worried about our API latency. Here's the trace data: [pastes traces]
User: Could be the new caching layer, or maybe the DB connection pool is saturated.
User: /analyze
```

The skill picks up the problem, the evidence, and the competing explanations, then confirms before proceeding:

```
Based on our conversation, here's what I'm picking up:

Problem: Root cause of API latency degradation
Mode: Adaptive (auto-select techniques)
Techniques: ACH (competing explanations identified), Cross-Impact (variable interactions)
Flags: none
Prior context: Trace data pasted above (Tier 1 evidence)

Does this look right? Adjust anything before I proceed.
```

Explicit arguments always take precedence. If you run `/analyze premortem`, the skill uses your technique choice but still surfaces useful context ("I noticed you shared trace data — I'll include that as evidence.").

## Using Local Evidence

The skill collects evidence from three tiers: conversation context, local files, and OSINT. To feed your own documents into the analysis:

**Mention files in your prompt:**

```
/analyze Should we migrate to Kubernetes?

Context:
- Architecture doc: docs/architecture.md
- Incident reports: reports/2025-Q4-incidents.csv
- Vendor proposal: proposals/cloud-native-migration.pdf
```

**Paste data directly** (becomes Tier 1 evidence, cited as `[User-provided, session context]`):

```
/analyze Is our Q4 revenue forecast realistic?

Key data points:
- Q3 revenue: $4.2M (up 12% QoQ)
- Pipeline coverage: 2.8x for Q4
- Two enterprise deals ($500K+) in final negotiation
- Competitor launched a price-cut campaign in October
```

**Place files in the working directory.** The evidence collector discovers relevant files in your project via Glob.

All tiers combine into a unified evidence registry. Use `--no-osint` to skip web research and rely solely on local evidence.

## Usage

### Modes

| Command | Description |
|---------|-------------|
| `/analyze` | Auto-select techniques based on problem characteristics |
| `/analyze ach` | Run a single named technique directly |
| `/analyze --guided` | Walk through all analytical phases step by step |
| `/analyze --lean` | Abbreviated technique set (fast, ~15 min) |
| `/analyze --resume <id>` | Continue or update a previous analysis |
| `/analyze --iterate <id>` | Re-run full analysis with new evidence |
| `/analyze --iterate <id> ach` | Re-run specific technique(s) only |

### Flags

| Flag | Effect |
|------|--------|
| `--lean` | Use only Problem Restatement + KAC (abbreviated) + Inconsistencies Finder |
| `--comprehensive` | Full 14-question rubric, no technique cap, unlocks adversarial + deception checks |
| `--no-osint` | Disable web research — use only conversation context and local files |
| `--iterate <id>` | Re-run with artifact versioning and evidence delta tracking |

Flags combine: `/analyze --guided --no-osint` runs all phases without web research.

### Techniques

| Name | Technique | Phase |
|------|-----------|-------|
| `customer-checklist` | Customer Checklist | Launch |
| `issue-redefinition` | Issue Redefinition | Launch |
| `restatement` | Problem Restatement | Launch |
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
| `devils-advocacy` | Devil's Advocacy | Challenge |
| `red-hat` | Red Hat Analysis | Challenge |
| `alt-futures` | Alternative Futures | Foresight |
| `deception` | Deception Detection | Diagnostic |

### Example Output

Running `/analyze ach` on a ransomware attribution problem produces:

```markdown
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

Every claim traces to a cited source. Every judgment traces to technique output.

### Auto-Remediation

When self-critique identifies HIGH-severity analytical weaknesses — evidence imbalance >2:1, unstated critical premises, strong counter-arguments, or quality score failures — the skill automatically remediates them before presenting the report. No manual intervention needed:

```
Layer 2 identified 2 HIGH-severity flag(s). Auto-remediating before presenting report...
```

The auto-remediation gate collects targeted evidence, re-runs the flagged techniques (max 3), and regenerates the report as iteration 2 — all transparently. The final report shows what was addressed:

```
## Iteration Suggestions

### Auto-Remediated (HIGH severity)
The following 2 flags were automatically addressed before report finalization:

1. **Evidence imbalance >2:1** [AUTO-REMEDIATED]: H4 evidence ratio was 8:1
   → Re-ran: ach | Evidence focus: Seek sources for the underrepresented side

2. **Unstated premises impacting key judgment** [AUTO-REMEDIATED]: Three critical assumptions lacked evidence
   → Re-ran: kac | Evidence focus: Collect evidence testing the flagged assumptions

### Suggested for Manual Iteration (MEDIUM/LOW severity)
Self-critique identified 1 additional item:

1. **Missing perspectives** [MEDIUM]: No technical operator viewpoint represented
   → Re-run: narratives | Evidence focus: Collect evidence from the missing viewpoint

Suggested command: /analyze --iterate 2026-02-15-cybersecurity-assessment narratives
```

### Manual Iteration

Run the suggested command (or modify it) to iterate without losing the reasoning trail:

```
/analyze --iterate 2026-02-15-cybersecurity-assessment ach
```

The iteration protocol:
- **Archives prior artifacts** as `working/ach-matrix.v1.md` — canonical names always point to latest
- **Accumulates evidence** — new items append with an `Iter` column tracking provenance
- **Compares findings** — cross-iteration synthesis tracks judgment revisions and confidence shifts
- **Records metadata** — each iteration logs its trigger, scope, and changes
- **Tracks gaps in `next-steps.md`** — a standalone ledger at the analysis root tracking all identified gaps and their resolution status (OPEN, REMEDIATED, RESOLVED, DEFERRED) across iterations. The `--iterate` handler reads this file to determine which techniques to re-run and what evidence to collect.

First-run analyses are unaffected. Iteration tracking activates only on `--iterate`.

## Techniques by Phase

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
| Deception Detection | Mirror-imaging, trust bias | Hours |

**Challenge** — Stress-test your conclusions.

| Technique | Bias Mitigated | Effort |
|-----------|---------------|--------|
| What If? Analysis | Status quo bias, failure of imagination | Hours-days |
| Premortem + Self-Critique | Overconfidence, blind spots | 30 min-2 hr |
| Devil's Advocacy | Groupthink, conformity pressure | Hours |
| Red Hat Analysis | Mirror-imaging, ethnocentrism | Hours |

**Foresight** — Model alternative futures.

| Technique | Bias Mitigated | Effort |
|-----------|---------------|--------|
| Counterfactual Reasoning | Rationality attribution, historical inevitability | Hours |
| Contrasting Narratives | Narrative bias, emotional reasoning | Hours |
| Alternative Futures | Single-outcome fixation, failure of imagination | Hours-days |

**Decision Support** — Evaluate options and risks.

| Technique | Bias Mitigated | Effort |
|-----------|---------------|--------|
| Bowtie Analysis | Linear risk thinking, single-point failure focus | Hours |
| Opportunities Incubator | Threat fixation | Hours |

## Reference Library

The `docs/library/` directory contains a comprehensive knowledge base synthesizing 60+ years of SAT doctrine.

Start with **[00-prime.md](docs/library/00-prime.md)** — the master index covering axioms, taxonomy, selection logic, and modern practice.

| File | Content |
|------|---------|
| [00-prime.md](docs/library/00-prime.md) | Master synthesis and navigation guide |
| [01-tradecraft-primer-2009.md](docs/library/01-tradecraft-primer-2009.md) | Original CIA 2009 doctrine |
| [02-tradecraft-primer-analysis.md](docs/library/02-tradecraft-primer-analysis.md) | Expert analysis, ICD 203 mapping |
| [03-practical-guides.md](docs/library/03-practical-guides.md) | Reusable prompts, checklists |
| [04-agile-rigor-update.md](docs/library/04-agile-rigor-update.md) | 2020-2026 evolution, Lean SATs, HMT |
| [05-66-techniques-taxonomy.md](docs/library/05-66-techniques-taxonomy.md) | All 66 techniques by family |
| [06-decision-matrix.md](docs/library/06-decision-matrix.md) | Selection framework and rubrics |
| [07-axioms-and-laws.md](docs/library/07-axioms-and-laws.md) | Foundational principles |
| [08-updates-and-optimizations.md](docs/library/08-updates-and-optimizations.md) | Post-2009 doctrine changes |
| [09-core-techniques.md](docs/library/09-core-techniques.md) | The essential 8 techniques |

---

## Developer Guide

Everything below is for contributors and people who want to understand or modify the internals.

### Architecture

```
Question → Orchestrator → Evidence Collector → Technique Dispatch → Self-Correction → Report
                ↑                                  ├─ Tier 1 (parallel subagents)
                │                                  ├─ Tier 2 (parallel subagents)
                │                                  ├─ Tier 3 (parallel subagents)
                │                                  └─ Tier 4 (parallel subagents)
                │
                ├─── Auto-Remediation Gate ← HIGH-severity Layer 2 flags ──────────┘
                │    (automatic: re-collect evidence, re-run techniques, regen report)
                │
                └─── Manual Iterate (--iterate) ← artifact versioning + evidence delta
```

**Orchestrator** — The [orchestrator protocol](skills/structured-analysis/protocols/orchestrator.md) handles mode detection, technique selection, and workflow management. In adaptive mode, it uses a 14-question rubric to match problem characteristics to appropriate techniques.

**Technique Dispatch** — When 2+ techniques are selected, the orchestrator dispatches them to background subagents in dependency-aware tiers. Tier 1 techniques (brainstorm, kac) run first; tier 2 (ach, cross-impact, inconsistencies) wait for tier 1 outputs; tier 3 and 4 cascade similarly. Within each tier, techniques run in parallel. Each subagent reads its protocol and template, executes the full technique workflow, writes the artifact to disk, and returns only a compact findings summary. The main context window accumulates summaries and file paths — not full technique work — preserving budget for synthesis, follow-ups, and iteration. Single-technique runs (Direct mode) execute in-context to avoid subagent overhead.

**Evidence Collector** — The [evidence collector](skills/structured-analysis/protocols/evidence-collector.md) gathers evidence across three tiers (conversation, local files, OSINT). OSINT uses a four-step pipeline: theme planning generates adaptive search themes based on problem domain, technique needs, and stakeholder perspectives (3 core + 3/5/7 adaptive themes scaling by mode); foreground subagents scrape raw content to disk; background subagents extract structured evidence; then the main context integrates everything into the registry. This keeps raw web content out of the context window. MCP tools are only available in the main conversation and foreground subagents, not background subagents — the pipeline is designed around this constraint.

**Evidence Sufficiency Gate** — After collection, hard checks (minimum count, quality floor) can halt the analysis; soft checks (source diversity, diagnostic coverage, temporal recency) log warnings that surface in the report.

**Self-Correction** — Three layers plus an auto-remediation gate: (1) protocol compliance after each technique, (2) analytical self-critique before report synthesis with severity-classified flags, (3) human review gate before finalization. Between layers 2 and 3, the **Auto-Remediation Gate** checks for HIGH-severity flags (evidence imbalance, unstated critical premises, strong counter-arguments, analytical bias, quality failures). If any exist, it automatically invokes the iteration handler to collect targeted evidence, re-run flagged techniques (max 3, max 1 cycle), and regenerate the report — all before the user sees output. MEDIUM/LOW flags are presented as manual iteration suggestions.

**Iteration Handler** — The [iteration handler](skills/structured-analysis/protocols/iteration-handler.md) manages artifact versioning, evidence delta tracking, and cross-iteration synthesis when re-running techniques.

**Report Generator** — The [report generator](skills/structured-analysis/protocols/report-generator.md) synthesizes technique outputs into a final assessment with confidence ratings, monitoring plan, and citation registry.

### Citation Methods

Every claim must be cited. Five methods:

| Method | Format |
|--------|--------|
| OSINT | `[Source](URL) — Retrieved: YYYY-MM-DD` |
| FILE | `[filename:line_range]` |
| USER | `[User-provided, session context]` |
| ANALYSIS | `[Derived via technique_name]` |
| PRIOR-ITERATION | `[PRIOR-v{N}: technique_name]` |

### Project Structure

```
structured-analysis-skill/
├── .claude-plugin/
│   ├── plugin.json                   # Plugin manifest
│   └── marketplace.json              # Marketplace discovery
├── skills/structured-analysis/
│   ├── SKILL.md                      # Skill entry point
│   ├── protocols/
│   │   ├── orchestrator.md           # Mode routing and selection logic
│   │   ├── evidence-collector.md     # Evidence gathering and OSINT
│   │   ├── report-generator.md       # Report synthesis
│   │   ├── iteration-handler.md      # Artifact versioning and iteration logic
│   │   └── techniques/              # 18 technique execution protocols
│   └── templates/
│       ├── report-template.md        # Final report structure
│       ├── evidence-registry-template.md
│       ├── monitoring-plan-template.md
│       ├── review-summary-template.md  # Phase A → Phase B handoff summary
│       ├── meta-template.md
│       ├── iteration-meta-template.md # Per-iteration metadata
│       ├── next-steps-template.md    # Iteration suggestions ledger
│       ├── techniques/              # 18 technique artifact templates
│       └── sections/                # Reusable report components
├── docs/
│   ├── library/                     # Reference knowledge base (10 files)
│   ├── background/                  # Source materials (CIA Primer + analyses)
│   └── plans/                       # Design and implementation documents
├── helper_scripts/
│   └── extract_docx_text.py         # DOCX text extraction utility
└── LICENSE                          # Apache 2.0
```

### Contributing

**Adding a technique:**

1. Create protocol at `protocols/techniques/<name>.md` (SETUP → PRIME → EXECUTE → ARTIFACT → FINDINGS → HANDOFF)
2. Create template at `templates/techniques/<name>.md` with `{{PLACEHOLDER}}` tokens
3. Add to routing table in `protocols/orchestrator.md`
4. Add library references linking to `docs/library/` files

**Extending the reference library:**

Library files follow a numbered sequence. Update `00-prime.md` to reference new material. All claims must cite their source.

## License

[Apache 2.0](LICENSE)
