# Context Optimization Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Reduce main-context token consumption, parallelize OSINT, fix iteration report regeneration, and add inter-tier feedback gates to catch self-critique findings earlier.

**Architecture:** Five changes to the structured-analysis skill's protocol files: (1) move report synthesis into a subagent with a compact review-summary handoff, (2) batch OSINT collection subagents in parallel, (3) bump technique cap from 5 to 6, (4) wire iteration-handler's delta output into report regeneration, (5) add tier gate checks between subagent dispatch tiers.

**Tech Stack:** Markdown protocol files (no code — this is a prompt engineering project). All files under `skills/structured-analysis/`.

**Working directory:** `/home/angel/Documents/code/structured-analysis-skill/.worktrees/context-optimization`

**Note:** This project has no tests. "Verification" means reading the modified file and confirming the changes are consistent with the rest of the protocol. Each task ends with a commit.

---

### Task 1: Technique Cap 5 → 6

**Files:**
- Modify: `skills/structured-analysis/protocols/orchestrator.md` (lines ~145-150, Step 4 Default Mode)

**Step 1: Edit the technique cap**

In the orchestrator's "Step 4 — Effort Check & Technique Cap" section, under "Default Mode (no effort flag)", change:

```
- Questions 1–11 active; max 5 techniques
```
to:
```
- Questions 1–11 active; max 6 techniques
```

And change:
```
  1. Phase alignment (prefer techniques matching Stage Check result)
```
(the prioritization block that references >5 match) — update all references from 5 to 6:

```
- Prioritization when >6 match:
```

And the overflow message:
```
- If >6 match, inform user: "I identified [N] matching techniques. Recommending [6 prioritized]. Use --comprehensive to run all."
```

**Step 2: Verify**

Read the modified section and confirm all instances of "5" in the technique cap context are now "6". Confirm the lean mode (3 techniques) and comprehensive mode (no cap) are unchanged.

**Step 3: Commit**

```bash
git add skills/structured-analysis/protocols/orchestrator.md
git commit -m "fix: bump default technique cap from 5 to 6

The 14-question rubric regularly selects 6 techniques for moderately
complex problems. The old cap of 5 forced unnecessary technique drops.

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>"
```

---

### Task 2: Create Review Summary Template

**Files:**
- Create: `skills/structured-analysis/templates/review-summary-template.md`

**Step 1: Write the template**

Create a new template file for the compact review summary that the report synthesis subagent will write. This is the only file the main context reads after report generation.

```markdown
# Review Summary: {{ANALYSIS_ID}}

## Problem
{{REFRAMED_PROBLEM}}

## Evidence
- **Total items**: {{EVIDENCE_COUNT}}
- **Tier breakdown**: Tier 1: {{T1_COUNT}} | Tier 2: {{T2_COUNT}} | Tier 3: {{T3_COUNT}}
- **Evidence gaps**: {{GAPS_OR_NONE}}

## Key Finding
{{TOP_FINDING}}
- **Confidence**: {{CONFIDENCE_LEVEL}}
- **Supporting techniques**: {{TECHNIQUE_LIST}}

## Quality Score
- **Score**: {{SCORE}}/5.0
- **Result**: {{PASS_ADVISORY_FAIL}}
- **Lowest criterion**: {{LOWEST_CRITERION_NAME}} ({{LOWEST_SCORE}}/5)

## Layer 2 Flags
{{#EACH FLAG}}
{{N}}. **{{FLAG_SOURCE}}**: {{FLAG_DESCRIPTION}}
{{/EACH}}

## Iteration Suggestions
{{#IF HAS_ACTIONABLE_FLAGS}}
Self-critique identified {{FLAG_COUNT}} actionable items:

{{#EACH SUGGESTION}}
{{N}}. **{{FLAG_TYPE}}**: {{FLAG_DESCRIPTION}}
   → Re-run: {{TECHNIQUES}} | Evidence focus: {{FOCUS}}
{{/EACH}}

Suggested command: `/analyze --iterate {{ANALYSIS_ID}} {{TECHNIQUE_LIST}}`
{{/IF}}
{{#IF NO_ACTIONABLE_FLAGS}}
No actionable iteration suggestions. All self-critique checks passed or produced only in-report adjustments.
{{/IF}}
```

**Step 2: Verify**

