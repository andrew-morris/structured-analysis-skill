# Structured Analysis Skill Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a Claude Code skill (`/analyze`) that conducts structured analytic techniques against any problem domain, producing templatized artifacts with mandatory citations.

**Architecture:** Single master skill (`analyze.md`) with internal protocol files read on demand, technique templates for artifact generation, and parallel sub-agent dispatching for OSINT evidence gathering. All files are markdown — pure prompt engineering, no code dependencies.

**Tech Stack:** Claude Code skill system (YAML frontmatter + markdown), Task tool for parallel agents, Firecrawl MCP (primary OSINT) with WebSearch/WebFetch fallback.

**Design Doc:** `docs/plans/2026-02-15-structured-analysis-skill-design.md`
**Reference Library:** `docs/library/00-prime.md` through `docs/library/09-core-techniques.md`

---

## Important Context for Implementing Agent

- This is a **markdown-only** project. Every file you create is a `.md` file — there is no Python, TypeScript, or other code.
- **Skill format**: YAML frontmatter (`name`, `description`) + markdown body. See `superpowers:writing-skills` for canonical format.
- **Templates** use `{{PLACEHOLDER}}` tokens as structural contracts — Claude fills them with actual analytical prose at runtime, not literal string replacement.
- **Protocols** are imperative instructions Claude reads and follows. They should be concise, action-oriented, and reference the library docs for background context.
- **The reference library** (`docs/library/00-prime.md` and files 01-09) is the knowledge base. Protocol files distill this into execution instructions.
- After creating each file, verify it with: `wc -w <file>` to check token budget, and read it back to verify structural completeness.

---

### Task 1: Create Directory Structure

**Files:**
- Create: `skills/structured-analysis/analyze.md` (placeholder)
- Create: `skills/structured-analysis/protocols/` (directory)
- Create: `skills/structured-analysis/protocols/techniques/` (directory)
- Create: `skills/structured-analysis/templates/` (directory)
- Create: `skills/structured-analysis/templates/techniques/` (directory)
- Create: `skills/structured-analysis/templates/sections/` (directory)

**Step 1: Create full directory tree**

Run:
```bash
mkdir -p skills/structured-analysis/protocols/techniques
mkdir -p skills/structured-analysis/templates/techniques
mkdir -p skills/structured-analysis/templates/sections
```

**Step 2: Create placeholder master skill file**

Create `skills/structured-analysis/analyze.md` with just the frontmatter:

```markdown
---
name: analyze
description: Use when conducting structured analysis on any problem — assessing competing hypotheses, challenging assumptions, stress-testing judgments, or producing defensible analytical assessments with evidence and citations
---

# Structured Analysis Skill

(Implementation pending — see Task 13)
```

**Step 3: Verify structure**

Run: `find skills/ -type f -o -type d | sort`

Expected: Directory tree matching design doc Section 5.

**Step 4: Commit**

```bash
git add skills/
git commit -m "scaffold: create skill directory structure"
```

---

### Task 2: Shared Section Templates

These are small reusable partials included by reference in technique and report templates. They enforce consistency across all artifacts.

**Files:**
- Create: `skills/structured-analysis/templates/sections/header.md`
- Create: `skills/structured-analysis/templates/sections/confidence-scale.md`
- Create: `skills/structured-analysis/templates/sections/citation-block.md`
- Create: `skills/structured-analysis/templates/sections/judgment-table.md`

**Step 1: Write header template**

Create `skills/structured-analysis/templates/sections/header.md`:

```markdown
# {{TECHNIQUE_NAME}}: {{PROBLEM_TITLE}}

> **Analysis ID**: {{ANALYSIS_ID}}
> **Date**: {{DATE}}
> **Technique**: {{TECHNIQUE_FULL_NAME}}
> **Phase**: {{PHASE_NAME}}
> **Analyst**: User + Claude ({{MODE}} mode)
> **Evidence base**: {{EVIDENCE_COUNT}} items | [Full registry](../evidence-registry.md)
```

**Step 2: Write confidence scale template**

Create `skills/structured-analysis/templates/sections/confidence-scale.md`:

```markdown
### Confidence Level Definitions

| Level | Meaning | Criteria |
|-------|---------|----------|
| **High** | Well-corroborated by multiple independent sources; key assumptions well-supported | 3+ independent sources; linchpin assumptions rated Supported |
| **Moderate** | Plausibly supported but with notable gaps; some assumptions unverified | 1-2 sources with partial corroboration; some assumptions rated Correct-with-Caveats |
| **Low** | Fragmented evidence; significant assumptions unsupported; alternative explanations viable | Single source or uncorroborated; linchpin assumptions rated Unsupported |

> Per ICD 203 Standard 2: Analysts must express and explain uncertainties associated with major analytic judgments.
```

**Step 3: Write citation block template**

Create `skills/structured-analysis/templates/sections/citation-block.md`:

```markdown
---

## Sources & Citations

| Ref | Source | Retrieved | Reliability | Method |
|-----|--------|-----------|-------------|--------|
| [{{REF_NUM}}] | {{FULL_CITATION_WITH_URL}} | {{DATE}} | {{HIGH/MED/LOW}} | {{OSINT/FILE/USER/ANALYSIS}} |

**Citation Methods:**
- **OSINT**: Retrieved via Firecrawl MCP or WebSearch/WebFetch fallback
- **FILE**: Read from local file system
- **USER**: Provided by analyst in conversation
- **ANALYSIS**: Analytical judgment produced via named technique protocol

> Every claim must be cited. Uncited claims are not permitted.
```

**Step 4: Write judgment table template**

Create `skills/structured-analysis/templates/sections/judgment-table.md`:

```markdown
## Key Judgments

| # | Judgment | Confidence | Supporting Techniques | Critical Assumptions | Indicators to Watch |
|---|---------|------------|----------------------|---------------------|-------------------|
| J{{NUM}} | {{JUDGMENT_TEXT}} | {{HIGH/MOD/LOW}} | {{TECHNIQUE_LIST}} | {{ASSUMPTION_REFS}} | {{INDICATOR_REFS}} |

> Each judgment links to the technique artifacts and assumption inventory that support it.
> Confidence levels follow the scale defined in the Confidence Level Definitions section.
```

