# Analysis of Competing Hypotheses: Will the US Successfully Remove Chinese Hackers from Their Networks?

> **Analysis ID**: 2026-02-15-us-chinese-hackers-network-removal | **Date**: 2026-02-15 | **Phase**: Diagnostic
> **Evidence base**: 20 items | [Full registry](../evidence-registry.md)

---

## Problem Statement

What is the range of plausible outcomes for US efforts to detect, remove, and prevent future Chinese state-sponsored cyber intrusions across critical infrastructure — particularly telecommunications networks — over the next 2-5 years?

This inquiry encompasses the technical feasibility of removal, the political and institutional capacity to sustain the effort, the adversary's ability to adapt and re-penetrate, and the possibility that current public understanding underestimates the depth of compromise.

---

## Hypotheses

| ID | Hypothesis | Source / Basis |
|----|-----------|----------------|
| H1 | **Complete removal succeeds within 2-3 years** through mandatory regulation, massive investment, and offensive cyber deterrence | Optimistic policy scenario — assumes political will, regulatory authority, and technical capacity align |
| H2 | **Incremental improvement occurs but full removal is never achieved** — US learns to "manage" persistent Chinese presence | Moderate scenario — consistent with historical cyber conflict patterns (E10) and expert assessments (E04) |
| H3 | **Removal efforts stall or regress** due to political dysfunction, CISA capacity loss, and industry resistance to mandates | Pessimistic institutional scenario — grounded in observed CISA workforce losses (E12) and FCC regulatory rollback (E02) |
| H4 | **Status quo continues (Null)** — neither significant improvement nor deterioration; the problem persists at roughly current levels | Null hypothesis — baseline against which change is measured |
| H5 | **China has already achieved deeper access than publicly known (Deception)** — visible "Typhoon" campaigns are diversionary while more covert operations remain undetected | Denial/deception scenario — motivated by contractor-enabled modular operations model (E17, E18) and Brickstorm long-dwell discovery (E16) |

---

## Evidence Inventory

