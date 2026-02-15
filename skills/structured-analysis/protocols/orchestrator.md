# Orchestrator Protocol

Route modes, select techniques, and manage the analysis workflow.

---

## Mode Detection

Parse the skill invocation arguments:

| Input | Mode |
|-------|------|
| No args | **Adaptive** — auto-select techniques |
| Technique name (e.g., `ach`, `kac`) | **Direct** — run single technique |
| `--guided` | **Guided** — walk through all phases |
| `--resume <id>` | **Resume** — continue or update existing analysis |
| `--iterate <id>` | **Iterate** — re-run with artifact versioning and evidence delta |
| `--iterate <id> <technique>` | **Iterate (scoped)** — re-run specific technique(s) only |
| `--lean` | **Lean** — Problem Restatement + KAC + Inconsistencies only (overrides rubric) |
| `--comprehensive` | **Comprehensive** — full 14-question rubric, no technique cap, unlocks adversarial + deception checks |
| `--no-osint` | Flag — disable web research |

Flags combine with modes: `--guided --no-osint` is valid.

### Mode Flag Conflict Resolution

| Combination | Resolution |
|---|---|
| `--lean` + `--comprehensive` | ERROR: conflicting effort levels — prompt user to choose |
| `--lean` + `--guided` | Lean overrides technique selection; guided phases still execute |
| `--comprehensive` + `--guided` | Both apply: full rubric + phased execution |
| `--no-osint` + any mode | OSINT disabled; all modes respect this |

---

## Technique Routing Table

| Invocation Name | Protocol File | Template File | Phase |
|---|---|---|---|
| `customer-checklist` | `protocols/techniques/customer-checklist.md` | `templates/techniques/customer-checklist.md` | Launch |
| `issue-redefinition` | `protocols/techniques/issue-redefinition.md` | `templates/techniques/issue-redefinition.md` | Launch |
| `restatement` | `protocols/techniques/problem-restatement.md` | `templates/techniques/problem-restatement.md` | Launch |
| `brainstorm` | `protocols/techniques/structured-brainstorming.md` | `templates/techniques/brainstorm.md` | Exploration |
| `kac` | `protocols/techniques/key-assumptions-check.md` | `templates/techniques/assumptions.md` | Diagnostic |
| `ach` | `protocols/techniques/ach.md` | `templates/techniques/ach-matrix.md` | Diagnostic |
| `inconsistencies` | `protocols/techniques/inconsistencies-finder.md` | `templates/techniques/inconsistencies.md` | Diagnostic |
| `cross-impact` | `protocols/techniques/cross-impact-matrix.md` | `templates/techniques/cross-impact.md` | Diagnostic |
| `what-if` | `protocols/techniques/what-if.md` | `templates/techniques/what-if.md` | Challenge |
| `premortem` | `protocols/techniques/premortem.md` | `templates/techniques/premortem.md` | Challenge |
| `counterfactual` | `protocols/techniques/counterfactual-reasoning.md` | `templates/techniques/counterfactual.md` | Foresight |
| `narratives` | `protocols/techniques/contrasting-narratives.md` | `templates/techniques/contrasting-narratives.md` | Foresight |
| `devils-advocacy` | `protocols/techniques/devils-advocacy.md` | `templates/techniques/devils-advocacy.md` | Challenge |
| `red-hat` | `protocols/techniques/red-hat-analysis.md` | `templates/techniques/red-hat-analysis.md` | Challenge |
| `bowtie` | `protocols/techniques/bowtie-analysis.md` | `templates/techniques/bowtie.md` | Decision Support |
| `opportunities` | `protocols/techniques/opportunities-incubator.md` | `templates/techniques/opportunities.md` | Decision Support |
| `alt-futures` | `protocols/techniques/alternative-futures.md` | `templates/techniques/alternative-futures.md` | Foresight |
| `deception` | `protocols/techniques/deception-detection.md` | `templates/techniques/deception-detection.md` | Diagnostic |

All paths are relative to the skill directory (`skills/structured-analysis/`).

---

## Analysis Directory Setup

At the start of ANY analysis:

1. Generate analysis ID: `YYYY-MM-DD-<slugified-problem-title>`
2. Create directories:
   - `analyses/{{ANALYSIS_ID}}/working/` — technique artifacts
   - `analyses/{{ANALYSIS_ID}}/working/osint-raw/` — raw scraped content (Step 3a output)
   - `analyses/{{ANALYSIS_ID}}/working/osint-processed/` — extracted evidence items (Step 3b output)
3. Write initial `meta.md` using `templates/meta-template.md`
4. Note the analysis directory path — all artifacts go here

---

## Adaptive Mode Selection Logic

### Step 1 — Trigger Check