**Step 5: Verify and commit**

Run: `wc -w skills/structured-analysis/templates/sections/*.md`

Expected: Each file under 150 words.

```bash
git add skills/structured-analysis/templates/sections/
git commit -m "feat: add shared section templates (header, confidence, citations, judgments)"
```

---

### Task 3: Infrastructure Templates (Report, Monitoring, Evidence, Meta)

These are the four top-level artifact templates that structure the final outputs.

**Files:**
- Create: `skills/structured-analysis/templates/report-template.md`
- Create: `skills/structured-analysis/templates/monitoring-plan-template.md`
- Create: `skills/structured-analysis/templates/evidence-registry-template.md`
- Create: `skills/structured-analysis/templates/meta-template.md`

**Step 1: Write report template**

Create `skills/structured-analysis/templates/report-template.md`. This is the largest template — the final deliverable.

Required sections (from design doc Section 6):
- Header with analysis metadata
- Executive Summary (2-3 paragraphs: key findings, confidence levels, primary recommendation)
- Problem Framing (how defined; restatements considered; final framing)
- Evidence Base (summary with source breakdown, quality assessment, link to full registry)
- Analysis sections — one per technique applied, each containing:
  - Purpose: why selected
  - Process: abbreviated description
  - Findings: key results with citations
  - Confidence: level with justification
  - Link to working artifact
- Synthesis (cross-technique agreement/disagreement, integrated assessment)
- Key Judgments table (using `sections/judgment-table.md` format)
- Risks & Blind Spots (from Layer 2 self-critique: assumption audit, evidence balance, confidence calibration, alternative check, missing voices)
- Monitoring Plan summary with link to full plan
- Complete Sources bibliography

**Step 2: Write monitoring plan template**

Create `skills/structured-analysis/templates/monitoring-plan-template.md`.

Required sections (from design doc):
- Header with analysis metadata and recommended review cadence
- Key Judgments Under Watch (list from report)
- Confirming Indicators table (indicator, what to look for, source to check, status with emoji ⬜/✅/❌)
- Disconfirming Indicators table (same structure)
- Trigger Thresholds (how many disconfirming indicators warrant re-analysis)
- Review Log table (date, reviewer, indicators updated, action taken)

**Step 3: Write evidence registry template**

Create `skills/structured-analysis/templates/evidence-registry-template.md`.

Required sections:
- Header with analysis metadata
- Collection Summary (total items, breakdown by tier: conversation/file/OSINT, collection methods used)
- Evidence Inventory table (ID, source, type, summary, reliability rating, diagnostic value, citation)
- Quality Assessment Notes (overall evidence balance, identified gaps, deception indicators checked)
- Source Reliability Ratings explanation

**Step 4: Write meta template**

Create `skills/structured-analysis/templates/meta-template.md`.

Required sections:
- Analysis ID and timestamps (created, last updated)
- Mode used (adaptive/direct/guided/lean)
- Problem statement (original + reframed)
- Techniques executed (ordered list with status: completed/skipped/pending)
- Evidence collection config (OSINT enabled/disabled, Firecrawl available, sources checked)
- Self-correction results (Layer 1 flags, Layer 2 flags, human feedback incorporated)
- File manifest (all artifacts produced with relative paths)

**Step 5: Verify and commit**

Run: `wc -w skills/structured-analysis/templates/*.md`

```bash
git add skills/structured-analysis/templates/*.md
git commit -m "feat: add infrastructure templates (report, monitoring, evidence, meta)"
```

---

### Task 4: Launch Phase Technique Templates + Protocols (3 techniques)

Customer Checklist, Issue Redefinition, Problem Restatement (Lean).

**Files:**
- Create: `skills/structured-analysis/templates/techniques/customer-checklist.md`
- Create: `skills/structured-analysis/templates/techniques/issue-redefinition.md`
- Create: `skills/structured-analysis/templates/techniques/problem-restatement.md`
- Create: `skills/structured-analysis/protocols/techniques/customer-checklist.md`
- Create: `skills/structured-analysis/protocols/techniques/issue-redefinition.md`
- Create: `skills/structured-analysis/protocols/techniques/problem-restatement.md`
- Reference: `docs/library/09-core-techniques.md` (core technique profiles)
- Reference: `docs/library/01-tradecraft-primer-2009.md` (original methods)
- Reference: `docs/library/04-agile-rigor-update.md` (Problem Restatement)

**Step 1: Write Customer Checklist template**

Template sections: header, stakeholder identification, requirements table (question/answer/source), "So What?" statement, required output format, success criteria, citation block.

**Step 2: Write Customer Checklist protocol**

Protocol structure — imperative execution steps:
1. SETUP: Read conversation context, identify the requester/stakeholder
2. PRIME: Explain purpose ("clarify what the consumer actually needs")
3. EXECUTE: Work through Customer Checklist questions:
   - What is the core question being asked?
   - Who will consume this analysis?
   - What decisions will this inform?
   - What format is needed?
   - What is the timeline?
   - What is the "So What?" — what action should the consumer be able to take?
4. ARTIFACT: Write `working/requirements.md` using template
5. FINDINGS: Summarize the refined requirement
6. HANDOFF: Pass to Issue Redefinition
- Watch-outs: Type III Error (solving wrong problem), assuming the stated question is the real question
- Bias mitigated: Type III Error
- Effort: Minutes

**Step 3: Write Issue Redefinition template**

Template sections: header, original problem statement, restatement table (3+ reformulations with shift type: actor→system, threat→vulnerability, why→how, etc.), selected framing with rationale, citation block.

**Step 4: Write Issue Redefinition protocol**

Protocol steps:
1. SETUP: Read requirements from Customer Checklist output (or conversation)
2. PRIME: Explain purpose ("break anchoring on initial framing")
3. EXECUTE:
   - State original problem as given
   - Generate 3+ reformulations using specific shifts: Actor→System, Threat→Vulnerability, Short-term→Long-term, "Why"→"How", Reverse the question (turn 180 degrees)
   - For each: explain what new perspectives it opens
   - Select best framing with explicit rationale
4. ARTIFACT: Write `working/problem-framing.md` using template
5. FINDINGS: The reframed problem statement
6. HANDOFF: Pass to KAC or next technique
- Watch-outs: Anchoring bias, accepting first framing
- Bias mitigated: Anchoring
- Effort: Minutes