| ID | Evidence | Source | Reliability | Citation |
|----|----------|--------|-------------|----------|
| E01 | Salt Typhoon exploited basic failures (weak passwords, 7-year-old unpatched Cisco vulnerabilities), not zero-days. Telecoms "still have not convincingly shown they have evicted the intruders" as of Dec 2025. | War on the Rocks | High | [1] |
| E02 | FCC rescinded binding cybersecurity orders for telecom carriers in late 2025, replacing them with voluntary industry collaboration. | War on the Rocks | High | [1] |
| E03 | Salt Typhoon exploited US-made equipment maintenance failures, not Chinese hardware. Rip-and-replace does not address the actual attack vector. | War on the Rocks | High | [1] |
| E04 | Multiple experts assess telecoms "may never fully expunge" Salt Typhoon due to network size/complexity, identity management difficulty, and decades of consolidation creating "Frankenstein's monster" architecture. | CyberScoop | High | [2] |
| E05 | Censys found 200,000+ public exposures of vulnerable network/edge devices; only 25% reduction in six months despite growing awareness. | CyberScoop/Censys | High | [2] |
| E06 | Lack of confirmed, granular IOCs for Salt Typhoon makes reliable threat hunting difficult. | CyberScoop | Medium | [2] |
| E07 | University of Florida researchers found 100+ exploitable vulnerabilities in LTE/5G implementations; maintainers lacked personnel or expertise to patch. | CyberScoop/U. Florida | High | [2] |
| E08 | FBI: Salt Typhoon obtained 1M+ call records; 600+ companies impacted. AT&T/Verizon eviction claims contradicted by joint advisory confirming "persistent, long-term access." | Lawfare | High | [3] |
| E09 | Salt Typhoon compromised Army National Guard network (Mar-Dec 2024), stealing admin credentials and network diagrams, which were then used to compromise another government agency. | Lawfare | High | [3] |
| E10 | After OPM breach (2015), diplomatic response produced only temporary subsidence; Chinese cyber operations surged by 2017. | Lawfare | High | [3] |
| E11 | Hack-back operations do not remove vulnerabilities; US presidents "lack enough confidence that U.S. defenses could withstand a potentially escalatory tit-for-tat battle in cyberspace." | Lawfare | High | [3] |
| E12 | CISA lost 1/3+ of its workforce under Trump administration; ~40% vacancy rate across key mission areas. Key experts pushed out, partnerships frozen. | Cybersecurity Dive | High | [4] |
| E13 | CISA aims to hire qualified professionals by end of FY2026; experts predict potential US-China crisis in 2027, creating timeline pressure. | Cybersecurity Dive | High | [4] |
| E14 | Typhoon campaigns exploited flaws across Cisco, F5, Ivanti, Fortinet, Sonicwall, Netgear, Citrix, and Juniper — broad multi-vendor attack surface. | SC World | Medium | [5] |
| E15 | Emerging regulatory responses: EU Cyber Resilience Act, US DoD SWFT Initiative (May 2025), UK Government Cyber Action Plan. "End of three-decade-long hands-off approach to software security." | SC World | Medium | [5] |
| E16 | "Brickstorm" malware maintained 17-month undetected access (Apr 2024-Sep 2025) in government and IT entities, targeting VMware vSphere. | Reuters | High | [6] |
| E17 | Salt Typhoon = MSS-aligned dual-use capability (espionage + wartime disruption). Contractor-enabled model using i-SOON, Sichuan Juxinhe, Beijing Huanyu Tianqiong. 600+ orgs breached worldwide, 200+ in US, across 80+ countries. Active since 2019. | DomainTools | High | [7] |
| E18 | Salt Typhoon leverages "contractor-enabled, industrial-scale operations" with modular tooling and built-in deniability at every layer. "China's next-generation cyber espionage model." | DomainTools | High | [7] |
| E19 | Legacy TDM infrastructure in FAA radar and railroad systems creates persistent vulnerability. China "perceives U.S. telecommunications networks as a key center of gravity in a possible conflict." FCC removed cybersecurity requirements post-Salt Typhoon. | FDD | High | [8] |
| E20 | Analytical framing note: binary success/failure framing may obscure partial or incremental outcomes. | Analyst | N/A | [9] |

---

## Diagnosticity Matrix

| Evidence | H1 (Complete Removal) | H2 (Incremental/Manage) | H3 (Stall/Regress) | H4 (Status Quo) | H5 (Deeper Access/Deception) | Diagnostic Value |
|----------|----------------------|------------------------|--------------------|-----------------|-----------------------------|------------------|
| E01 | I | C | C | C | C | **HIGH** |
| E02 | I | N | C | C | N | **HIGH** |
| E03 | I | C | C | N | C | **HIGH** |
| E04 | I | C | C | C | C | **HIGH** |
| E05 | I | C | C | C | C | **HIGH** |
| E06 | I | C | C | C | C | **HIGH** |
| E07 | I | C | C | C | C | **HIGH** |
| E08 | I | C | C | C | C | **HIGH** |
| E09 | I | N | C | C | C | **HIGH** |
| E10 | I | C | N | C | C | **HIGH** |
| E11 | I | C | C | C | C | **HIGH** |
| E12 | I | N | C | N | N | **HIGH** |
| E13 | N | C | I | N | N | **HIGH** |
| E14 | I | C | C | C | C | **HIGH** |
| E15 | C | C | I | I | N | **HIGH** |
| E16 | I | C | C | C | C | **HIGH** |
| E17 | I | C | C | C | C | **HIGH** |
| E18 | I | C | C | C | C | **HIGH** |
| E19 | I | C | C | C | C | **HIGH** |
| E20 | N | C | N | N | N | **LOW** |

### Legend