Read the file and confirm it covers all fields needed by the human review gate (Step 4 of report-generator.md): problem, evidence count, key finding, quality score, and Layer 2 flags.

**Step 3: Commit**

```bash
git add skills/structured-analysis/templates/review-summary-template.md
git commit -m "feat: add review-summary template for report synthesis subagent

Compact (~500 token) summary that the main context reads instead of
the full report. Contains problem, evidence stats, key finding,
quality score, Layer 2 flags, and iteration suggestions.

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>"
```

---

### Task 3: Refactor Report Generator — Split into Phase A/B

**Files:**
- Modify: `skills/structured-analysis/protocols/report-generator.md`

**Step 1: Add the Phase A/B architecture**

Restructure report-generator.md to clearly separate what runs in a subagent vs main context. Add a new section at the top (after the title and before Step 1) explaining the split:

```markdown
## Execution Architecture

The report generator runs in two phases to keep full artifact content out of the main context:

**Phase A — Synthesis Subagent** (foreground Task subagent):
- Executes Steps 1–3 and Step 5 (gather inputs, synthesize, self-critique, write artifacts)
- Reads all working artifacts from disk (fresh context window — no main context pollution)
- Writes: `report.md`, `monitoring-plan.md`, `working/review-summary.md`
- Returns ONLY: "Report written. Quality score: X/5.0 (STATUS)"

**Phase B — Human Review & Finalization** (main context):
- Executes Steps 4, 6, and 7 (human review gate, present results, iteration suggestions)
- Reads ONLY `working/review-summary.md` (~500 tokens)
- If user provides feedback: dispatches a revision subagent to apply edits to report.md

### Phase A Subagent Prompt

The orchestrator dispatches Phase A using the Task tool with `subagent_type: "general-purpose"` and this prompt:

\```
You are a structured analysis report synthesizer. Your task:

1. Read all working artifacts from analyses/{{ANALYSIS_ID}}/working/
2. Read analyses/{{ANALYSIS_ID}}/evidence-registry.md
3. Read analyses/{{ANALYSIS_ID}}/meta.md
4. Read the report-generator protocol at {{SKILL_DIR}}/protocols/report-generator.md — execute Steps 1-3 and Step 5
5. Read and fill {{SKILL_DIR}}/templates/report-template.md (disposition first, detail last)
6. Read {{SKILL_DIR}}/templates/monitoring-plan-template.md and generate the monitoring plan
7. Read {{SKILL_DIR}}/templates/review-summary-template.md and fill it with summary data
8. Write three files:
   - analyses/{{ANALYSIS_ID}}/report.md
   - analyses/{{ANALYSIS_ID}}/monitoring-plan.md
   - analyses/{{ANALYSIS_ID}}/working/review-summary.md
9. Update analyses/{{ANALYSIS_ID}}/meta.md with completion status and quality score

{{ITERATION_CONTEXT}}

Return ONLY: "Report written. Quality score: X/5.0 (STATUS)"
\```

Where `{{ITERATION_CONTEXT}}` is empty for first-run analyses, or contains iteration-specific instructions (see iteration-handler protocol).

### Phase B Flow

After the Phase A subagent returns:

1. Read `analyses/{{ANALYSIS_ID}}/working/review-summary.md`
2. Execute Step 4 (Human Review Gate) using the summary data
3. If user provides feedback:
   - Dispatch a revision subagent with: the feedback text, path to report.md, and instruction to apply edits and rewrite
   - After revision subagent returns, re-read review-summary.md if quality score changed
4. If no feedback: proceed
5. Execute Step 6 (Present Results) — use summary data for the conversation output
6. Execute Step 7 (Iteration Suggestions) — use the suggestions from review-summary.md
```

**Step 2: Update Steps 1-3 and Step 5 headers**

Add a note to each of Steps 1, 2, 3, and 5 indicating they run inside the Phase A subagent:

```markdown
## Step 1: Gather Inputs *(Phase A — runs in subagent)*
```

And to Steps 4, 6, 7:

```markdown
## Step 4: Human Review Gate *(Phase B — runs in main context)*
```

**Step 3: Update Step 4 to read from review-summary.md**

Replace the current Step 4 template that references `{{REFRAMED_PROBLEM}}` etc. with:

```markdown
## Step 4: Human Review Gate *(Phase B — runs in main context)*

Read `analyses/{{ANALYSIS_ID}}/working/review-summary.md` and present its contents in conversation:

\```
## Analysis Summary for Review

[Present the review summary contents directly — problem, evidence stats, key finding, quality score, Layer 2 flags]

What would you adjust before I finalize?
\```

If user provides feedback, dispatch a revision subagent:
- Input: user feedback text + `analyses/{{ANALYSIS_ID}}/report.md`
- Instruction: Apply the specified edits to the report. Rewrite the file.
- Return: "Revisions applied."

If no feedback, proceed to Step 6.
```

**Step 4: Update Step 6 and Step 7**

Update Step 6 to note it reads from review-summary.md rather than the full report:

```markdown
## Step 6: Present Results *(Phase B — runs in main context)*

Read `analyses/{{ANALYSIS_ID}}/working/review-summary.md`. In conversation, provide:
- One-paragraph summary of the key finding with confidence (from the summary)
- File paths to all generated artifacts
- Monitoring plan highlights (top 2-3 indicators to watch)
- Invitation: "Would you like me to deep-dive into any section or adjust the analysis?"
```

Update Step 7 to note iteration suggestions come from the summary:

```markdown
## Step 7: Iteration Suggestions *(Phase B — runs in main context)*

**Guard**: Check the review-summary.md for iteration suggestions. If the summary says "No actionable iteration suggestions," skip this step.

Present the iteration suggestions from the review summary in conversation.
```

**Step 5: Verify**

Read the full modified file. Confirm:
- Steps 1-3 and 5 are marked Phase A
- Steps 4, 6, 7 are marked Phase B
- Step 4 reads review-summary.md, not full artifacts
- The subagent prompt template is complete and references all necessary files
- No step references reading working artifacts in main context

**Step 6: Commit**

```bash
git add skills/structured-analysis/protocols/report-generator.md
git commit -m "feat: split report generator into Phase A subagent + Phase B main context

Phase A (subagent): reads all artifacts, runs synthesis + Layer 2
self-critique, writes report.md + monitoring-plan.md + review-summary.md.
Phase B (main context): reads only the ~500-token review summary,
presents human review gate, handles feedback via revision subagent.

Removes ~150KB of artifact content from main context window.

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>"
```

---

### Task 4: Update Orchestrator — Report Dispatch

**Files:**
- Modify: `skills/structured-analysis/protocols/orchestrator.md`

**Step 1: Update report invocation references**

In the orchestrator, find all places that reference executing the report generator and update them to reflect the Phase A/B split. Key locations:

1. **Guided Mode Phases** (Step 7: Report): Change from "Read and execute `protocols/report-generator.md`" to:

```markdown
7. **Report**: Dispatch report synthesis subagent per `protocols/report-generator.md` Phase A. Then execute Phase B (human review gate) in main context.
```

2. **Adaptive Mode** (wherever it references report generation): Same update pattern.

3. **SKILL.md Step 6**: Update the reference to say "For report synthesis: dispatch per `protocols/report-generator.md` Phase A/B architecture"

**Step 2: Verify**

Read the modified orchestrator sections. Confirm all report generation references now use Phase A/B language.

**Step 3: Commit**

```bash
git add skills/structured-analysis/protocols/orchestrator.md skills/structured-analysis/SKILL.md
git commit -m "feat: update orchestrator and SKILL.md for Phase A/B report dispatch

All report generation references now point to the split architecture:
Phase A subagent for synthesis, Phase B main context for human review.

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>"
```

---

### Task 5: Parallel OSINT Collection

**Files:**
- Modify: `skills/structured-analysis/protocols/evidence-collector.md` (Step 3a section)

**Step 1: Replace sequential dispatch with parallel batches**

In the "Step 3a — Raw Collection" section, replace the current dispatch instructions. Change from:

```markdown
Spawn foreground Task subagents sequentially (one per search theme). Each subagent inherits MCP tools from the parent and can use Firecrawl or WebSearch/WebFetch.

**Dispatch order**: Core themes first (1-3), then adaptive themes in priority order (4+). If time or tool constraints force early termination, the highest-priority themes will already be collected.

For each theme, invoke the Task tool with `subagent_type: "general-purpose"` and the following prompt template
```

To:

```markdown
Spawn foreground Task subagents in parallel batches. Each subagent inherits MCP tools from the parent and can use Firecrawl or WebSearch/WebFetch.

**Prerequisite**: Parallel dispatch requires search/scrape tools in the user's allow list (see Tool Permissions below). If tools require per-call approval, fall back to sequential dispatch (one theme at a time) to avoid permission prompt flooding.

**Batch dispatch**:
- **Batch 1 (Core themes 1-3)**: Launch all three core theme subagents simultaneously using the Task tool. Wait for all three to complete before proceeding.
- **Batch 2 (Adaptive themes 4+)**: Launch all adaptive theme subagents simultaneously. Wait for all to complete.

Core themes run first so that if Batch 2 encounters errors (rate limits, tool failures), the foundational evidence is already collected.

**Error handling per batch**: Each subagent writes to its own file independently. If any subagent in a batch fails, the others still succeed. Failed themes are logged and can be retried in the sufficiency gate retry cycle.

For each theme, invoke the Task tool with `subagent_type: "general-purpose"` and the following prompt template
```

**Step 2: Verify**

Read the modified Step 3a section. Confirm:
- Sequential dispatch language is replaced with parallel batch language
- Two-batch structure is documented (core first, adaptive second)
- Fallback to sequential is documented
- The per-theme subagent prompt template is unchanged
- Step 3b (evidence extraction, already parallel) is unchanged

**Step 3: Commit**

```bash
git add skills/structured-analysis/protocols/evidence-collector.md
git commit -m "feat: parallelize OSINT collection into two batches

Core themes (1-3) launch simultaneously as Batch 1, adaptive themes
(4+) as Batch 2. Falls back to sequential if tools need per-call
approval. Each subagent writes independently so failures don't block
siblings.

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>"
```

---

### Task 6: Iteration-Aware Report Regeneration

**Files:**
- Modify: `skills/structured-analysis/protocols/iteration-handler.md` (Steps 5 and 2)
- Modify: `skills/structured-analysis/protocols/orchestrator.md` (Iterate Mode section)

**Step 1: Add iteration-context.md writing to iteration-handler**

In the iteration-handler, add a new Step 5a after the current Step 5 (Cross-Iteration Synthesis), before Step 6:

```markdown
## Step 5a — Write Iteration Context for Report Generator

After cross-iteration synthesis, write `analyses/{{ANALYSIS_ID}}/working/iteration-context.md`:

\```markdown
# Iteration Context

- **Iteration**: {{CURRENT_ITER}}
- **Trigger**: {{TRIGGER_DETAIL}}
- **Scope**: {{FULL_OR_SCOPED}} — techniques: {{TECHNIQUE_LIST}}
- **Prior report**: report.v{{PRIOR_VERSION}}.md

## Delta Summary

### Judgment Revisions
| Judgment | Prior (v{{PRIOR}}) | Current (v{{CURRENT}}) | Driver |
|----------|-------------------|----------------------|--------|
{{JUDGMENT_REVISION_ROWS}}

### Findings That Strengthened
{{STRENGTHENED_LIST}}

### Findings That Weakened
{{WEAKENED_LIST}}

### New Findings
{{NEW_FINDINGS_LIST}}

### Invalidated Findings
{{INVALIDATED_LIST}}

### Confidence Shifts
{{CONFIDENCE_SHIFT_NARRATIVE}}
\```

This file is consumed by the report synthesis subagent (Phase A) to populate the Revision History section.
```

**Step 2: Add report/monitoring-plan archiving to iteration-handler Step 2**

In Step 2 (Archive Prior Artifacts), add after the existing technique artifact archiving:

```markdown
### Archive Report and Monitoring Plan

Before report regeneration:
1. If `report.md` exists: rename to `report.v{{PRIOR_VERSION}}.md`
2. If `monitoring-plan.md` exists: rename to `monitoring-plan.v{{PRIOR_VERSION}}.md`

These are archived alongside technique artifacts so the report synthesis subagent can reference the prior report for comparison.
```

**Step 3: Update orchestrator iterate mode — report dispatch**

In the orchestrator's "Full Iteration" section (steps 7-8), replace:

```
7. **Regenerate report** — include "Revision History" section from `templates/report-template.md`
```

With:

```markdown
7. **Regenerate report** — dispatch report synthesis subagent per `protocols/report-generator.md` Phase A with iteration context. The subagent prompt includes:
   ```
   ## Iteration Context
   This is iteration {{N}}. Read working/iteration-context.md for the delta summary.

   Additional instructions:
   - Populate the Revision History section in the report template
   - For each key judgment, note whether it changed from prior iteration and why
   - Reference prior findings using [PRIOR-v{{N}}: technique_name] citation format
   - Read the prior report at report.v{{PRIOR}}.md for comparison
   ```
   Then execute Phase B (human review gate) in main context.
```

**Step 4: Update scoped iteration report handling**

In the "Scoped Iteration" section, replace step 7:

```
7. **Write iteration findings summary** — in iteration metadata, NOT a full report regeneration (unless explicitly requested by the analyst)
```

With:

```markdown
7. **Write iteration findings summary** — write findings to iteration metadata. If the analyst explicitly requests report regeneration, dispatch report synthesis subagent with iteration context (same as full iteration Step 7). Otherwise, skip report regeneration.
```

**Step 5: Verify**

Read both modified files. Confirm:
- iteration-handler has Step 5a writing iteration-context.md
- iteration-handler Step 2 archives report.md and monitoring-plan.md
- Orchestrator iterate mode dispatches report via Phase A with iteration context
- Scoped iteration still defaults to no report regeneration
- The iteration-context.md format matches the report template's Revision History section fields

**Step 6: Commit**

```bash
git add skills/structured-analysis/protocols/iteration-handler.md skills/structured-analysis/protocols/orchestrator.md
git commit -m "feat: wire iteration delta into report regeneration

Iteration handler now writes working/iteration-context.md with the
cross-iteration delta summary. Orchestrator dispatches report
synthesis subagent with iteration context to populate the Revision
History section. Prior report and monitoring plan are archived.

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>"
```

---

### Task 7: Inter-Tier Feedback Gates

**Files:**
- Modify: `skills/structured-analysis/protocols/orchestrator.md` (after Subagent Dispatch section)

**Step 1: Add the Tier Gate Protocol section**

After the "Return Contract" subsection of the Subagent Dispatch section, add a new section:

```markdown
#### Tier Gate Protocol

After collecting all subagent summaries for tier N and before dispatching tier N+1, run this lightweight check in main context. The gate reads ONLY the compact findings summaries already accumulated (~200-400 tokens each) — no disk reads.

**Check 1: MISSING HYPOTHESIS**
- Scan findings for phrases like "gap in hypothesis coverage," "missing hypothesis," "additional hypothesis needed," or "not captured in current framework"
- Action: If a downstream technique can address it (What-If for scenarios, Contrasting Narratives for competing explanations, ACH re-run for matrix expansion), add to that technique's SETUP context: `"Prior technique {{NAME}} identified missing hypothesis: {{DESCRIPTION}}. Incorporate this into your analysis."`

**Check 2: EVIDENCE GAP**
- Scan findings for phrases like "insufficient evidence on," "no evidence available for," "evidence gap," or "unable to assess due to missing data"
- Action: If OSINT is enabled, spawn a targeted evidence collection mini-round before the next tier:
  - Generate 1-2 focused search themes addressing the gap
  - Dispatch as foreground Task subagent(s) using the Step 3a prompt template
  - Run Step 3b extraction on new raw files
  - Update evidence-registry.md with new items
  - Add to downstream techniques' SETUP context: `"New evidence E{{START}}-E{{END}} collected targeting: {{GAP_DESCRIPTION}}"`

**Check 3: CONTRADICTION**
- Scan findings from the same tier for conflicting conclusions (e.g., Technique A finds X likely while Technique B finds X unlikely)
- Action: Add to downstream techniques' SETUP context: `"Tier {{N}} produced conflicting findings: {{TECHNIQUE_A}} found {{X}} while {{TECHNIQUE_B}} found {{Y}}. Your analysis should address this tension."`

**Check 4: CONFIDENCE COLLAPSE**
- Scan findings for a technique where ALL findings are Low confidence
- Action: Log a Layer 1 flag in meta.md. Add warning to downstream techniques' SETUP context: `"{{TECHNIQUE}} returned all findings at Low confidence. Treat its outputs as provisional."`

**Gate rules:**
- Gates are **additive only** — they inject SETUP context or add mini-collection rounds. They never remove or skip already-selected techniques.
- A gate can add at most **1 targeted evidence collection round** (1-2 themes) per tier transition.
- If no checks trigger, the gate passes silently (no logging, no output).
- All gate actions are logged in `meta.md` under a new **Tier Gate Actions** subsection of Self-Correction.
- Gate processing should take <30 seconds — it reads only compact summaries already in main context.
```

