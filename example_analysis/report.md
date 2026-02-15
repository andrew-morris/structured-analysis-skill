# Structured Analysis Report: US Removal of Chinese Hackers from Networks

> **Analysis ID**: 2026-02-15-us-chinese-hackers-network-removal
> **Date**: 2026-02-15
> **Techniques Applied**: Key Assumptions Check, Analysis of Competing Hypotheses, Cross-Impact Matrix, What If? Analysis, Contrasting Narratives, Premortem + Self-Critique
> **Mode**: Adaptive
> **Evidence Base**: 20 items

---

## Executive Summary

The United States will not achieve durable, comprehensive removal of Chinese state-sponsored cyber actors (Salt Typhoon, Volt Typhoon, and related groups) from telecommunications and critical infrastructure networks within the next 2-3 years under current conditions. The most plausible outcome is **managed persistence** — incremental hardening of the most critical network segments while accepting that persistent Chinese access will continue across broad portions of US telecom infrastructure. This assessment carries **MODERATE** confidence, reflecting strong evidentiary support tempered by significant volatility in the political and institutional variables that could alter the trajectory.

Six structured analytic techniques converged on this finding but exposed a critical fragility: the assessment is conditioned on the current political trajectory (regulatory rollback, depleted CISA, voluntary compliance). Political will is simultaneously the **highest-leverage intervention point** and the **most volatile variable** in the system. A galvanizing crisis — such as a publicly disclosed compromise of senior military communications or a Taiwan Strait escalation — could trigger rapid political mobilization that would fundamentally alter the trajectory. The Premortem analysis estimates this "political activation" scenario at 30-40% probability within 18 months.

The analysis identifies a **12-18 month window of peak vulnerability** (mid-2026 to 2027) during which CISA is rebuilding, mandatory regulations are absent, Chinese offensive capabilities continue to advance, and the anticipated geopolitical crisis timeline approaches. Policy decisions made during this window will disproportionately determine outcomes.

---

## Problem Framing

**Original Statement**: "Will the US be able to successfully remove Chinese hackers from their networks?"

**Reframed Statement**: To what extent can the United States achieve durable removal of Chinese state-sponsored cyber actors (Salt Typhoon, Volt Typhoon, and related groups) from telecommunications and critical infrastructure networks, given current institutional, technical, and political constraints?

The reframing shifts from binary (yes/no) to a spectrum of outcomes, acknowledges the multi-group nature of the threat, specifies the target networks, and names the constraining variables. The evidence registry flagged the original binary framing as an analytical risk [E20].

---

## Evidence Base

**Collection scope**: 24 search results screened, 8 full articles scraped, yielding 20 catalogued evidence items.

| Source Tier | Count | Quality |
|-------------|-------|---------|
| Tier 1 (Conversation) | 1 | N/A |
| Tier 2 (Local Files) | 0 | N/A |
| Tier 3 (OSINT) | 19 | 15 High reliability, 4 Medium |

**Sources**: War on the Rocks, CyberScoop/Censys, Lawfare, Cybersecurity Dive, SC World, Reuters, DomainTools, FDD.

**Quality Assessment**: Evidence is weighted ~4:1 toward pessimism about removal success. This imbalance is partially justified by source quality but also reflects collection limitations: no telecom industry internal reporting, no cyber insurance market analysis, no classified assessments, and no Chinese strategic analysis beyond diplomatic denial. The evidence registry explicitly acknowledges these gaps.

> Full evidence details: [Evidence Registry](evidence-registry.md)

---

## Analysis

### 1. Key Assumptions Check (KAC)

**Purpose**: Identify and stress-test the foundational assumptions underlying the analytic line.
**Process**: Catalogued 10 assumptions (7 stated, 3 unstated), evaluated each against the evidence base, classified into Supported/Correct-with-Caveats/Unsupported bins, and identified 4 linchpin assumptions.

#### Findings

Ten assumptions were evaluated: **6 Supported**, **3 Correct with Caveats**, **1 Unsupported**.

