# Next Steps Artifact Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Add a standalone `next-steps.md` artifact that consolidates iteration suggestions, tracks resolution status across iterations, and serves as the primary input for `--iterate`.

**Architecture:** Phase A subagent writes `next-steps.md` to the analysis root during report synthesis. The auto-remediation gate and iteration handler update item statuses in-place. The `--iterate` handler reads this file instead of meta.md for technique/evidence targeting.

**Tech Stack:** Markdown protocol files (no code — this is a prompt engineering change)

**Design doc:** `docs/plans/2026-02-17-next-steps-artifact-design.md`

---

### Task 1: Create the next-steps template

**Files:**
- Create: `skills/structured-analysis/templates/next-steps-template.md`

**Step 1: Write the template**

```markdown
# Next Steps: {{ANALYSIS_ID}}

Generated: {{DATE}} | Iteration: {{N}} | Quality Score: {{SCORE}}/5.0

## Summary
{{TOTAL_COUNT}} items identified. {{REMEDIATED_COUNT}} auto-remediated. {{RESOLVED_COUNT}} resolved. {{OPEN_COUNT}} open.

## Iteration Items

| # | Flag Source | Flag Type | Severity | Status | Technique(s) | Evidence Focus |
|---|-----------|-----------|----------|--------|-------------|----------------|
{{#EACH ITEM}}
| {{N}} | {{FLAG_SOURCE}} | {{FLAG_TYPE}} | {{SEVERITY}} | {{STATUS}} | {{TECHNIQUES}} | {{EVIDENCE_FOCUS}} |
{{/EACH}}

## Detail

{{#EACH ITEM}}
### {{N}}. [{{STATUS}}] {{FLAG_TYPE}} ({{FLAG_SOURCE}}) — {{SEVERITY}}
{{DESCRIPTION}}
- **Technique**: {{TECHNIQUES}}
- **Evidence focus**: {{EVIDENCE_FOCUS}}
{{#IF REMEDIATED}}
- **Remediation note**: Auto-remediated in cycle {{CYCLE}}. See working/{{TECHNIQUE}}.remediation-prior.md
{{/IF}}
{{#IF RESOLVED}}
- **Resolved**: Iteration {{ITER_N}} ({{DATE}}). {{NEW_EVIDENCE_COUNT}} new evidence items ({{ID_RANGE}}). {{RESOLUTION_SUMMARY}}.
{{/IF}}
{{/EACH}}

## Suggested Command
{{#IF HAS_OPEN_ITEMS}}
`/analyze --iterate {{ANALYSIS_ID}} {{OPEN_TECHNIQUE_LIST}}`
{{/IF}}
{{#IF NO_OPEN_ITEMS}}
No open iteration items. All identified gaps have been addressed.
{{/IF}}
```

**Step 2: Commit**

```bash
git add skills/structured-analysis/templates/next-steps-template.md
git commit -m "feat: add next-steps-template.md for iteration suggestions artifact"
```

---

### Task 2: Update report-generator.md — Phase A writes next-steps.md

**Files:**
- Modify: `skills/structured-analysis/protocols/report-generator.md`

**Step 1: Add next-steps.md to Phase A subagent outputs**

In the Phase A Subagent Prompt section (lines 22-46), update the numbered instructions list. Currently item 8 writes three files. Add `next-steps.md` as a fourth output and add a new instruction item for reading the template.

Change item 7 from:
```
7. Read {{SKILL_DIR}}/templates/review-summary-template.md and fill it with summary data
```
to:
```
7. Read {{SKILL_DIR}}/templates/review-summary-template.md and fill it with summary data
7a. Read {{SKILL_DIR}}/templates/next-steps-template.md — populate it using the flags collected in Step 7a-7b. Set all items to Status: OPEN. Write to analyses/{{ANALYSIS_ID}}/next-steps.md
```

Update item 8 from writing three files to writing four:
```
8. Write four files:
   - analyses/{{ANALYSIS_ID}}/report.md
   - analyses/{{ANALYSIS_ID}}/monitoring-plan.md
   - analyses/{{ANALYSIS_ID}}/working/review-summary.md
   - analyses/{{ANALYSIS_ID}}/next-steps.md
```

Also update the "Phase A — Synthesis Subagent" description at line 14 to list `next-steps.md`:
```
- Writes: `report.md`, `monitoring-plan.md`, `working/review-summary.md`, `next-steps.md`
```

**Step 2: Simplify the Iteration Suggestions section in review-summary-template.md**

In `skills/structured-analysis/templates/review-summary-template.md`, replace the entire `## Iteration Suggestions` section (lines 27-51) with a cross-reference:

```markdown
## Iteration Suggestions
See [next-steps.md](../next-steps.md) for the full iteration suggestions ledger with status tracking.
```

**Step 3: Update Step 7c presentation format**

In `report-generator.md`, update Step 7c (lines 263-303). After the existing presentation format, add a line telling Phase B to also mention next-steps.md:

Add after line 295 (`{{/IF}}`), before the final notes:
```
- After presenting iteration suggestions in conversation, add: `"Full iteration ledger with status tracking: analyses/{{ANALYSIS_ID}}/next-steps.md"`
```