**Step 5: Write Problem Restatement (Lean) template + protocol**

Template: Appended section to `problem-framing.md` with 3 rapid reformulations and shift analysis.

Protocol: Lean 5-minute version. Steps: Rewrite the question 3 different ways, shift focus (actor→system, threat→vulnerability), identify which restatement opens the most new analytical paths. This is the "Lean SAT" version of Issue Redefinition for time-pressured situations.

**Step 6: Verify and commit**

Run: `wc -w skills/structured-analysis/templates/techniques/customer-checklist.md skills/structured-analysis/templates/techniques/issue-redefinition.md skills/structured-analysis/templates/techniques/problem-restatement.md`

Run: `wc -w skills/structured-analysis/protocols/techniques/customer-checklist.md skills/structured-analysis/protocols/techniques/issue-redefinition.md skills/structured-analysis/protocols/techniques/problem-restatement.md`

```bash
git add skills/structured-analysis/templates/techniques/customer-checklist.md skills/structured-analysis/templates/techniques/issue-redefinition.md skills/structured-analysis/templates/techniques/problem-restatement.md skills/structured-analysis/protocols/techniques/customer-checklist.md skills/structured-analysis/protocols/techniques/issue-redefinition.md skills/structured-analysis/protocols/techniques/problem-restatement.md
git commit -m "feat: add launch phase technique templates and protocols (3 techniques)"
```

---

### Task 5: Diagnostic Phase Technique Templates + Protocols (5 techniques)

Structured Brainstorming, Key Assumptions Check, ACH, Inconsistencies Finder, Cross-Impact Matrix.

**Files:**
- Create: `skills/structured-analysis/templates/techniques/brainstorm.md`
- Create: `skills/structured-analysis/templates/techniques/assumptions.md`
- Create: `skills/structured-analysis/templates/techniques/ach-matrix.md`
- Create: `skills/structured-analysis/templates/techniques/inconsistencies.md`
- Create: `skills/structured-analysis/templates/techniques/cross-impact.md`
- Create: `skills/structured-analysis/protocols/techniques/structured-brainstorming.md`
- Create: `skills/structured-analysis/protocols/techniques/key-assumptions-check.md`
- Create: `skills/structured-analysis/protocols/techniques/ach.md`
- Create: `skills/structured-analysis/protocols/techniques/inconsistencies-finder.md`
- Create: `skills/structured-analysis/protocols/techniques/cross-impact-matrix.md`
- Reference: `docs/library/01-tradecraft-primer-2009.md` (original ACH, KAC, Brainstorming methods)
- Reference: `docs/library/02-tradecraft-primer-analysis.md` (expert ACH + KAC guides with tools/watch-outs)
- Reference: `docs/library/04-agile-rigor-update.md` (Inconsistencies Finder, Lean ACH)
- Reference: `docs/library/09-core-techniques.md` (core technique profiles)

**Step 1: Write Structured Brainstorming template + protocol**

Template sections: header, focal question, divergent ideas list (numbered, uncensored), cluster groups (named), outlier ideas, priority votes, top priorities summary, citation block.

Protocol: Follow the 2009 Primer's two-phase twelve-step method adapted for AI-human teaming:
1. SETUP: Read problem framing + evidence context
2. PRIME: Explain divergent thinking purpose
3. EXECUTE:
   - State focal question clearly
   - Divergent phase: Generate 15+ ideas without censorship or judgment, covering all angles
   - Convergent phase: Group by commonality, label clusters, identify outliers
   - Prioritize: Rank by relevance and novelty
4. ARTIFACT: Write `working/brainstorm.md`
5. FINDINGS: Top priority clusters and outlier insights
- Watch-outs: Premature judgment, "killer phrases", stopping too early
- Bias mitigated: Premature closure, groupthink
- Effort: 60-90 minutes (human); minutes (AI-assisted)

**Step 2: Write Key Assumptions Check template + protocol**

Template (from design doc brainstorm session — full version):
- Header
- Current Analytic Line: {{ANALYTIC_LINE}}
- Assumption Inventory table: #, Assumption, Stated/Unstated, Category, Bin (S/C/U), Linchpin? (Y/N)
- Linchpin Analysis (per linchpin): justification, failure conditions, supporting evidence with citations, challenging evidence with citations, impact if wrong
- Fault Lines Summary: priority, assumption, risk, recommended action
- Citation block

Protocol: Follow 2009 Primer KAC + 02-analysis expert guide:
1. SETUP: Read problem framing + any prior technique outputs
2. PRIME: Explain purpose — "surface hidden premises"
3. EXECUTE:
   - State current analytic line (write it down)
   - List ALL premises — stated AND unstated
   - Challenge each: Why must this be true? Under all conditions? Still true now?
   - Bin each: Supported (S) / Correct-with-Caveats (C) / Unsupported (U)
   - Identify Linchpin assumptions (if wrong → analysis collapses)
   - For each linchpin: document what would demand rethinking
4. ARTIFACT: Write `working/assumptions.md`
5. FINDINGS: Linchpin assumptions and fault lines
- Watch-outs: Status quo bias (difficulty challenging "accepted truths"), missing unstated assumptions
- Bias mitigated: Status quo bias, institutional inertia
- Effort: 15 min to 2 hours

**Step 3: Write ACH template + protocol**

Template (the most complex — from design doc brainstorm):
- Header
- Problem Statement
- Hypotheses table: ID, Hypothesis, Source/Basis
- Evidence table: ID, Evidence, Source, Reliability, Citation
- Diagnosticity Matrix: Evidence rows x Hypothesis columns, C/I/N ratings, Diagnostic Value per row
- Legend: C=Consistent, I=Inconsistent, N=Neutral
- Rule callout: "Evidence consistent with ALL hypotheses = Zero diagnostic value"
- Inconsistency Tally: Hypothesis, Inconsistent Items, Score, Rank
- Sensitivity Analysis: Critical Evidence, If Removed/Reinterpreted, Impact on Conclusion
- Preliminary Findings: Most plausible hypothesis, confidence, rationale
- Citation block