Ask these questions about the problem:
- Is this high-consequence? (significant risk, resource allocation, strategic implications)
- Is there persistent uncertainty? (incomplete, ambiguous, contradictory data)
- External scrutiny expected? (output briefed to stakeholders)
- Complex interaction? (multiple actors with competing interests)

If **NONE** apply: suggest expert judgment + Problem Restatement only. Confirm with user.
If **ANY** apply: proceed to technique selection.

### Step 2 — Stage Check

Map the conversation context to the appropriate phase:

| Situation | Start With |
|-----------|-----------|
| New project, no analysis yet | Launch → Customer Checklist + Issue Redefinition |
| Have data, need to evaluate | Diagnostic → KAC, ACH |
| Strong consensus exists | Challenge → What If?, Premortem |
| Need to model futures | Foresight → Counterfactual, Contrasting Narratives |
| Need to decide between options | Decision Support → Bowtie, Opportunities |

### Step 3 — Challenge Check (14-Question Rubric)

Questions 1–11 are always active. Questions 12–14 activate in `--comprehensive` mode.

| # | Condition | Primary Technique | Substitutes | Complements | Threshold |
|---|-----------|-------------------|-------------|-------------|-----------|
| 1 | Large data volume to sort? | Structured Brainstorming | — | feeds → KAC, ACH | — |
| 2 | Premises not explicit? | Key Assumptions Check | — | feeds → ACH, Devil's Advocacy | — |
| 3 | Multiple exclusive explanations? | ACH | Inconsistencies Finder (≤2 hyp) | ← KAC; → Deception Detection | ≤2 hyp → Inconsistencies; 3+ → ACH |
| 4 | High uncertainty, many variables? | Cross-Impact Matrix | — | + Alternative Futures (if forecasting) | — |
| 5 | Fear of surprise? | What If? Analysis | — | + Premortem (if existing judgment) | — |
| 6 | Groupthink or dominant mindset? | Premortem + Self-Critique | — | + Devil's Advocacy (stronger challenge) | — |
| 7 | Need to find opportunities? | Opportunities Incubator | — | — | — |
| 8 | Competing options to evaluate? | Bowtie Analysis | — | — | — |
| 9 | Competing narratives in play? | Contrasting Narratives | — | + Deception Detection (if adversarial) | — |
| 10 | Need to understand causal dynamics? | Counterfactual Reasoning | — | — | — |
| 11 | Need to track over time? | Include Monitoring Plan | — | — | — |
| 12 | Multiple plausible futures? ★ | Alternative Futures | — | feeds → Monitoring Plan | — |
| 13 | Need to model adversary reaction? ★ | Red Hat Analysis | — | + Devil's Advocacy | — |
| 14 | Risk of intentional deception? ★ | Deception Detection | — | revises → ACH/Inconsistencies | — |

★ = Comprehensive mode only