**Step 4: Commit**

```bash
git add skills/structured-analysis/protocols/report-generator.md skills/structured-analysis/templates/review-summary-template.md
git commit -m "feat: Phase A writes next-steps.md alongside report and review-summary"
```

---

### Task 3: Update orchestrator.md — Auto-remediation gate updates next-steps.md

**Files:**
- Modify: `skills/structured-analysis/protocols/orchestrator.md`

**Step 1: Add next-steps.md status update to the auto-remediation gate**

In the Auto-Remediation Gate section (lines 429-476), after step 3f (the iteration handler executes its standard workflow), add a new step 3g:

```markdown
   g. **Update next-steps.md**: Read `analyses/{{ANALYSIS_ID}}/next-steps.md`. For each HIGH flag that was remediated in step 3e-3f:
      - Update the Status column from `OPEN` to `REMEDIATED` in the Iteration Items table
      - Update the detail section: change `[OPEN]` to `[REMEDIATED]` and append a remediation note: `"Auto-remediated in cycle 1. See working/{{TECHNIQUE}}.remediation-prior.md"`
      - Update the Summary line counts (increment REMEDIATED, decrement OPEN)
      - Update the Suggested Command to exclude remediated techniques
```

Re-letter the existing step 3g (Phase B now presents the iteration-2 report) to 3h.

**Step 2: Add next-steps.md to Phase B file references**

In the Phase B Flow section (lines 50-62), at step 5 (Execute Step 6 — Present Results), add:
```
   - Reference `next-steps.md` path when presenting iteration suggestions
```

**Step 3: Commit**

```bash
git add skills/structured-analysis/protocols/orchestrator.md
git commit -m "feat: auto-remediation gate updates next-steps.md status column"
```

---

### Task 4: Update iteration-handler.md — Read and update next-steps.md

**Files:**
- Modify: `skills/structured-analysis/protocols/iteration-handler.md`

**Step 1: Update Step 1 to read next-steps.md as primary input**

After the existing Step 1 content (lines 22-33), add a new sub-step:

```markdown
4. Read `analyses/{{ANALYSIS_ID}}/next-steps.md` (if it exists):
   - Parse the Iteration Items table
   - Filter to `OPEN` items only
   - Extract technique names and evidence focus descriptions
   - These become the iteration scope (overrides meta.md technique list when next-steps.md exists)
   - If `next-steps.md` does not exist (legacy analysis created before this feature): fall back to reading meta.md for technique list (existing behavior)
```

**Step 2: Add Step 5b — Update next-steps.md after iteration**

After the existing Step 5a (lines 127-161), add a new step:

```markdown
## Step 5b — Update Next Steps Ledger

After cross-iteration synthesis, update `analyses/{{ANALYSIS_ID}}/next-steps.md`:

1. **Resolve completed items**: For each technique that was re-run in this iteration:
   - Find matching `OPEN` items in the Iteration Items table
   - Update Status from `OPEN` to `RESOLVED — Iter {{CURRENT_ITER}}`
   - In the Detail section, change `[OPEN]` to `[RESOLVED — Iter {{CURRENT_ITER}}]`
   - Append a resolution note:
     ```
     - **Resolved**: Iteration {{CURRENT_ITER}} ({{DATE}}). {{NEW_EVIDENCE_COUNT}} new evidence items ({{ID_RANGE}}). {{ONE_LINE_SUMMARY_FROM_DELTA}}.
     ```

2. **Append new flags**: If the iteration's Layer 2 self-critique (run during report regeneration) produces new actionable flags:
   - Append new rows to the Iteration Items table with sequential numbering continuing from the last item
   - Add corresponding Detail sections at the bottom
   - Set Status to `OPEN` for all new items

3. **Update summary counts**: Recalculate TOTAL, REMEDIATED, RESOLVED, and OPEN counts in the Summary line

4. **Update suggested command**: Regenerate the `/analyze --iterate` command with only the techniques from remaining `OPEN` items. If no `OPEN` items remain, replace with: `"No open iteration items. All identified gaps have been addressed."`

5. **Update header**: Set `Iteration: {{CURRENT_ITER}}` and update Quality Score if a new score was produced

**Legacy fallback**: If `next-steps.md` does not exist (analysis predates this feature), skip this step. The iteration's Phase A report synthesis will create `next-steps.md` as part of its normal output, bootstrapping the ledger for future iterations.
```

**Step 3: Commit**

```bash
git add skills/structured-analysis/protocols/iteration-handler.md
git commit -m "feat: iteration handler reads and updates next-steps.md ledger"
```

---

### Task 5: Update SKILL.md with next-steps.md in artifact list

**Files:**
- Modify: `skills/structured-analysis/SKILL.md`

**Step 1: Add next-steps.md to the output artifacts documentation**

Find the section that lists output artifacts and add `next-steps.md` with a one-line description:
```
- `next-steps.md` — Iteration suggestions ledger with status tracking (OPEN/REMEDIATED/RESOLVED/DEFERRED)
```

**Step 2: Commit**

```bash
git add skills/structured-analysis/SKILL.md
git commit -m "docs: add next-steps.md to SKILL.md artifact list"
```