Protocol: Follow 2009 Primer ACH 10-step + 02-analysis expert guide:
1. SETUP: Read problem framing, assumptions, evidence registry
2. PRIME: Explain purpose — "evaluate ALL hypotheses simultaneously; focus on disproving"
3. EXECUTE:
   - Frame problem as unbiased, open-ended inquiry
   - Brainstorm all plausible hypotheses (minimum 3; include null + deception/denial)
   - List all evidence including negative evidence (expected but absent)
   - Build matrix: hypotheses across top, evidence down side
   - Rate each cell: C (Consistent), I (Inconsistent), N (Neutral)
   - Apply Law of Diagnostic Dominance: mark evidence consistent with all hypotheses as "Zero diagnostic value"
   - Focus on DISPROVING — tally inconsistencies per hypothesis
   - Winner = hypothesis with LEAST inconsistent evidence (not most confirmed)
   - Sensitivity analysis: identify evidence that, if wrong, changes conclusion
   - Define future indicators/signposts
4. ARTIFACT: Write `working/ach-matrix.md`
5. FINDINGS: Most plausible hypothesis with confidence; sensitivity items
6. Layer 1 self-check: ≥3 hypotheses? Null included? Deception considered? Inconsistent evidence present? Zero-diagnostic items identified?
- Watch-outs: Initial hypothesis framing (if true answer not listed, matrix yields false winner), mechanical box-checking, ACH noise amplification (per Denzler 2024 — more cells ≠ better signal)
- Bias mitigated: Confirmation bias, premature closure, first-impression anchoring
- Effort: Hours to days (use Inconsistencies Finder for speed)

**Step 4: Write Inconsistencies Finder (Lean) template + protocol**

Template sections: header, lead hypothesis statement, contradiction search table (evidence ID, evidence, how it contradicts lead hypothesis, source, citation), assessment (does lead hypothesis survive?), alternative explanation suggested, citation block.

Protocol: Streamlined ACH from 04-agile-rigor:
1. SETUP: Read lead hypothesis (from prior analysis or conversation)
2. PRIME: "Focus ONLY on the lead hypothesis — try to prove it wrong"
3. EXECUTE:
   - State the lead hypothesis clearly
   - Search evidence specifically for items that CONTRADICT it
   - For each contradicting item: explain the inconsistency
   - Assess: does the lead hypothesis survive the contradiction scan?
   - If contradictions are strong: suggest the next-best hypothesis
4. ARTIFACT: Write `working/inconsistencies.md`
5. FINDINGS: Contradiction count and severity; lead hypothesis verdict
- Watch-outs: Confirmation bias (searching for confirming rather than contradicting evidence)
- Speed advantage: Bypasses noise amplification of large ACH matrices
- Bias mitigated: Confirmation bias
- Effort: 15-30 minutes

**Step 5: Write Cross-Impact Matrix template + protocol**

Template sections: header, variable inventory table (ID, variable, description, source), cross-impact matrix (variable x variable, effect direction +/-, magnitude H/M/L), second-order effects identified, key interaction chains, citation block.

Protocol:
1. SETUP: Read problem framing, brainstorm outputs, evidence
2. PRIME: "Identify how variables interact — find second-order effects"
3. EXECUTE:
   - List all key variables/factors from prior analysis
   - Build NxN matrix: for each pair, assess "if Variable A changes, how does it affect Variable B?"
   - Rate: positive/negative direction, High/Medium/Low magnitude
   - Identify reinforcing loops (A↑ → B↑ → A↑) and balancing loops
   - Surface second-order effects (chains of 2+ interactions)
4. ARTIFACT: Write `working/cross-impact.md`
5. FINDINGS: Critical interaction chains and non-obvious second-order effects
- Watch-outs: Linear thinking, missing indirect effects
- Bias mitigated: Linear thinking, failure to see second-order effects
- Effort: Hours

**Step 6: Verify and commit**

Run: `wc -w skills/structured-analysis/templates/techniques/brainstorm.md skills/structured-analysis/templates/techniques/assumptions.md skills/structured-analysis/templates/techniques/ach-matrix.md skills/structured-analysis/templates/techniques/inconsistencies.md skills/structured-analysis/templates/techniques/cross-impact.md`

Run: `wc -w skills/structured-analysis/protocols/techniques/structured-brainstorming.md skills/structured-analysis/protocols/techniques/key-assumptions-check.md skills/structured-analysis/protocols/techniques/ach.md skills/structured-analysis/protocols/techniques/inconsistencies-finder.md skills/structured-analysis/protocols/techniques/cross-impact-matrix.md`

```bash
git add skills/structured-analysis/templates/techniques/brainstorm.md skills/structured-analysis/templates/techniques/assumptions.md skills/structured-analysis/templates/techniques/ach-matrix.md skills/structured-analysis/templates/techniques/inconsistencies.md skills/structured-analysis/templates/techniques/cross-impact.md skills/structured-analysis/protocols/techniques/structured-brainstorming.md skills/structured-analysis/protocols/techniques/key-assumptions-check.md skills/structured-analysis/protocols/techniques/ach.md skills/structured-analysis/protocols/techniques/inconsistencies-finder.md skills/structured-analysis/protocols/techniques/cross-impact-matrix.md
git commit -m "feat: add diagnostic phase technique templates and protocols (5 techniques)"
```

---

### Task 6: Challenge & Foresight Phase Technique Templates + Protocols (4 techniques)

What If? Analysis, Premortem + Self-Critique, Counterfactual Reasoning, Contrasting Narratives.

**Files:**
- Create: `skills/structured-analysis/templates/techniques/what-if.md`
- Create: `skills/structured-analysis/templates/techniques/premortem.md`
- Create: `skills/structured-analysis/templates/techniques/counterfactual.md`
- Create: `skills/structured-analysis/templates/techniques/contrasting-narratives.md`
- Create: `skills/structured-analysis/protocols/techniques/what-if.md`
- Create: `skills/structured-analysis/protocols/techniques/premortem.md`
- Create: `skills/structured-analysis/protocols/techniques/counterfactual-reasoning.md`
- Create: `skills/structured-analysis/protocols/techniques/contrasting-narratives.md`
- Reference: `docs/library/01-tradecraft-primer-2009.md` (What If? method)
- Reference: `docs/library/04-agile-rigor-update.md` (Counterfactual Reasoning, Contrasting Narratives)
- Reference: `docs/library/09-core-techniques.md` (What If?, Premortem profiles)