**Reading the interaction columns:**
- **Substitutes**: Use INSTEAD of primary when threshold applies (e.g., Inconsistencies Finder replaces ACH for ≤2 hypotheses)
- **Complements**: Use IN ADDITION to primary (e.g., Devil's Advocacy adds to Premortem when both Q2+Q6 match). Complements are recommendations, not automatic additions.
- **feeds →**: Output of this technique is input to the next (prerequisite relationship)
- **+ technique**: Consider running alongside (reinforcing relationship)
- **revises →**: Output may require re-running a prior technique

### Step 4 — Effort Check & Technique Cap

**Lean Mode** (`--lean`):
- Use ONLY Problem Restatement + KAC Quick + Inconsistencies Finder
- Overrides all rubric selections — no exceptions

**Default Mode** (no effort flag):
- Questions 1–11 active; max 5 techniques
- Prioritization when >5 match:
  1. Phase alignment (prefer techniques matching Stage Check result)
  2. Diagnostic before Challenge (test assumptions before stress-testing)
  3. Essential 8 before Extended techniques
- If >5 match, inform user: "I identified [N] matching techniques. Recommending [5 prioritized]. Use --comprehensive to run all."

**Comprehensive Mode** (`--comprehensive`):
- Full 14-question rubric (adds Q12–14: adversary reaction, deception risk, alternative futures)
- No technique cap — all matching techniques execute
- Complements from interaction model are promoted to selections (not just suggestions)

Present recommendation with rationale. Wait for user confirmation.

### No-Match Fallback

If zero Challenge Check questions match (or user requests minimal structure):

1. Problem Restatement (Launch)
2. Key Assumptions Check (Diagnostic)
3. Evidence collection
4. Inconsistencies Finder (≤2 hypotheses) or ACH (3+)
5. Premortem (Challenge)
6. Report with low-confidence flag

This ensures minimum viable rigor for edge cases.

---

## Guided Mode Phases

Execute in order, each phase building on the previous:

1. **Framing**: Customer Checklist → Issue Redefinition
2. **Assumptions**: Key Assumptions Check
3. **Evidence**: Read and execute `protocols/evidence-collector.md`
4. **Evidence Gate**: Execute the Evidence Sufficiency Gate (defined in `protocols/evidence-collector.md`). If hard checks fail, retry or halt. If soft checks fail, log flags and proceed.
5. **Core Analysis**: Use selection logic (Steps 2-3 above) to pick technique(s)
6. **Stress Test**: Premortem + What If?
7. **Report**: Read and execute `protocols/report-generator.md`

---

## Direct Mode

1. Look up technique in the routing table
2. Create analysis directory
3. If OSINT not disabled, run evidence collector first
4. Execute Evidence Sufficiency Gate; if hard checks fail, retry or surface to analyst before proceeding
5. Read the technique's protocol file
6. Read the technique's template file
7. Execute the protocol, writing the artifact using the template
8. Present findings in conversation
9. Offer: "Would you like me to continue with a full analysis, or is this technique sufficient?"

---

## Resume Mode

1. Read `analyses/<id>/meta.md`
2. If analysis is **incomplete**: continue from last completed technique
3. If analysis is **complete**: read `analyses/<id>/monitoring-plan.md`, run fresh OSINT against indicators, update indicator status column and review log

---

## Iterate Mode

When `--iterate <id>` is detected (with or without a technique specifier):

1. Read the existing analysis at `analyses/<id>/`
2. Read `protocols/iteration-handler.md` and follow its steps in order

### Full Iteration (`--iterate <id>`)

Execute the complete iteration workflow:

1. **Detect iteration number** — iteration-handler Step 1
2. **Collect new evidence** — re-run OSINT via `protocols/evidence-collector.md` (unless `--no-osint`), iteration-handler Step 3
3. **Evidence Gate** — evaluate only the evidence delta (newly collected items), not the full accumulated registry. Techniques execute against the combined evidence base (prior + new).
4. **Archive ALL working artifacts** — iteration-handler Step 2
5. **Re-run all techniques** — use the same technique set from `meta.md`, iteration-handler Step 4
6. **Cross-iteration synthesis** — iteration-handler Step 5, comparing new vs. prior findings
7. **Regenerate report** — include "Revision History" section from `templates/report-template.md`
8. **Update monitoring plan** — refresh indicators based on new findings
9. **Write iteration metadata** — iteration-handler Step 6
10. **Update meta.md** — iteration-handler Step 7
11. **Present delta summary** — show the analyst what changed, what remained stable, and what drove revisions

### Scoped Iteration (`--iterate <id> <technique>`)

Execute a targeted re-run of specific technique(s):

1. **Detect iteration number** — iteration-handler Step 1
2. **Optionally collect targeted evidence** — if the iteration trigger specifies a collection focus (e.g., "counter-evidence for pessimistic bias"), run evidence collection with that focus. Otherwise, skip.
3. **Evidence Gate** — evaluate only the evidence delta if new evidence was collected
4. **Archive only the specified technique's artifact(s)** — iteration-handler Step 2 (scoped)
5. **Re-run the specified technique(s)** — iteration-handler Step 4
6. **Cross-iteration synthesis** — compare new vs. prior findings for the re-run technique(s) only
7. **Write iteration findings summary** — in iteration metadata, NOT a full report regeneration (unless explicitly requested by the analyst)
8. **Update meta.md** — iteration-handler Step 7
9. **Present delta summary** — show what changed for the re-run technique(s)

---

## Evidence Gate (All Modes)

After evidence collection and before any technique execution, run the **Evidence Sufficiency Gate** defined in `protocols/evidence-collector.md`. This applies to Adaptive, Guided, and Direct modes.

- If hard checks fail: retry evidence collection or surface to analyst
- If soft checks fail: log flags in `meta.md` under Self-Correction > Layer 1 Flags, then proceed
- The Evidence Sufficiency Report should be included in the orchestrator's handoff to each technique

**Iteration Note**: In iterate mode, the sufficiency gate evaluates only the
evidence delta (newly collected items), not the full accumulated registry.
Techniques execute against the combined evidence base (prior + new).

---

## Technique Execution Contract

For every technique, follow this sequence:

1. **Read** the protocol file from the routing table
2. **Read** the template file from the routing table
3. **Execute** the protocol's SETUP → PRIME → EXECUTE → ARTIFACT → FINDINGS → HANDOFF steps
4. **Write** the artifact to `analyses/{{ANALYSIS_ID}}/working/{{ARTIFACT_NAME}}.md`
5. **Layer 1 Check** (silent): Did the protocol complete all required steps? Are all template sections filled? Any {{PLACEHOLDER}} tokens remaining unfilled?
6. If Layer 1 fails: re-execute the missed steps before proceeding
7. **Update** `meta.md` with technique completion status
