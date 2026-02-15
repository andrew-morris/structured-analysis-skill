---
name: analyze
description: Use when conducting structured analysis on any problem — assessing competing hypotheses, challenging assumptions, stress-testing judgments, or producing defensible analytical assessments with evidence and citations
---

# Structured Analysis Skill

Apply CIA/IC Structured Analytic Techniques to produce defensible, evidence-based analytical assessments. Every claim must be cited. Every judgment must trace to technique outputs.

## Invocation

```
/analyze                          → Adaptive mode (auto-select techniques)
/analyze <technique>              → Direct mode (run one technique)
/analyze --guided                 → Guided mode (walk through all phases)
/analyze --resume <analysis-id>   → Resume or update existing analysis
/analyze --lean                   → Lean mode (abbreviated technique set)
/analyze --no-osint               → Disable web research
```

Techniques: `customer-checklist`, `issue-redefinition`, `restatement`, `brainstorm`, `kac`, `ach`, `inconsistencies`, `cross-impact`, `what-if`, `premortem`, `counterfactual`, `narratives`, `bowtie`, `opportunities`

Flags combine: `/analyze --guided --no-osint` is valid.

## Execution

**You MUST read the orchestrator protocol before proceeding.** It contains mode routing, technique selection logic, and the technique routing table.

1. Read `protocols/orchestrator.md` (relative to this skill's directory)
2. Parse arguments → determine mode and flags
3. Follow the orchestrator's instructions for the detected mode
4. For each technique selected:
   - Read the technique's protocol file (SETUP → PRIME → EXECUTE → ARTIFACT → FINDINGS → HANDOFF)
   - Read the technique's template file for artifact structure
   - Execute the protocol, writing artifacts to `analyses/<id>/working/`
5. For evidence gathering: read and execute `protocols/evidence-collector.md`
6. For report synthesis: read and execute `protocols/report-generator.md`

## Self-Correction (3 Layers)

- **Layer 1** (after each technique, silent): Protocol compliance check — all steps completed? All template sections filled? No unfilled `{{PLACEHOLDER}}` tokens?
- **Layer 2** (before report, silent): Analytical self-critique — assumption audit, evidence balance, confidence calibration, alternative check, missing voices
- **Layer 3** (before finalization): Human review gate — present summary, incorporate feedback

## Citation Requirement

Every claim in every artifact must be cited. No exceptions. Citation methods:
- **OSINT**: `[Source](URL)` — Retrieved: YYYY-MM-DD
- **FILE**: `[filename:line_range]`
- **USER**: `[User-provided, session context]`
- **ANALYSIS**: `[Derived via technique_name]`

OSINT is never presented as fact — always "according to [source]".

## Reference Library

For deep background on any technique, read `docs/library/00-prime.md` and the specific library files referenced in each protocol. The library contains the full theoretical foundation, axioms, selection matrices, and empirical critiques underlying this skill.