| Rating | Meaning |
|--------|---------|
| **C** | Consistent -- evidence supports or is expected under this hypothesis |
| **I** | Inconsistent -- evidence contradicts or would be surprising under this hypothesis |
| **N** | Neutral -- evidence neither supports nor contradicts |

### Rating Rationale for Key Judgments

- **E01 vs H1 (I)**: Telecoms failing to demonstrate eviction after 14+ months directly contradicts the premise that complete removal is feasible within 2-3 years [E01].
- **E02 vs H1 (I)**: Rescinding binding cybersecurity orders removes the regulatory mechanism H1 requires [E02].
- **E03 vs H1 (I)**: The mismatch between policy focus (rip-and-replace Chinese hardware) and actual attack vector (US equipment maintenance failures) means current policy does not address the root vulnerability [E03].
- **E10 vs H1 (I)**: Historical precedent shows deterrence-based approaches produce only temporary subsidence, not lasting removal [E10].
- **E11 vs H1 (I)**: Senior officials themselves doubt US capacity to sustain an escalatory cyber exchange, undermining the offensive deterrence pillar of H1 [E11].
- **E13 vs H3 (I)**: CISA's active hiring push and FY2026 timeline represent institutional effort that partially contradicts the "stall/regress" narrative [E13].
- **E15 vs H3 (I)**: Emerging regulatory frameworks (SWFT, EU CRA) indicate policy movement, partially contradicting complete stagnation under H3 [E15].
- **E15 vs H4 (I)**: The same regulatory momentum contradicts a pure status quo scenario [E15].
- **E16 vs H5 (C)**: Discovery of a previously unknown 17-month persistent access campaign is consistent with the hypothesis that deeper, undetected access exists [E16].

> **Law of Diagnostic Dominance**: No evidence item received C or N across ALL five hypotheses. All 20 items carry nonzero diagnostic value. However, E20 has low discriminatory power (N for H1, H3, H4, H5; C only for H2) and is retained primarily for analytical hygiene.

---

## Inconsistency Tally

| Hypothesis | Inconsistent Items | Score | Rank |
|------------|-------------------|-------|------|
| H1 (Complete Removal) | E01, E02, E03, E04, E05, E06, E07, E08, E09, E10, E11, E12, E14, E16, E17, E18, E19 | **17** | 5 (least plausible) |
| H2 (Incremental/Manage) | *(none)* | **0** | 1 (most plausible) |
| H3 (Stall/Regress) | E13, E15 | **2** | 3 |
| H4 (Status Quo/Null) | E15 | **1** | 2 |
| H5 (Deeper Access/Deception) | *(none)* | **0** | 1 (tied) |

**Interpretation**: H1 (Complete Removal) is overwhelmingly inconsistent with the evidence base -- 17 of 20 items contradict it. H2 (Incremental/Manage) and H5 (Deeper Access/Deception) have zero inconsistencies, making them the most plausible on a raw tally. However, the tie between H2 and H5 requires careful interpretation:

- **H2** is a moderate, well-supported outcome scenario with extensive direct evidence.
- **H5** has zero inconsistencies because it is inherently difficult to disprove (unfalsifiability concern). The absence of contradictory evidence for a deception hypothesis does not confirm it -- it merely reflects the epistemic gap. E16 (Brickstorm 17-month dwell) and E18 (modular deniability architecture) provide circumstantial support but not confirmation.

H4 (Status Quo) carries only 1 inconsistency, but the emerging regulatory signals (E15) suggest at least some change from the baseline, making pure status quo slightly less tenable than managed persistence (H2).

---

## Sensitivity Analysis