**Step 2: Update the dispatch sequence to include tier gates**

In the "Dispatch Sequence" subsection, after step 4d ("Wait for all tier subagents to complete") and before step 4e ("Collect and validate each subagent's return summary"), the sequence currently goes straight to validation. Insert the tier gate between tiers. Update the dispatch sequence step 4 to:

```markdown
4. **For each tier (1 → 4)**, if that tier has selected techniques:
   a. Build the file manifest: list all files currently in `analyses/{{ANALYSIS_ID}}/working/`
   b. Construct one subagent prompt per technique using the template below
   c. Launch all tier subagents as background Task agents in parallel
   d. Wait for all tier subagents to complete
   e. Collect and validate each subagent's return summary
   e2. **Artifact validation**: For each completed subagent, verify the artifact file exists on disk and contains no unfilled `{{PLACEHOLDER}}` tokens. If validation fails, log as `PARTIAL` and add a Layer 1 flag.
   f. If any subagent returns `FAILED`: log the error, skip that technique, add a Layer 1 flag
   g. If any subagent returns `PARTIAL`: log warnings, accept partial results, add a Layer 1 flag
   h. Update `meta.md` with each technique's status and dispatch tier
   **i. Run Tier Gate Protocol** (see below): scan accumulated findings for signals, inject corrective context into next tier's subagent prompts if needed
```

**Step 3: Add Tier Gate Actions to meta.md guidance**

In the meta.md update section or the Self-Correction section references, add:

```markdown
### Tier Gate Actions

| Tier | Check | Signal | Action Taken |
|------|-------|--------|-------------|
```

**Step 4: Verify**

Read the modified orchestrator. Confirm:
- Tier Gate Protocol section is placed after the Return Contract
- Dispatch Sequence step 4i references the Tier Gate Protocol
- Four checks are documented with specific signal phrases and actions
- Rules are clear (additive only, max 1 evidence round per gate, silent pass)
- Meta.md logging is specified

**Step 5: Commit**

```bash
git add skills/structured-analysis/protocols/orchestrator.md
git commit -m "feat: add inter-tier feedback gates for early self-critique

Tier gates scan compact findings summaries after each dispatch tier
and inject corrective context into downstream techniques: missing
hypotheses, evidence gaps (with mini-collection), contradictions,
and confidence collapse. Additive only — never removes techniques.

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>"
```

---

### Task 8: Final Verification Pass

**Files:**
- Read: all modified files for cross-reference consistency

**Step 1: Cross-reference check**

Read the following files and verify internal consistency:

1. `protocols/orchestrator.md` — confirm:
   - Technique cap says 6 (Task 1)
   - Report dispatch uses Phase A/B language (Task 4)
   - Iterate mode references iteration-context.md (Task 6)
   - Tier Gate Protocol section exists after Return Contract (Task 7)
   - Dispatch Sequence includes step 4i (Task 7)

2. `protocols/report-generator.md` — confirm:
   - Phase A/B architecture section exists at top
   - Steps 1-3 and 5 marked Phase A
   - Steps 4, 6, 7 marked Phase B
   - Step 4 reads review-summary.md
   - Subagent prompt template references all necessary files

3. `protocols/evidence-collector.md` — confirm:
   - Step 3a uses parallel batch dispatch
   - Fallback to sequential documented
   - Steps 3b, 3c unchanged

4. `protocols/iteration-handler.md` — confirm:
   - Step 2 archives report.md and monitoring-plan.md
   - Step 5a writes iteration-context.md
   - Delta summary format matches report template Revision History fields

5. `templates/review-summary-template.md` — confirm:
   - Contains all fields needed by Phase B Step 4

6. `SKILL.md` — confirm:
   - Report reference updated to Phase A/B

**Step 2: Verify no orphaned references**

Search all protocol files for references to deleted or renamed concepts. Specifically:
- No remaining references to report generator reading artifacts in main context
- No references to "max 5 techniques" remaining
- No references to sequential OSINT dispatch (outside the fallback clause)

**Step 3: Commit if any fixes needed**

```bash
git add -A
git commit -m "fix: cross-reference consistency pass

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>"
```

If no fixes needed, skip this commit.
