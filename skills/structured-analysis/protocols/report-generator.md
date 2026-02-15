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

### 2b: Analytical Framework (Technique Weights)

After synthesis, assign each technique a weight reflecting its influence on the key judgments:

| Column | Values | Guidance |
|--------|--------|----------|
| **Role** | Diagnostic, Challenge, Foresight, Decision Support | Use the technique's Phase from the routing table |
| **Weight** | Primary, Supporting, Qualifying | Primary = technique directly shaped 2+ key judgments. Supporting = reinforced or refined primary findings. Qualifying = introduced caveats, conditions, or confidence adjustments without changing the direction of findings. |
| **Direction** | Confirming, Challenging, Mixed | Confirming = findings align with the emerging consensus across techniques. Challenging = findings push against it. Mixed = some findings confirm, others challenge. |
| **Key Contribution** | 1 sentence | The single most important thing this technique added to the assessment. |

**Assignment rules**:
- At least one technique must be weighted Primary (the analysis must have a backbone)
- Challenge-phase techniques (Premortem, What If?, Devil's Advocacy) are typically Qualifying or Challenging — if a challenge technique confirms, note that explicitly as it strengthens confidence
- If all techniques are Confirming, flag this in the Confidence Calibration check (potential groupthink)
- Weights are qualitative labels for reader orientation, not numeric scores — do not assign percentages

---

## Step 3: Layer 2 Self-Critique

Execute ALL eight checks before writing the report. Results from 3a–3g go into the "Risks & Blind Spots" section. Results from 3h are logged to `meta.md`.

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

### 3f. Internal Consistency Audit
Cross-reference validation across generated artifacts. Flag discrepancies and explain measurement differences where applicable:
- Evidence registry diagnostic values vs ACH diagnosticity ratings (these measure different things — flag mismatches, explain the difference)
- Hypothesis rankings consistent between ACH matrix, synthesis section, executive summary, and key judgments table
- Confidence levels consistent across technique findings and the integrated assessment
- Evidence IDs referenced in technique artifacts actually exist in the evidence registry
- Negative evidence items properly dual-rated (factual reliability vs interpretive reliability)

### 3g. Analytical Bias Scan
Check for known cognitive biases in the assessment:
- **Sycophancy**: Does the assessment tell the user what they want to hear? If the user-provided hypothesis is confirmed, apply extra scrutiny to the confirming evidence chain.
- **Completion bias**: Is quality conflated with thoroughness? Running many techniques does not strengthen a conclusion — only discriminating evidence does.
- **Authority bias**: Are high-reliability sources given disproportionate weight over diagnostic value? A reliable source with low diagnostic power should not outweigh a less established source with high diagnostic power.
- **Anchoring**: Does the original hypothesis framing dominate the assessment despite problem restatement? Compare the original framing to the reframed problem — if findings track the original framing more closely, anchoring may be present.
- **Vividness bias**: Are emotionally compelling narratives (e.g., "front company," "state-sponsored") given more weight than their evidence supports? Flag vivid framings and verify their evidentiary basis independently.

### 3h. Quality Score
Quantitative self-assessment. Compute silently; log results to `meta.md` under the Quality Score section.

| Criterion | Base Weight | What to Assess | Requires |
|-----------|-------------|----------------|----------|
| Evidence discrimination | 0.30 | What % of evidence items have non-zero diagnostic value in ACH? | ACH |
| Hypothesis separation | 0.25 | How many inconsistencies separate the top-ranked from bottom-ranked hypothesis? | ACH or Inconsistencies |
| Assumption coverage | 0.20 | What % of linchpin assumptions have direct supporting or challenging evidence? | KAC |
| Cross-technique convergence | 0.15 | Do techniques agree on the primary finding? | 2+ techniques |
| Deception resilience | 0.10 | What % of high-diagnostic evidence is rated LOW deception risk? | Deception Detection |

**Weight renormalization**: If a required technique was not run, drop that criterion and redistribute its weight proportionally among the remaining criteria. For example, in Lean mode (no ACH, no Deception Detection), drop Evidence discrimination (0.30) and Deception resilience (0.10), then renormalize: Hypothesis separation becomes 0.25/0.60 ≈ 0.42, Assumption coverage becomes 0.20/0.60 ≈ 0.33, Cross-technique convergence becomes 0.15/0.60 ≈ 0.25. Always ensure weights sum to 1.0.

Score each criterion 1–5, compute weighted total (using renormalized weights). Log to `meta.md`.
- **Score > 4.0**: PASS — proceed to report finalization.
- **Score 3.0–4.0**: ADVISORY — note in report, proceed unless human review overrides.
- **Score < 3.0**: FAIL — flag for mandatory iteration before report finalization. Do not finalize without addressing the lowest-scoring criteria.

---

## Step 4: Human Review Gate

Present a consolidated summary in conversation BEFORE finalizing:

```
## Analysis Summary for Review

**Problem**: {{REFRAMED_PROBLEM}}
**Evidence**: {{EVIDENCE_COUNT}} items ({{TIER_BREAKDOWN}})
**Evidence Gaps**: {{GAPS_IDENTIFIED}}
**Key Finding**: {{TOP_FINDING}} (Confidence: {{LEVEL}})
**Quality Score**: {{SCORE}}/5.0 ({{PASS/ADVISORY/FAIL}})
**Self-Critique Flags**: {{LAYER_2_FLAGS}}

What would you adjust before I finalize?
```

Incorporate user feedback into the final report. If no feedback, proceed.

---

## Step 5: Write Final Artifacts

### Report (`report.md`)
Read `templates/report-template.md` and fill every section in document order (disposition first, detail last). The template is the authoritative section ordering — follow it exactly.

**Reusable section components** (in `templates/sections/`):
- `header.md` — standard artifact header format
- `confidence-scale.md` — confidence level definitions (High/Moderate/Low with criteria) per ICD 203 Standard 2
- `judgment-table.md` — key judgments table format with technique/assumption/indicator cross-references
- `citation-block.md` — standard citation table with method explanations

Use these as reference for consistent formatting across all report sections.

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
- **Layer 2**: Self-critique results from Step 3 above (3a–3h checks)

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
| Layer 2 - 3f | Internal inconsistency found | *(in-report fix — no technique re-run)* | Correct the inconsistency in the report |
| Layer 2 - 3g | Analytical bias detected | `devils-advocacy` or `red-hat` | Seek evidence that counters the biased direction |
| Layer 2 - 3h | Quality score < 3.0 | Technique(s) driving lowest-scoring criterion | Collect discriminating evidence |

**Rules**:
- Bipolar bias (3c) is noted in the report but NOT mapped to an iteration — it's an in-report confidence adjustment, not a re-run trigger.
- Internal inconsistency (3f) is corrected in the report directly — no technique re-run needed.
- Quality score failures (3h) map to the technique(s) responsible for the lowest-scoring criterion, not a blanket re-run.
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