| Critical Evidence | If Removed / Reinterpreted | Impact on Conclusion |
|-------------------|----------------------------|----------------------|
| E01 (telecoms not evicted) | If telecoms provided credible proof of eviction | H1 inconsistency count drops by 1; still overwhelming. H2 weakened slightly. Core conclusion unchanged. |
| E02 (FCC regulatory rollback) | If FCC reverses course and reinstates binding rules | H1 slightly less implausible; H3 weakened. Shifts balance modestly toward H1/H2 overlap. |
| E04 (experts say "may never purge") | If this expert consensus shifted to "difficult but achievable" | H1 inconsistency drops by 1; H2 narrative weakened. Would be the single most impactful reinterpretation -- but would require multiple independent experts to reverse assessment. |
| E12 (CISA workforce gutted) | If CISA successfully rebuilds to full capacity by end FY2026 | H3 loses primary support pillar. H2/H1 boundary scenario becomes more plausible. This is the most policy-responsive evidence item. |
| E15 (emerging regulations) | If all regulatory initiatives stall or are repealed | H3 gains strength. H4 becomes more plausible. H1 becomes even more implausible. |
| E16 (Brickstorm 17-month dwell) | If Brickstorm attributed to non-Chinese actor | H5 loses key supporting evidence. Does not change H1-H4 rankings materially. |
| E05 (200K+ exposed devices, only 25% reduction) | If remediation rate accelerated dramatically (e.g., 75%+ reduction) | H1 becomes marginally less implausible; H2 strengthened. Would require evidence of industry-wide patch acceleration not currently observed. |

**Key sensitivity finding**: The conclusion that H1 is implausible is robust. Even removing the 3-4 most impactful evidence items, H1 retains 13+ inconsistencies. The H2-vs-H5 discrimination is the most sensitive judgment -- it depends heavily on whether the intelligence community can confirm or deny deeper undetected penetration, which is inherently difficult to assess from OSINT alone.

---

## Future Indicators

### Indicators That Would Increase Confidence in H1 (Complete Removal)
- Verified, independent audit confirming Salt Typhoon eviction from major telecoms
- FCC or Congress enacts binding cybersecurity mandates with enforcement mechanisms and penalties
- Sustained CISA hiring achieves <10% vacancy rate across mission areas
- Measurable reduction in exposed vulnerable devices to <50,000 (from 200,000+)
- No new Chinese APT campaigns discovered in US critical infrastructure for 12+ consecutive months

### Indicators That Would Increase Confidence in H2 (Incremental/Manage)
- Telecoms acknowledge persistent risk but demonstrate measurable reduction in dwell times
- US government shifts rhetoric from "removal" to "resilience" and "risk management"
- Creation of permanent public-private threat sharing mechanisms focused on telecom sector
- Gradual reduction in exposed devices (to ~100,000) without achieving full remediation

### Indicators That Would Increase Confidence in H3 (Stall/Regress)
- CISA hiring targets missed; vacancy rate remains >30% through FY2027
- Additional regulatory rollbacks or Congressional failure to act on cybersecurity legislation
- Discovery of new major Chinese intrusion campaigns exploiting previously disclosed vulnerabilities
- Industry lobbying successfully blocks mandatory cybersecurity requirements

### Indicators That Would Increase Confidence in H4 (Status Quo)
- No significant policy changes in either direction through 2027
- Exposed device count remains approximately stable (~150,000-200,000)
- Periodic discovery of Chinese intrusions at roughly current cadence

### Indicators That Would Increase Confidence in H5 (Deeper Access/Deception)
- Discovery of Chinese APT campaigns with dwell times >24 months in previously "cleared" networks
- Revelation of novel Chinese implants in infrastructure not covered by Typhoon advisories (e.g., power grid SCADA, water systems, financial networks)
- Intelligence community assessment that known campaigns represent <50% of actual Chinese access
- Discovery of Chinese exploitation of supply chain compromises in US-manufactured networking equipment

---

## Preliminary Findings

