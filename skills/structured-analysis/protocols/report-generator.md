# Report Generator Protocol

Synthesize all technique outputs into the final report and monitoring plan. Execute Layer 2 self-critique before writing.

---

## Step 1: Gather Inputs

Read from the analysis directory (`analyses/{{ANALYSIS_ID}}/`):
- All working artifacts in `working/`
- `evidence-registry.md`
- `meta.md` for technique execution order and modes

---

## Step 2: Cross-Technique Synthesis

For every technique that produced findings:
1. Extract key findings with confidence levels
2. Identify **agreement** — techniques pointing to the same conclusion
3. Identify **disagreement** — techniques suggesting different conclusions
4. Resolve disagreements: explain which technique's methodology is more applicable to this specific problem and why
5. Generate an integrated assessment with overall confidence level

---

## Step 3: Layer 2 Self-Critique

Execute ALL five checks before writing the report. Results go into the "Risks & Blind Spots" section.

### 3a. Assumption Audit
Review all working artifacts for unstated premises embedded in findings. List each with its source artifact.

### 3b. Evidence Balance
Count evidence items supporting each hypothesis or finding. Flag any imbalance exceeding 2:1. Ask: is the imbalance justified by evidence quality, or does it reflect collection bias?

### 3c. Confidence Calibration
Check for Bipolar Bias — are confidence levels proportional to actual evidence strength, or over/under-corrected? Analysts tend to cluster at High or Low while avoiding Moderate.

### 3d. Alternative Check
Write the strongest 2-3 sentence case AGAINST the top finding. If you cannot construct a credible counter-argument, the finding may be trivially true (and therefore low-value). If the counter-argument is strong, the confidence level may need lowering.

### 3e. Missing Voices
List domains, perspectives, or stakeholder viewpoints where no evidence was gathered. These are blind spots, not necessarily errors.

---

## Step 4: Human Review Gate

Present a consolidated summary in conversation BEFORE finalizing:

```
## Analysis Summary for Review

**Problem**: {{REFRAMED_PROBLEM}}
**Evidence**: {{EVIDENCE_COUNT}} items ({{TIER_BREAKDOWN}})
**Evidence Gaps**: {{GAPS_IDENTIFIED}}
**Key Finding**: {{TOP_FINDING}} (Confidence: {{LEVEL}})
**Self-Critique Flags**: {{LAYER_2_FLAGS}}

What would you adjust before I finalize?
```

Incorporate user feedback into the final report. If no feedback, proceed.

---

## Step 5: Write Final Artifacts

### Report (`report.md`)
Use `templates/report-template.md`. Fill every section:
- Header with analysis metadata
- Executive Summary (2-3 paragraphs: key findings, confidence levels, primary recommendation)
- Problem Framing (how defined, restatements considered, final framing)
- Evidence Base (summary with source breakdown, quality assessment, link to registry)
- Analysis sections — one per technique applied (purpose, process, findings with citations, confidence, link to working artifact)
- Synthesis (cross-technique agreement/disagreement, integrated assessment)
- Key Judgments table (using `sections/judgment-table.md` format)
- Risks & Blind Spots (all Layer 2 self-critique results)
- Monitoring Plan summary with link to full plan
- Complete Sources bibliography

### Monitoring Plan (`monitoring-plan.md`)
Use `templates/monitoring-plan-template.md`. For each key judgment:
- Generate 2-3 confirming indicators (what to look for if judgment is correct)
- Generate 2-3 disconfirming indicators (what to look for if judgment is wrong)
- Each indicator must be observable and have a specific source to check
- Set trigger threshold: recommend re-analysis when ≥2 disconfirming indicators observed
- Initialize empty review log

### Meta Update (`meta.md`)
Update with completion status, final technique list, and file manifest.

---

## Step 6: Present Results

In conversation, provide:
- One-paragraph summary of the key finding with confidence
- File paths to all generated artifacts
- Monitoring plan highlights (top 2-3 indicators to watch)
- Invitation: "Would you like me to deep-dive into any section or adjust the analysis?"

---

## Step 7: Iteration Suggestions

**Guard**: Only execute this step if Layer 1 (Sufficiency Gate) or Layer 2 (self-critique) produced at least one actionable flag. If no flags exist, skip this step entirely — do not print "no suggestions."

### 7a. Collect Flags

Read flags from:
- **Layer 1**: Sufficiency Gate soft-check warnings in `evidence-registry.md` (LOW SOURCE DIVERSITY, LOW DIAGNOSTIC POWER, STALE EVIDENCE)
- **Layer 2**: Self-critique results from Step 3 above (3a–3e checks)

### 7b. Map Flags to Iterations

Use this mapping table to convert each actionable flag into a concrete iteration suggestion:

| Flag Source | Flag Type | Suggested Technique(s) | Evidence Focus |
|---|---|---|---|
| Sufficiency Gate | LOW SOURCE DIVERSITY | *(evidence collection only)* | Broaden source types |
| Sufficiency Gate | LOW DIAGNOSTIC POWER | `ach`, `inconsistencies` | Seek evidence that discriminates between hypotheses |
| Sufficiency Gate | STALE EVIDENCE | *(evidence collection only)* | Focus on last 6 months |
| Layer 2 - 3a | Unstated premises found | `kac` | Collect evidence testing the flagged assumptions |
| Layer 2 - 3b | Evidence imbalance >2:1 | `ach` | Seek sources for the underrepresented side |
| Layer 2 - 3c | Bipolar bias detected | *(confidence recalibration — no re-run)* | N/A |
| Layer 2 - 3d | Strong counter-argument | `what-if`, `premortem` | Collect evidence on the counter-argument scenario |
| Layer 2 - 3e | Missing perspectives | `narratives` | Collect evidence from the missing viewpoint |

**Rules**:
- Bipolar bias (3c) is noted in the report but NOT mapped to an iteration — it's an in-report confidence adjustment, not a re-run trigger.
- Evidence-only flags (LOW SOURCE DIVERSITY, STALE EVIDENCE) suggest re-running evidence collection without technique re-runs.
- Deduplicate techniques — if multiple flags map to the same technique, combine their evidence focus descriptions.

### 7c. Present Suggestions

Append to the conversation output (after Step 6) using this format:

```
## Iteration Suggestions

Self-critique identified {{N}} actionable items:

1. **{{FLAG_TYPE}}**: {{FLAG_DESCRIPTION}}
   → Re-run: {{TECHNIQUE(S)}} | Evidence focus: {{FOCUS}}

2. ...

Suggested command:
/analyze --iterate {{ANALYSIS_ID}} {{TECHNIQUE_LIST}}
```

- `{{N}}` = count of actionable flags (excluding 3c bipolar bias)
- `{{TECHNIQUE_LIST}}` = deduplicated, space-separated list of all suggested techniques
- If only evidence-collection flags exist (no technique re-runs), omit the technique list from the command and note that re-running evidence collection is the primary action
- Each numbered item corresponds to one flag with its mapped technique(s) and evidence focus