**Step 1: Write What If? Analysis template + protocol**

Template sections: header, conventional analytic line (the prevailing view), assumed event (the "what if"), triggering events/catalysts, backward chain of argumentation (step-by-step causal path), plausibility assessment, indicators to monitor, scope of consequences, citation block.

Protocol: Follow 2009 Primer method:
1. SETUP: Read current analytical findings
2. PRIME: "Assume a high-impact event occurred — explain how it could happen"
3. EXECUTE:
   - State conventional line clearly
   - Define the "What If" event (the thing people say can't/won't happen)
   - Select triggering events that could initiate the chain
   - "Think backwards" — develop chain of argumentation from event to present
   - Identify plausible pathways
   - Generate indicator lists
   - Consider scope of consequences
4. ARTIFACT: Write `working/what-if.md`
5. FINDINGS: Plausibility of scenario, indicators to watch
- Watch-outs: Status quo bias, failure of imagination
- Bias mitigated: Status quo bias, failure of imagination
- Effort: Hours to days

**Step 2: Write Premortem + Self-Critique template + protocol**

Template sections: header, project/assessment being tested, failure assumption statement ("It is [future date] and this analysis was catastrophically wrong"), failure narrative (how it went wrong), weakness inventory table (weakness, severity, which technique missed it, recommended mitigation), structured self-critique questions and answers, citation block.

Protocol:
1. SETUP: Read all prior technique outputs and findings
2. PRIME: "Imagine this analysis has failed — find out why before it does"
3. EXECUTE:
   - State the assessment being tested
   - Assume failure: "It is [future date] and this analysis was completely wrong"
   - Write the failure story — what happened?
   - Inventory weaknesses: what did each technique miss?
   - Self-critique structured questions:
     - What assumption am I most uncertain about?
     - What evidence did I give too much weight?
     - What perspective is completely absent?
     - What would a fierce critic say about this analysis?
4. ARTIFACT: Write `working/premortem.md`
5. FINDINGS: Top 3-5 weaknesses with mitigations
- Watch-outs: Overconfidence, blind spots, reluctance to criticize own work
- Bias mitigated: Overconfidence, blind spots
- Effort: 30 minutes to 2 hours

**Step 3: Write Counterfactual Reasoning template + protocol**

Template sections: header, current state description, selected pivot point (past event/decision), the counterfactual (smallest possible change), traced effects (step-by-step consequences of the change), insights for current analysis ("what this reveals"), citation block.

Protocol: Follow 04-agile-rigor method:
1. SETUP: Read current findings and historical context
2. PRIME: "Look at what might have been to understand what is"
3. EXECUTE:
   - Identify the specific past event/decision driving current state
   - Posit the SMALLEST possible change to that event
   - Rigorously trace subsequent effects
   - Extract insights: what does this counterfactual reveal about current dynamics?
4. ARTIFACT: Write `working/counterfactual.md`
5. FINDINGS: Key insights about causal dynamics
- Use in disinformation: Deconstructs "historical inevitability" narratives
- Effort: Hours

**Step 4: Write Contrasting Narratives template + protocol**

Template sections: header, primary narrative (protagonist, antagonist, crisis, resolution, evidence basis), competing narrative(s) (same structural decomposition), evidence stress-test table (narrative element, supporting evidence, contradicting evidence, resilience rating), vulnerability assessment (elements with high emotional resonance but low factual support), pre-bunking strategies, citation block.

Protocol: Follow 04-agile-rigor method:
1. SETUP: Read problem context, evidence, any adversarial narratives
2. PRIME: "Analyze the STRUCTURE of competing stories, not just their claims"
3. EXECUTE:
   - Map primary narrative: Protagonist, Antagonist, Crisis, Resolution
   - Map competing/adversarial narrative(s): same structure
   - Stress-test each: where does it break against evidence?
   - Identify vulnerabilities: high emotional resonance + low factual support
   - Develop pre-bunking strategies for vulnerable narrative elements
4. ARTIFACT: Write `working/narratives.md`
5. FINDINGS: Narrative vulnerabilities and pre-bunking opportunities
- Optimized for "post-truth" environments
- Effort: Hours

**Step 5: Verify and commit**

```bash
git add skills/structured-analysis/templates/techniques/what-if.md skills/structured-analysis/templates/techniques/premortem.md skills/structured-analysis/templates/techniques/counterfactual.md skills/structured-analysis/templates/techniques/contrasting-narratives.md skills/structured-analysis/protocols/techniques/what-if.md skills/structured-analysis/protocols/techniques/premortem.md skills/structured-analysis/protocols/techniques/counterfactual-reasoning.md skills/structured-analysis/protocols/techniques/contrasting-narratives.md
git commit -m "feat: add challenge/foresight phase technique templates and protocols (4 techniques)"
```

---

### Task 7: Decision Support Phase Technique Templates + Protocols (2 techniques)

Bowtie Analysis, Opportunities Incubator.

**Files:**
- Create: `skills/structured-analysis/templates/techniques/bowtie.md`
- Create: `skills/structured-analysis/templates/techniques/opportunities.md`
- Create: `skills/structured-analysis/protocols/techniques/bowtie-analysis.md`
- Create: `skills/structured-analysis/protocols/techniques/opportunities-incubator.md`
- Reference: `docs/library/04-agile-rigor-update.md` (both techniques)

**Step 1: Write Bowtie Analysis template + protocol**

Template sections: header, top event (the catastrophic event being avoided), left side — causes table (threat, likelihood, preventative barriers), right side — consequences table (outcome, severity, reactive mitigations), escalation factors (conditions that weaken barriers), defensive posture assessment, citation block.

Protocol: Follow 04-agile-rigor method:
1. SETUP: Read problem context, identified risks
2. PRIME: "Visualize both sides of a risk — what causes it and what follows"
3. EXECUTE:
   - Define the Top Event (catastrophic event to prevent)
   - Left side: List causes/threats + preventative barriers for each
   - Right side: List consequences + reactive mitigations for each
   - Identify escalation factors that weaken barriers
   - Assess overall defensive posture