- **Most plausible hypothesis**: **H2** -- Incremental improvement occurs but full removal is never achieved; the US learns to "manage" persistent Chinese presence.
- **Confidence**: **Moderate-High**
- **Rationale**: H2 is consistent with all 20 evidence items and zero inconsistencies. The evidence overwhelmingly shows that: (a) the technical scale of compromise is vast and deeply embedded in legacy architecture [E04, E05, E07, E14, E19]; (b) the adversary operates an industrial-scale, modular, contractor-enabled model that can adapt faster than US remediation [E17, E18]; (c) the policy and institutional apparatus needed for complete removal is being weakened rather than strengthened [E02, E12]; and (d) historical precedent demonstrates that even successful diplomatic/punitive responses produce only temporary reduction in Chinese operations [E10]. The one area of forward movement -- emerging regulatory frameworks [E15] and CISA rebuilding efforts [E13] -- supports incremental improvement rather than complete removal or total stagnation.

- **Runner-up**: **H5** (Deeper Access/Deception) cannot be ruled out and warrants continuous monitoring. The Brickstorm discovery [E16], the modular deniability architecture [E18], and the six-year operational history of Salt Typhoon [E17] all suggest that the publicly known campaigns may not represent the full extent of Chinese access. This hypothesis is inherently difficult to disprove from OSINT sources.

- **Key uncertainty**: The H2-vs-H5 boundary is the most analytically consequential ambiguity. If H5 is closer to reality, the "managed persistence" framing of H2 understates the threat and the US may be managing a visible subset of a much larger problem.

- **Rejected**: **H1** (Complete Removal) is strongly rejected with 17 of 20 evidence items inconsistent. No plausible near-term scenario achieves complete removal given current technical, institutional, and political conditions.

---

## Sources & Citations

| Ref | Source | Retrieved | Reliability | Method |
|-----|--------|-----------|-------------|--------|
| [1] | [Is America's Cyber Weakness Self-Inflicted? -- War on the Rocks](https://warontherocks.com/2026/01/is-americas-cyber-weakness-self-inflicted/) | 2026-02-15 | High | OSINT |
| [2] | [Why telecoms may never purge Salt Typhoon -- CyberScoop](https://cyberscoop.com/salt-typhoon-chinese-hackers-us-telecom-breach/) | 2026-02-15 | High | OSINT |
| [3] | [Reconfiguring U.S. Cyber Strategy in the Wake of Salt Typhoon -- Lawfare](https://www.lawfaremedia.org/article/reconfiguring-u.s.-cyber-strategy-in-the-wake-of-salt-typhoon) | 2026-02-15 | High | OSINT |
| [4] | [CISA plans hiring spree to rebuild depleted ranks -- Cybersecurity Dive](https://www.cybersecuritydive.com/news/cisa-hiring-workforce-strategy/805733/) | 2026-02-15 | High | OSINT |
| [5] | [China's Typhoon hackers have changed the rules -- SC World](https://www.scworld.com/perspective/chinas-typhoon-hackers-have-changed-the-rules-of-cybersecurity) | 2026-02-15 | Medium | OSINT |
| [6] | [Chinese-linked hackers use Brickstorm backdoor -- Reuters](https://www.reuters.com/world/china/chinese-linked-hackers-use-back-door-potential-sabotage-us-canada-say-2025-12-04/) | 2026-02-15 | High | OSINT |
| [7] | [Inside Salt Typhoon -- DomainTools](https://dti.domaintools.com/research/inside-salt-typhoon-chinas-state-corporate-advanced-persistent-threat) | 2026-02-15 | High | OSINT |
| [8] | [Advancing IP Interconnection -- FDD](https://www.fdd.org/analysis/2025/12/16/advancing-ip-interconnection/) | 2026-02-15 | High | OSINT |
| [9] | Analyst-provided framing note | 2026-02-15 | N/A | USER |

**Citation Methods:**
- **OSINT**: Retrieved via Firecrawl MCP or WebSearch/WebFetch fallback
- **FILE**: Read from local file system
- **USER**: Provided by analyst in conversation
- **ANALYSIS**: Analytical judgment produced via named technique protocol

> Every claim must be cited. Uncited claims are not permitted.

---

*Generated by Structured Analysis Skill -- ACH Protocol | 2026-02-15*