Four linchpin assumptions identified:
1. **Infrastructure too large/complex** (Supported) — Most robustly evidenced: 200,000+ exposed devices with only 25% reduction in six months [E05], "Frankenstein's monster" architectures [E04], 100+ exploitable LTE/5G vulnerabilities with maintainers lacking capacity to patch [E07].
2. **Institutional/political obstacles will persist** (Supported but **most volatile**) — FCC rescinded binding orders [E02], CISA at ~40% vacancy [E12]. This is the **#1 fault line**: a galvanizing crisis could invalidate this assumption rapidly, cascading through the entire assessment.
3. **China's offensive capabilities will continue at scale** (Supported) — Contractor-enabled model with 600+ organizations breached [E17, E18], new malware families continuing to deploy [E16]. No challenging evidence found, though this may reflect a collection gap.
4. **Telecoms lack incentive to self-remediate** (Supported) — Seven-year-old unpatched vulnerabilities [E01, E03], Senate concluded carriers have not demonstrated eviction [E01].

The **unsupported** assumption (#10: allied coordination will not produce a step-change) represents a significant evidence gap — the analytic line implicitly treats the problem as purely domestic.

**Confidence**: MODERATE — Linchpin 2 (political will) is highly volatile and could invalidate the assessment rapidly.

> Full working artifact: [Key Assumptions Check](working/assumptions.md)

---

### 2. Analysis of Competing Hypotheses (ACH)

**Purpose**: Systematically evaluate which outcome scenario is most consistent with the evidence and identify the least plausible.
**Process**: Defined 5 hypotheses, evaluated all 20 evidence items against each for consistency/inconsistency/neutrality, tallied inconsistencies, conducted sensitivity analysis.

#### Findings

| Hypothesis | Inconsistencies | Rank |
|------------|----------------|------|
| H1: Complete removal within 2-3 years | 17 | 5th (least plausible) |
| H2: Incremental improvement / managed persistence | 0 | 1st (most plausible) |
| H3: Stall / regress | 2 | 3rd |
| H4: Status quo (null) | 1 | 2nd |
| H5: Deeper access / deception | 0 | 1st (tied) |

H1 (Complete Removal) is **strongly rejected** — 17 of 20 evidence items are inconsistent. H2 (Managed Persistence) is most plausible with zero inconsistencies. H5 (Deeper Access) also has zero inconsistencies but is inherently unfalsifiable from OSINT; the Brickstorm 17-month dwell [E16] and modular deniability architecture [E18] provide circumstantial support.

The sensitivity analysis confirms robustness: even removing the 3-4 most impactful evidence items, H1 retains 13+ inconsistencies.

**Critical gap identified by Premortem**: No intermediate hypothesis for "strategically meaningful partial removal" — the jump from H1 to H2 creates a false binary that biases toward the pessimistic pole.

**Confidence**: MODERATE-HIGH for H2; LOW for H1.

> Full working artifact: [ACH Matrix](working/ach-matrix.md)

---

### 3. Cross-Impact Matrix

**Purpose**: Map how key variables interact, identify reinforcing and balancing feedback loops, and determine the highest-leverage intervention points.
**Process**: Defined 6 variables, populated a 6x6 interaction matrix with direction and magnitude ratings, identified 3 reinforcing loops and 3 balancing loops, analyzed second-order effects.

#### Findings

**Active reinforcing loops (driving degradation)**:
- **R1 — Regulatory Hollowing** (HIGH risk): Weak political will → reduced compliance → persistent complexity → expanded Chinese access → should increase political will but instead produces deregulation [E02]. This is **pathological** — the expected democratic response cycle is inverted.
- **R2 — Capacity Erosion** (MED-HIGH): CISA gutted → reduced industry support → industry resists mandates → political will erodes further.
- **R3 — Adversary Adaptation** (HIGH): Chinese operations steal network diagrams [E09] → increased attack surface → further operations → self-accelerating cycle.

**Dormant/weak balancing loops**:
- **B1 — Threat-Response** (HIGH potential, DORMANT): Expected democratic response broken at V4→V1 link — FCC deregulated after Salt Typhoon [E02].
- **B2 — International Pressure** (MEDIUM, weakly active): Joint advisories issued but effectiveness unclear.
- **B3 — Reputational Pressure** (LOW): Carriers chose PR over remediation [E08].

**Highest-leverage variable**: V1 (Political Will) — drives 4 of 5 other variables but is currently at its weakest state.

**Critical finding**: A **12-18 month vulnerability window** (mid-2026 to 2027) exists where US defensive capacity is at its lowest relative to accelerating threats [E13].

**Confidence**: MODERATE — Systems dynamics well-supported but balancing loop activation probability is uncertain.

> Full working artifact: [Cross-Impact Matrix](working/cross-impact.md)

---

### 4. What If? Analysis

**Purpose**: Challenge the conventional analytic line by imagining a specific disruptive event and tracing backward to assess plausibility.
**Process**: Postulated China activating pre-positioned access during a February 2027 Taiwan Strait crisis, then traced a 5-step backward chain from the event to present conditions.

#### Findings

**Overall plausibility: HIGH.** The backward chain reveals that **every precondition for the scenario is already observable in the evidence base**:

- Step 1 (Present): All conditions set — Salt Typhoon not evicted [E01], CISA depleted [E12], regulations rescinded [E02], adversary operating at scale [E17].
- Step 2: Regulatory rollback removes forcing function for industry remediation [E02, E07].
- Step 3: Structural barriers stall remediation — 200K+ exposed devices, only 25% reduction [E05].
- Step 4: Telecoms declare victory without verification — IOC gap prevents confirmation [E06, E08].
- Step 5: Geopolitical trigger — the **only genuinely uncertain variable** is the Taiwan crisis itself.

The scenario is consistent with historical precedent (Russia's Viasat attack at onset of Ukraine invasion) and China's documented strategic framing of US telecom networks as a "center of gravity in conflict" [E19].

**Confidence**: HIGH for Steps 1-4 (conditions already exist); MEDIUM for Step 5 (geopolitical trigger uncertain).

> Full working artifact: [What If? Analysis](working/what-if.md)

---

### 5. Contrasting Narratives

**Purpose**: Map competing narratives structurally, stress-test each against the evidence base, and identify narrative vulnerabilities.
**Process**: Decomposed 4 narratives into protagonist/antagonist/crisis/resolution elements, evaluated 10 key claims against evidence, rated resilience.

#### Findings

| Narrative | Resilience | Key Assessment |
|-----------|-----------|----------------|
| **Government/Industry: "Making Progress"** | LOW | Carrier eviction claims contradicted by joint advisory [E08]; FCC regulatory rollback undercuts enforcement narrative [E02] |
| **Critics: "Removal Is Impossible"** | HIGH | Broadest and deepest evidentiary support across multiple independent sources [E01, E03-E07, E14] |
| **China: "Politically Motivated"** | LOW | Zero factual support; standard diplomatic response with no analytical weight |
| **Hawks: "Hack Back"** | LOW-MED | Correctly identifies defensive limitations [E10] but core prescription assessed as counterproductive by senior officials [E11] |

**Central finding**: The narrative with the **highest evidential resilience** (critics) has the **least political appeal**, while the narrative with the **most political appeal** (government/industry progress) has the **lowest factual resilience**. This evidence-vs-appeal gap is the central vulnerability in US response.

**Confidence**: HIGH for resilience ratings; assessments well-grounded in evidence stress-testing.

> Full working artifact: [Contrasting Narratives](working/contrasting-narratives.md)

---

### 6. Premortem + Structured Self-Critique

**Purpose**: Imagine catastrophic failure of the assessment and work backward to identify weaknesses, then conduct structured self-critique.
**Process**: Assumed the analysis was proven catastrophically wrong by August 2027, constructed a detailed failure narrative, inventoried 7 weaknesses, performed 5-question self-critique.

#### Findings

**7 weaknesses identified** (3 CRITICAL/HIGH, 4 HIGH/MEDIUM):

| Priority | Weakness | Severity |
|----------|----------|----------|
| 1 | False stability assumption on political will — treated current regulatory rollback as durable | CRITICAL |
| 2 | Linear extrapolation of remediation pace from worst-case 6-month baseline | HIGH |
| 3 | Missing intermediate hypothesis ("strategic denial") in ACH — created false binary | HIGH |
| 4 | Underweighted emerging forcing functions (insurance market, automation, allied enforcement) | HIGH |
| 5 | Treated Chinese offensive resilience as near-absolute — collection gap mistaken for confirmation | MED |
| 6 | Anchored on OPM precedent without testing applicability under changed conditions | MED |
| 7 | Temporally and perspectively skewed evidence base (single-day OSINT, pessimistic sources) | MED |

**Most devastating critique**: "This analysis committed the classic intelligence failure of straight-lining current conditions into the future while cataloguing the reasons it might be wrong but not adjusting the conclusion."

**Strongest counter-argument**: The current regulatory trough is self-correcting — any crisis could trigger emergency mobilization (PATRIOT Act precedent); remediation may be more tractable via risk-prioritization of critical chokepoints (Pareto principle); Chinese contractor ecosystem has demonstrated OPSEC vulnerabilities (i-SOON leak).

**Confidence**: HIGH that the weaknesses identified are genuine; MODERATE that they would produce catastrophic failure.

> Full working artifact: [Premortem + Self-Critique](working/premortem.md)

---

## Synthesis

### Cross-Technique Agreement

All six techniques agree on the core finding: **complete removal of Chinese state-sponsored access from US networks is not achievable within 2-3 years under current conditions.** The evidence is overwhelming (17/20 ACH inconsistencies for H1), structurally reinforced (3 active escalation loops in Cross-Impact), and narratively dominant (critics' assessment rated HIGH resilience). The What If? analysis demonstrates that all preconditions for a worst-case activation scenario already exist.

### Cross-Technique Tension

The Premortem stands in productive tension with the other five techniques. While it does not dispute the assessment under current conditions, it argues the assessment **underweights the probability and impact of condition change** — specifically:

1. **Political will is modeled as static when it behaves as a step-function** capable of rapid phase transitions (KAC and Cross-Impact both identified this but did not propagate it into the headline finding)
2. **The ACH lacks a middle-ground hypothesis** between "complete removal" and "managed persistence," forcing the analysis into a pessimistic binary that the evidence registry itself flagged [E20]
3. **Remediation pace is extrapolated from worst-case conditions**, potentially representing a floor rather than a ceiling

### Integrated Assessment

**Primary judgment (MODERATE confidence)**: Under the current political and institutional trajectory, the US will achieve only incremental progress in reducing Chinese state-sponsored access. The most plausible outcome is managed persistence — learning to detect, contain, and segment rather than fully remove.

**Conditional judgment (MODERATE confidence)**: If a galvanizing crisis triggers political mobilization (estimated 30-40% probability within 18 months), the trajectory could shift significantly. Risk-prioritized remediation of critical chokepoints, combined with mandatory regulation and allied coordination, could achieve "strategic denial" — reducing Chinese access below wartime-useful thresholds without full removal. This intermediate outcome was not formally evaluated in the ACH and represents a gap in the analysis.

**Overall confidence: MODERATE** — adjusted downward from MODERATE-HIGH by the Premortem's valid identification of status quo bias, linear extrapolation risk, and evidence base limitations.

---

## Key Judgments

| # | Judgment | Confidence | Supporting Techniques | Critical Assumptions | Indicators to Watch |
|---|---------|------------|----------------------|---------------------|-------------------|
| J1 | Complete removal of Chinese state-sponsored access is not achievable within 2-3 years under current conditions | HIGH | ACH (17/20 inconsistencies), KAC (4 linchpins), Contrasting Narratives (critics HIGH resilience) | Infrastructure complexity persists; political will remains dormant; Chinese capability continues at scale | Verified independent audit confirming eviction; binding federal mandates enacted; exposed devices drop below 50,000 |
| J2 | Managed persistence is the most plausible outcome — incremental hardening with continued Chinese presence | MODERATE | ACH (H2 = 0 inconsistencies), Cross-Impact (reinforcing loops dominate), KAC (6 supported assumptions) | Current political trajectory continues; telecoms maintain voluntary compliance posture; CISA rebuilds slowly | Rhetorical shift from "removal" to "resilience"; permanent public-private threat sharing created; gradual device reduction |
| J3 | A 12-18 month window of peak vulnerability exists (mid-2026 to 2027) | MODERATE-HIGH | Cross-Impact (SO5), What If? (HIGH plausibility), KAC (Linchpin 2 volatility) | CISA rebuilding proceeds slowly; no mandatory regulation enacted; Chinese operations continue advancing | CISA vacancy rate trajectory; new Chinese campaigns discovered; Taiwan Strait activity levels |
| J4 | Political will is the highest-leverage variable but currently at its weakest state | HIGH | Cross-Impact (V1 drives 4/5 variables), KAC (Linchpin 2 = #1 fault line), Premortem (Weakness #1) | Current deregulatory posture is not permanent | FCC regulatory reversal; Congressional cybersecurity legislation; emergency executive action |
| J5 | The publicly known campaigns may not represent the full extent of Chinese access (H5 unfalsifiable) | LOW-MODERATE | ACH (H5 = 0 inconsistencies), What If? (Step 4 false confidence), Premortem (Weakness #5) | OSINT captures majority of threat landscape | Discovery of campaigns with >24-month dwell; novel implants in previously "cleared" networks; IC assessment of access scope |

---

## Risks & Blind Spots

### Assumption Audit

The analysis rests on several unstated premises identified through the Premortem:
- **Current political trajectory is durable** — treated as stable when KAC and Cross-Impact both flagged it as the most volatile variable
- **Remediation pace is linear** — extrapolated from a single 6-month data point measured under worst-case institutional conditions [E05]
- **Chinese offensive capability is effectively invulnerable** — no challenging evidence found, but this may reflect collection bias rather than confirmed resilience
- **OPM precedent is applicable** — cited across all techniques without testing whether changed conditions (mandatory regulation + allied coordination + offensive pressure) would produce different outcomes

> **Optional follow-up**: Re-run the analysis with an explicit "political activation" branch scenario. Construct two conditional assessments — one under continued current trajectory, one under emergency mobilization — and assign probabilities to each branch. This would transform the single-point estimate into a probability-weighted range that better captures political volatility.

### Evidence Balance

Evidence skews ~4:1 pessimistic (16 items supporting difficulty/impossibility vs. 3-4 noting emerging positive signals). This imbalance is **partially justified** by source quality (pessimistic items are better-evidenced) but **also reflects collection bias**: single-day OSINT from Western sources with structural pessimistic framing. The evidence registry acknowledged this but downstream techniques did not adequately adjust.

> **Optional follow-up**: Conduct a targeted "red collection" round specifically seeking evidence that challenges the pessimistic consensus — carrier remediation progress reports, cybersecurity investment disclosures, insurance market shifts, DARPA automation program updates, and allied coordination outcomes. If 5+ items with Medium or High reliability are found supporting accelerated remediation, re-evaluate J1 and J2 confidence levels.

### Confidence Calibration

Initial MODERATE-HIGH confidence was **adjusted to MODERATE** after the Premortem identified status quo anchoring as a systematic bias. The analysis catalogued reasons the assessment might be wrong (KAC failure conditions, Cross-Impact balancing loops, Contrasting Narratives' institutional momentum) without propagating those into the headline finding — a form of "acknowledging uncertainty without acting on it."

> **Optional follow-up**: Re-run the ACH matrix adding hypothesis H1.5 ("Strategic denial — Chinese persistent access reduced below wartime-useful threshold without full removal"). Evaluate all 20 evidence items against H1.5. If H1.5 collects fewer inconsistencies than H2, the headline assessment should be revised upward and the confidence recalibrated accordingly.

### Strongest Counter-Argument

The current regulatory trough and institutional degradation represent a political nadir, not a steady state. US cybersecurity policy operates in crisis-driven cycles. Any galvanizing event — a leaked classified briefing, visible infrastructure disruption, Taiwan escalation — could produce emergency legislation within weeks (PATRIOT Act precedent). Simultaneously, risk-prioritized remediation focusing on critical chokepoints (perhaps 15-20% of infrastructure carrying 80% of strategic risk) could achieve strategically meaningful denial faster than the "patch everything" frame implies. The Premortem estimates 30-40% probability of political activation within 18 months.

> **Optional follow-up**: Research historical remediation acceleration under crisis conditions — compare PCI-DSS implementation timelines post-TJX breach, NERC CIP enforcement effects on power grid security, and SOX compliance velocity post-Enron. If historical precedents show >3x acceleration under mandatory regimes, model a non-linear remediation scenario and assess whether "strategic denial" becomes plausible within 18-24 months.

### Missing Perspectives

The following domains represent blind spots where no evidence was collected. Each gap has a potential impact on the assessment and a recommended collection action.

| Gap | Impact on Assessment | Recommended Collection Action |
|-----|---------------------|-------------------------------|
| **Telecom industry internal remediation** — no carrier statements on progress or investment | May understate actual remediation progress; carrier actions are invisible to external observers | Search carrier SEC filings (10-K cybersecurity risk disclosures), earnings call transcripts, and industry group publications (USTelecom, CTIA) for investment signals |
| **Cyber insurance market** — potentially powerful forcing function not in evidence base | Insurance-driven compliance pressure could accelerate remediation independent of government regulation (Premortem identified this as a key missed variable) | Search for Marsh McLennan, Aon, and Swiss Re cyber insurance market reports; check for telecom-specific underwriting changes post-Salt Typhoon |
| **Defense industrial base** — CrowdStrike/Mandiant remediation outcomes unknown | Incident response firms have direct visibility into actual remediation progress across carrier networks | Review CrowdStrike annual threat reports, Mandiant M-Trends, and Palo Alto Unit 42 publications for Salt Typhoon-specific remediation data |
| **Chinese strategic calculus** — beyond diplomatic denial, no analysis of conditions under which China might reduce operations | Assessment may overstate adversary persistence by ignoring potential Chinese restraint under specific conditions | Search Chinese strategic studies journals, PLA-affiliated publications, and academic analyses of Chinese cyber doctrine for signaling about operational ceilings |
| **Allied coordination effectiveness** — Five Eyes outcomes not documented | May understate the impact of multilateral pressure; allied enforcement (EU CRA) could create compliance cascading effects | Review Five Eyes joint advisories, NATO CCDCOE publications, and EU Agency for Cybersecurity (ENISA) reports for coordination effectiveness data |
| **Classified intelligence** — OSINT-only analysis cannot assess full scope of compromise or remediation | Likely understates both the threat (deeper access unknown) and the response (classified defensive programs invisible) | This gap cannot be closed from OSINT; flag for analysts with appropriate clearances to cross-reference |
| **DARPA/defense automation** — AI-assisted vulnerability scanning and patching programs not evaluated | May understate the pace of future remediation if automated tools reduce the human labor bottleneck | Search DARPA program announcements (Cyber Grand Challenge follow-ons), DoD CDAO publications, and defense contractor press releases |

> **Recommended next iteration**: Prioritize collection on the top 3 gaps (telecom industry, cyber insurance, defense industrial base) within 90 days. These are the most likely to contain evidence that would challenge the pessimistic baseline and are accessible via OSINT. Schedule a reassessment after collection is complete.

---

## Monitoring Plan

The analysis identifies **5 key judgments** requiring ongoing monitoring. The most time-sensitive is J3 (12-18 month vulnerability window), which creates urgency for near-term indicator tracking. The highest-leverage monitoring target is J4 (political will), where a single event could cascade through the entire assessment.

**Top indicators to watch**:
1. **Censys exposed device count** — benchmark: 200,000+; trigger: drop below 100,000 in 6 months
2. **CISA vacancy rate** — benchmark: ~40%; trigger: restoration below 15% or failure to improve by FY2026 end
3. **Congressional/FCC regulatory action** — benchmark: voluntary framework; trigger: binding mandates enacted
4. **New Chinese campaigns discovered** — benchmark: Brickstorm (Dec 2025); trigger: novel implant families in "cleared" networks
5. **Taiwan Strait activity** — benchmark: elevated; trigger: military exercises or diplomatic crisis escalation

> Full monitoring plan: [Monitoring Plan](monitoring-plan.md)

---

## Sources

| Ref | Source | Retrieved | Reliability | Method |
|-----|--------|-----------|-------------|--------|
| [E01] | [Is America's Cyber Weakness Self-Inflicted? — War on the Rocks](https://warontherocks.com/2026/01/is-americas-cyber-weakness-self-inflicted/) | 2026-02-15 | High | OSINT |
| [E02] | [Is America's Cyber Weakness Self-Inflicted? — War on the Rocks](https://warontherocks.com/2026/01/is-americas-cyber-weakness-self-inflicted/) | 2026-02-15 | High | OSINT |
| [E03] | [Is America's Cyber Weakness Self-Inflicted? — War on the Rocks](https://warontherocks.com/2026/01/is-americas-cyber-weakness-self-inflicted/) | 2026-02-15 | High | OSINT |
| [E04] | [Why telecoms may never purge Salt Typhoon — CyberScoop](https://cyberscoop.com/salt-typhoon-chinese-hackers-us-telecom-breach/) | 2026-02-15 | High | OSINT |
| [E05] | [Why telecoms may never purge Salt Typhoon — CyberScoop/Censys](https://cyberscoop.com/salt-typhoon-chinese-hackers-us-telecom-breach/) | 2026-02-15 | High | OSINT |
| [E06] | [Why telecoms may never purge Salt Typhoon — CyberScoop](https://cyberscoop.com/salt-typhoon-chinese-hackers-us-telecom-breach/) | 2026-02-15 | Medium | OSINT |
| [E07] | [Why telecoms may never purge Salt Typhoon — CyberScoop/U. Florida](https://cyberscoop.com/salt-typhoon-chinese-hackers-us-telecom-breach/) | 2026-02-15 | High | OSINT |
| [E08] | [Reconfiguring U.S. Cyber Strategy in the Wake of Salt Typhoon — Lawfare](https://www.lawfaremedia.org/article/reconfiguring-u.s.-cyber-strategy-in-the-wake-of-salt-typhoon) | 2026-02-15 | High | OSINT |
| [E09] | [Reconfiguring U.S. Cyber Strategy in the Wake of Salt Typhoon — Lawfare](https://www.lawfaremedia.org/article/reconfiguring-u.s.-cyber-strategy-in-the-wake-of-salt-typhoon) | 2026-02-15 | High | OSINT |
| [E10] | [Reconfiguring U.S. Cyber Strategy in the Wake of Salt Typhoon — Lawfare](https://www.lawfaremedia.org/article/reconfiguring-u.s.-cyber-strategy-in-the-wake-of-salt-typhoon) | 2026-02-15 | High | OSINT |
| [E11] | [Reconfiguring U.S. Cyber Strategy in the Wake of Salt Typhoon — Lawfare](https://www.lawfaremedia.org/article/reconfiguring-u.s.-cyber-strategy-in-the-wake-of-salt-typhoon) | 2026-02-15 | High | OSINT |
| [E12] | [CISA plans hiring spree to rebuild depleted ranks — Cybersecurity Dive](https://www.cybersecuritydive.com/news/cisa-hiring-workforce-strategy/805733/) | 2026-02-15 | High | OSINT |
| [E13] | [CISA plans hiring spree to rebuild depleted ranks — Cybersecurity Dive](https://www.cybersecuritydive.com/news/cisa-hiring-workforce-strategy/805733/) | 2026-02-15 | High | OSINT |
| [E14] | [China's Typhoon hackers have changed the rules — SC World](https://www.scworld.com/perspective/chinas-typhoon-hackers-have-changed-the-rules-of-cybersecurity) | 2026-02-15 | Medium | OSINT |
| [E15] | [China's Typhoon hackers have changed the rules — SC World](https://www.scworld.com/perspective/chinas-typhoon-hackers-have-changed-the-rules-of-cybersecurity) | 2026-02-15 | Medium | OSINT |
| [E16] | [Chinese-linked hackers use Brickstorm backdoor — Reuters](https://www.reuters.com/world/china/chinese-linked-hackers-use-back-door-potential-sabotage-us-canada-say-2025-12-04/) | 2026-02-15 | High | OSINT |
| [E17] | [Inside Salt Typhoon — DomainTools](https://dti.domaintools.com/research/inside-salt-typhoon-chinas-state-corporate-advanced-persistent-threat) | 2026-02-15 | High | OSINT |
| [E18] | [Inside Salt Typhoon — DomainTools](https://dti.domaintools.com/research/inside-salt-typhoon-chinas-state-corporate-advanced-persistent-threat) | 2026-02-15 | High | OSINT |
| [E19] | [Advancing IP Interconnection — FDD](https://www.fdd.org/analysis/2025/12/16/advancing-ip-interconnection/) | 2026-02-15 | High | OSINT |
| [E20] | Analyst-provided framing note | 2026-02-15 | N/A | USER |

---

*Generated by Structured Analysis Skill | 2026-02-15 | Analysis ID: 2026-02-15-us-chinese-hackers-network-removal*