4. ARTIFACT: Write `working/bowtie.md`
5. FINDINGS: Weakest barriers, most critical escalation factors
- Use in cyber: Visualize how new vulnerabilities weaken which barriers
- Effort: Hours

**Step 2: Write Opportunities Incubator template + protocol**

Template sections: header, environmental scan results (emerging trends by domain), driver convergence table (trend A + trend B → opportunity, confidence, timeframe), windows of opportunity (opportunity, opening indicators, closing indicators, recommended action), citation block.

Protocol: Follow 04-agile-rigor method:
1. SETUP: Read problem context, evidence base, trend data from OSINT
2. PRIME: "Look for opportunities, not just threats"
3. EXECUTE:
   - Scan evidence for emerging trends (technology, economic, social, political)
   - Identify convergences where 2+ trends create windows
   - For each window: define opening indicators (it's starting) and closing indicators (window is shutting)
   - Recommend actions per window
4. ARTIFACT: Write `working/opportunities.md`
5. FINDINGS: Top opportunities with timing indicators
- Watch-outs: Threat fixation (intelligence bias toward threats over opportunities)
- Effort: Hours

**Step 3: Verify and commit**

```bash
git add skills/structured-analysis/templates/techniques/bowtie.md skills/structured-analysis/templates/techniques/opportunities.md skills/structured-analysis/protocols/techniques/bowtie-analysis.md skills/structured-analysis/protocols/techniques/opportunities-incubator.md
git commit -m "feat: add decision support phase technique templates and protocols (2 techniques)"
```

---

### Task 8: Evidence Collector Protocol

**Files:**
- Create: `skills/structured-analysis/protocols/evidence-collector.md`
- Reference: `docs/library/01-tradecraft-primer-2009.md` (Quality of Information Check)
- Reference: `docs/library/00-prime.md` Section 2 (Law of Diagnostic Dominance)

**Step 1: Write evidence collector protocol**

Create `skills/structured-analysis/protocols/evidence-collector.md`.

This protocol governs how the skill gathers, rates, and organizes evidence. Key content:

**Evidence Tiers:**
- Tier 1: Conversation context (extract from conversation history)
- Tier 2: Local files (read files user referenced; use Glob to discover relevant files)
- Tier 3: OSINT (parallel Task agents for web research)

**OSINT Collection Strategy:**
- Spawn 3 parallel Task agents via the Task tool:
  - Agent 1 prompt: "Research background and context for: {{PROBLEM_SUMMARY}}. Search for authoritative sources, data, expert analysis. Use Firecrawl MCP if available, otherwise WebSearch and WebFetch. Return findings with full source URLs and retrieval dates."
  - Agent 2 prompt: "Research contrarian and critical perspectives on: {{PROBLEM_SUMMARY}}. Specifically look for: counterarguments, failures, risks, critics' views, alternative explanations. Use Firecrawl MCP if available, otherwise WebSearch and WebFetch. Return findings with full source URLs and retrieval dates."
  - Agent 3 prompt: "Research recent developments (last 6 months) related to: {{PROBLEM_SUMMARY}}. Look for: new data, policy changes, market shifts, emerging trends, breaking news. Use Firecrawl MCP if available, otherwise WebSearch and WebFetch. Return findings with full source URLs and retrieval dates."

**Firecrawl MCP Integration:**
- Check if Firecrawl MCP server is available in the current session
- If available: Use Firecrawl for deep page scraping (extracts full article content, handles JavaScript-rendered pages)
- If not available: Fall back to WebSearch for discovery + WebFetch for content extraction
- Always note which method was used in the evidence registry

**Evidence Quality Rating (per 2009 Primer Quality of Information Check):**
- Source Reliability: High (established, track record) / Medium (known but unverified) / Low (unknown, potential bias)
- Information Credibility: High (corroborated, consistent) / Medium (plausible, partially confirmed) / Low (uncorroborated, contradictory)
- Diagnostic Value: High (contradicts specific hypotheses) / Medium (supports some, neutral for others) / Low (consistent with all — per Law of Diagnostic Dominance) / Zero (consistent with ALL hypotheses — explicitly flag)

**Citation Enforcement:**
- Every evidence item MUST include: source URL or file path, retrieval date, reliability rating
- OSINT: `[Source Title](URL)` with `Retrieved: YYYY-MM-DD`
- Files: `[filename:line_range]`
- User-provided: `[User-provided, session context]`
- OSINT is NEVER presented as fact — always "according to [source]"

**`--no-osint` flag behavior:**
- Skip Tier 3 entirely
- Note in evidence registry: "OSINT collection disabled by analyst"

**Step 2: Verify and commit**

Run: `wc -w skills/structured-analysis/protocols/evidence-collector.md`

```bash
git add skills/structured-analysis/protocols/evidence-collector.md
git commit -m "feat: add evidence collector protocol with OSINT and citation rules"
```

---

### Task 9: Report Generator Protocol

**Files:**
- Create: `skills/structured-analysis/protocols/report-generator.md`
- Reference: `skills/structured-analysis/templates/report-template.md`
- Reference: `skills/structured-analysis/templates/monitoring-plan-template.md`

**Step 1: Write report generator protocol**

Create `skills/structured-analysis/protocols/report-generator.md`.

This protocol governs how the skill synthesizes technique outputs into the final report and monitoring plan.

**Report Generation Steps:**
1. Read all working artifacts from `analyses/<id>/working/`
2. Read evidence registry from `analyses/<id>/evidence-registry.md`
3. Read meta file for technique execution order and modes

**Synthesis Instructions:**
- Cross-reference findings from ALL techniques executed
- Identify areas of agreement (techniques pointing to same conclusion)
- Identify areas of disagreement (techniques suggesting different conclusions)
- Resolve disagreements: explain which technique's methodology is more applicable and why
- Generate integrated assessment with overall confidence level

**Layer 2 Self-Critique (execute before writing report):**
1. Assumption Audit: Review all working artifacts for unstated premises in findings
2. Evidence Balance: Count evidence items supporting each hypothesis/finding; flag >2:1 imbalance
3. Confidence Calibration: Check for Bipolar Bias — are confidence levels proportional to evidence strength, or over/under-corrected?
4. Alternative Check: Write the strongest 2-3 sentence case AGAINST the top finding
5. Missing Voices: List domains/perspectives where no evidence was gathered
- Write results into report's "Risks & Blind Spots" section

**Monitoring Plan Generation:**
- Extract key judgments from report
- For each judgment: generate 2-3 confirming indicators and 2-3 disconfirming indicators
- Each indicator must be observable and have a specific source to check
- Set trigger thresholds: recommend re-analysis when ≥2 disconfirming indicators observed
- Initialize empty review log

**Human Review Gate (execute after draft report, before finalization):**
- Present consolidated summary in conversation:
  - Problem framing summary
  - Evidence count and source breakdown
  - Evidence gaps identified
  - Key finding with confidence level
  - Self-critique flags (from Layer 2)
- Ask: "What would you adjust before I finalize?"
- Incorporate user feedback into final report

**Final Write:**
- Write `report.md` using report template
- Write `monitoring-plan.md` using monitoring plan template
- Update `meta.md` with completion status
- Present summary in conversation with file paths

**Step 2: Verify and commit**

```bash
git add skills/structured-analysis/protocols/report-generator.md
git commit -m "feat: add report generator protocol with self-critique and human review gate"
```

---

### Task 10: Orchestrator Protocol

**Files:**
- Create: `skills/structured-analysis/protocols/orchestrator.md`
- Reference: `docs/library/00-prime.md` Section 5 (Selection Logic)
- Reference: `docs/library/06-decision-matrix.md` (full decision matrix)
- Reference: `docs/library/09-core-techniques.md` (core technique profiles)

**Step 1: Write orchestrator protocol**

Create `skills/structured-analysis/protocols/orchestrator.md`.

This is the "brain" of the skill — it handles mode routing, technique selection, and workflow orchestration.

**Mode Detection:**
- Parse arguments from skill invocation
- No args → Adaptive mode
- Technique name → Direct mode (map names: `ach`, `kac`, `brainstorm`, `what-if`, `premortem`, `bowtie`, `counterfactual`, `narratives`, `opportunities`, `cross-impact`, `inconsistencies`, `restatement`)
- `--guided` → Guided mode
- `--resume <id>` → Resume mode
- `--lean` → Set lean flag (abbreviates self-correction)
- `--no-osint` → Set no-osint flag (disables web research)

**Technique Routing Table:**
```
| Name (invocation) | Protocol File | Template File | Phase |
|---|---|---|---|
| customer-checklist | protocols/techniques/customer-checklist.md | templates/techniques/customer-checklist.md | Launch |
| issue-redefinition | protocols/techniques/issue-redefinition.md | templates/techniques/issue-redefinition.md | Launch |
| restatement | protocols/techniques/problem-restatement.md | templates/techniques/problem-restatement.md | Launch |
| brainstorm | protocols/techniques/structured-brainstorming.md | templates/techniques/brainstorm.md | Exploration |
| kac | protocols/techniques/key-assumptions-check.md | templates/techniques/assumptions.md | Diagnostic |
| ach | protocols/techniques/ach.md | templates/techniques/ach-matrix.md | Diagnostic |
| inconsistencies | protocols/techniques/inconsistencies-finder.md | templates/techniques/inconsistencies.md | Diagnostic |
| cross-impact | protocols/techniques/cross-impact-matrix.md | templates/techniques/cross-impact.md | Diagnostic |
| what-if | protocols/techniques/what-if.md | templates/techniques/what-if.md | Challenge |
| premortem | protocols/techniques/premortem.md | templates/techniques/premortem.md | Challenge |
| counterfactual | protocols/techniques/counterfactual-reasoning.md | templates/techniques/counterfactual.md | Foresight |
| narratives | protocols/techniques/contrasting-narratives.md | templates/techniques/contrasting-narratives.md | Foresight |
| bowtie | protocols/techniques/bowtie-analysis.md | templates/techniques/bowtie.md | Decision Support |
| opportunities | protocols/techniques/opportunities-incubator.md | templates/techniques/opportunities.md | Decision Support |
```

**Adaptive Mode Selection Logic (from prime Section 5):**

Step 1 — TRIGGER CHECK:
- Is this high-consequence? (significant risk, resource allocation, strategic implications)
- Is there persistent uncertainty? (incomplete, ambiguous, contradictory data)
- External scrutiny expected? (output briefed to stakeholders)
- Complex interaction? (multiple actors with competing interests)
- If NONE: suggest expert judgment + Problem Restatement; confirm with user
- If ANY: proceed to technique selection

Step 2 — STAGE CHECK:
- Map conversation context to appropriate family/phase
- New project with no analysis → Start with Launch (Customer Checklist + Issue Redefinition)
- Have data, need to evaluate → Diagnostic (KAC, ACH)
- Strong consensus exists → Reframing (What If?, Premortem)
- Need to model futures → Foresight (Counterfactual, Contrasting Narratives)
- Need to decide between options → Decision Support (Bowtie, Opportunities)

Step 3 — CHALLENGE CHECK (12-Question Rubric):
```
Large data volume to sort? → Brainstorming
Premises not explicit? → KAC
Multiple exclusive explanations? → ACH (or Inconsistencies Finder for lean)
High uncertainty, many variables? → Cross-Impact Matrix
Fear of surprise? → What If? Analysis
Need adversary perspective? → (not in v1 — flag for future)
Groupthink/dominant mindset? → Premortem + Self-Critique
Find opportunities? → Opportunities Incubator
Risk of deception? → (not in v1 — flag for future)
Track over time? → Include Monitoring Plan generation
Competing options? → Bowtie Analysis
```

Step 4 — EFFORT CHECK:
- `--lean` flag → Use only: Problem Restatement + KAC Quick + Inconsistencies Finder
- Default → Full technique set as selected by rubric
- Present recommendation to user: "Based on the problem, I recommend: [technique list with rationale]. Shall I proceed or adjust?"

**Guided Mode Phases:**
1. Framing: Customer Checklist → Issue Redefinition
2. Assumptions: KAC
3. Evidence: Run evidence-collector protocol
4. Core Analysis: Selection logic picks technique(s)
5. Stress Test: Premortem + What If?
6. Report: Run report-generator protocol

**Resume Mode:**
- Read `analyses/<id>/meta.md`
- If incomplete: continue from last completed technique
- If complete: read `analyses/<id>/monitoring-plan.md`, run fresh OSINT against indicators, update status column and review log

**Analysis Directory Setup:**
- At the start of any analysis, create: `analyses/YYYY-MM-DD-<slug>/working/`
- Generate analysis ID from date + slugified problem title
- Write initial `meta.md`

**Step 2: Verify and commit**

Run: `wc -w skills/structured-analysis/protocols/orchestrator.md`

```bash
git add skills/structured-analysis/protocols/orchestrator.md
git commit -m "feat: add orchestrator protocol with selection logic and mode routing"
```

---

### Task 11: Master Skill File (analyze.md)

**Files:**
- Modify: `skills/structured-analysis/analyze.md` (replace placeholder from Task 1)
- Reference: All protocol files in `skills/structured-analysis/protocols/`
- Reference: `docs/library/00-prime.md` (for background context)

**Step 1: Write the master skill file**

Replace the placeholder `skills/structured-analysis/analyze.md` with the full skill.

**Frontmatter:**
```yaml
---
name: analyze
description: Use when conducting structured analysis on any problem — assessing competing hypotheses, challenging assumptions, stress-testing judgments, or producing defensible analytical assessments with evidence and citations
---
```

**Body structure (this is what Claude reads when `/analyze` is invoked):**

1. **Skill header and overview** — brief description of what this skill does and how it works
2. **Invocation patterns** — how to invoke (default, direct technique, --guided, --resume, --lean, --no-osint)
3. **Instruction to read orchestrator** — "Read `protocols/orchestrator.md` relative to this skill's directory for mode routing and selection logic"
4. **Execution flow** — high-level steps:
   - Parse args → determine mode
   - Read orchestrator protocol → execute mode-appropriate selection
   - Create analysis directory under `analyses/`
   - For each selected technique: read protocol → read template → execute → write artifact
   - Evidence collection: read evidence-collector protocol → spawn parallel agents
   - Report synthesis: read report-generator protocol → self-critique → human gate → write report
5. **File reading instructions** — how to locate protocol and template files relative to the skill directory
6. **Self-correction summary** — Layer 1 (protocol compliance, after each technique), Layer 2 (analytical self-critique, before report), Layer 3 (human review gate, single checkpoint at end)
7. **Citation requirement** — "Every claim in every artifact must be cited. No exceptions."
8. **Background reference** — "For deep reference on any technique, read `docs/library/00-prime.md` and the specific library files referenced in each protocol."

**Key design constraint:** This file should be **concise** — under 500 words ideally. All detailed logic lives in the protocol files. The master skill is a dispatcher, not an encyclopedia.

**Step 2: Verify word count**

Run: `wc -w skills/structured-analysis/analyze.md`

Target: Under 500 words. If over, move content to protocol files.

**Step 3: Commit**

```bash
git add skills/structured-analysis/analyze.md
git commit -m "feat: implement master analyze.md skill file"
```

---

### Task 12: Skill Installation and Integration Test

**Step 1: Install the skill**

Create a symlink from the skill directory to the Claude Code skills directory:

```bash
ln -sf /home/angel/Documents/code/structured-analysis-skill/skills/structured-analysis /home/angel/.claude/skills/structured-analysis
```

Verify: `ls -la /home/angel/.claude/skills/structured-analysis/analyze.md`

Expected: Symlink pointing to the project's skill directory.

**Step 2: Verify skill discovery**

The skill should now appear in Claude Code's skill list. The description should trigger on analysis-related requests.

**Step 3: Smoke test — Direct mode**

In a new Claude Code session, test direct technique invocation:

```
/analyze kac

I'm evaluating whether to migrate our monolithic application to microservices.
The current system handles 10K requests/day and the team is 5 engineers.
```

Expected behavior:
- Skill loads `analyze.md`
- Detects direct mode with `kac` technique
- Reads KAC protocol
- Executes Key Assumptions Check against the stated problem
- Produces `working/assumptions.md` artifact
- Presents findings with citations

**Step 4: Smoke test — Adaptive mode**

```
/analyze

We're seeing a 30% decline in user engagement over the past quarter.
Multiple theories exist: product fatigue, new competitor entry, pricing changes,
and seasonal variation. We need to determine the most likely cause and recommend action.
```

Expected behavior:
- Skill loads `analyze.md`
- Detects adaptive mode
- Reads orchestrator → runs selection logic
- Should recommend: Issue Redefinition → KAC → ACH (multiple hypotheses) → Premortem
- Asks user to confirm technique selection
- Executes full workflow with evidence gathering
- Produces full artifact suite
- Human review gate at end
- Final report + monitoring plan

**Step 5: Verify artifacts**

Check `analyses/` directory for generated files:

```bash
find analyses/ -type f | sort
```

Expected: report.md, monitoring-plan.md, evidence-registry.md, meta.md, working/*.md

**Step 6: Final commit**

```bash
git add -A
git commit -m "feat: complete structured analysis skill v1 with installation"
```

---

## Task Summary

| Task | Description | Files | Commit |
|------|-------------|-------|--------|
| 1 | Directory structure | 1 placeholder + directories | scaffold commit |
| 2 | Section templates | 4 files | section templates commit |
| 3 | Infrastructure templates | 4 files | infrastructure templates commit |
| 4 | Launch phase (3 techniques) | 6 files (3 templates + 3 protocols) | launch phase commit |
| 5 | Diagnostic phase (5 techniques) | 10 files (5 templates + 5 protocols) | diagnostic phase commit |
| 6 | Challenge/Foresight phase (4 techniques) | 8 files (4 templates + 4 protocols) | challenge phase commit |
| 7 | Decision Support phase (2 techniques) | 4 files (2 templates + 2 protocols) | decision support commit |
| 8 | Evidence collector protocol | 1 file | evidence collector commit |
| 9 | Report generator protocol | 1 file | report generator commit |
| 10 | Orchestrator protocol | 1 file | orchestrator commit |
| 11 | Master skill file | 1 file (modify) | master skill commit |
| 12 | Installation + integration test | symlink + manual test | final commit |

**Total: 40 files across 12 tasks with 12 commits**
