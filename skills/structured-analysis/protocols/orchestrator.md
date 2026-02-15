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
| `--lean` | **Lean** — abbreviated technique set |
| `--no-osint` | Flag — disable web research |

Flags combine with modes: `--guided --no-osint` is valid.

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
| `bowtie` | `protocols/techniques/bowtie-analysis.md` | `templates/techniques/bowtie.md` | Decision Support |
| `opportunities` | `protocols/techniques/opportunities-incubator.md` | `templates/techniques/opportunities.md` | Decision Support |

All paths are relative to the skill directory (`skills/structured-analysis/`).

---

## Analysis Directory Setup

At the start of ANY analysis:

1. Generate analysis ID: `YYYY-MM-DD-<slugified-problem-title>`
2. Create directory: `analyses/{{ANALYSIS_ID}}/working/`
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

### Step 3 — Challenge Check (12-Question Rubric)

| Condition | Technique |
|-----------|-----------|
| Large data volume to sort? | Structured Brainstorming |
| Premises not explicit? | Key Assumptions Check |
| Multiple exclusive explanations? | ACH (or Inconsistencies Finder if lean) |
| High uncertainty, many variables? | Cross-Impact Matrix |
| Fear of surprise? | What If? Analysis |
| Groupthink or dominant mindset? | Premortem + Self-Critique |
| Need to find opportunities? | Opportunities Incubator |
| Competing options to evaluate? | Bowtie Analysis |
| Competing narratives in play? | Contrasting Narratives |
| Need to understand causal dynamics? | Counterfactual Reasoning |
| Need to track over time? | Include Monitoring Plan generation |

### Step 4 — Effort Check

- If `--lean` flag: Use ONLY Problem Restatement + KAC Quick + Inconsistencies Finder
- Default: Full technique set as selected by rubric
- Present recommendation: "Based on the problem, I recommend: [technique list with rationale]. Shall I proceed or adjust?"
- Wait for user confirmation before executing

---

## Guided Mode Phases

Execute in order, each phase building on the previous:

1. **Framing**: Customer Checklist → Issue Redefinition
2. **Assumptions**: Key Assumptions Check
3. **Evidence**: Read and execute `protocols/evidence-collector.md`
4. **Core Analysis**: Use selection logic (Steps 2-3 above) to pick technique(s)
5. **Stress Test**: Premortem + What If?
6. **Report**: Read and execute `protocols/report-generator.md`

---

## Direct Mode

1. Look up technique in the routing table
2. Create analysis directory
3. If OSINT not disabled, run evidence collector first
4. Read the technique's protocol file
5. Read the technique's template file
6. Execute the protocol, writing the artifact using the template
7. Present findings in conversation
8. Offer: "Would you like me to continue with a full analysis, or is this technique sufficient?"

---

## Resume Mode

1. Read `analyses/<id>/meta.md`
2. If analysis is **incomplete**: continue from last completed technique
3. If analysis is **complete**: read `analyses/<id>/monitoring-plan.md`, run fresh OSINT against indicators, update indicator status column and review log

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
