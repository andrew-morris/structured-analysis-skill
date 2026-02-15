# Key Assumptions Check: US Removal of Chinese Hackers from Networks

> **Analysis ID**: 2026-02-15-us-chinese-hackers-network-removal | **Date**: 2026-02-15 | **Phase**: Diagnostic
> **Evidence base**: 20 items | [Full registry](../evidence-registry.md)

---

## Current Analytic Line

The US will make incremental progress in hardening networks and detecting intrusions, but will not achieve complete removal of Chinese state-sponsored hackers from telecommunications and critical infrastructure networks within the next 2-3 years, due to the scale and complexity of US telecom infrastructure, persistent institutional and political obstacles, and the adversary's industrial-scale offensive capabilities.

---

## Assumption Inventory

| # | Assumption | Stated / Unstated | Category | Bin (S/C/U) | Linchpin? |
|---|-----------|-------------------|----------|-------------|-----------|
| 1 | US telecom infrastructure is too large and complex for comprehensive remediation within 2-3 years | Stated | Technical | S | Y |
| 2 | Institutional and political obstacles will persist and impede a coordinated national response | Stated | Political/Institutional | S | Y |
| 3 | China's offensive cyber capabilities operate at industrial scale and will continue to do so | Stated | Adversary | S | Y |
| 4 | The US will make at least incremental progress (i.e., the situation will not get dramatically worse) | Stated | Defensive | C | N |
| 5 | "Complete removal" is the correct benchmark for evaluating success | Unstated | Framing | C | N |
| 6 | Current detection and attribution capabilities are insufficient to identify all Chinese footholds | Unstated | Technical | S | N |
| 7 | Telecom companies lack sufficient incentive and capacity to self-remediate without regulatory compulsion | Unstated | Economic/Regulatory | S | Y |
| 8 | Offensive cyber operations ("hack back") cannot substitute for defensive remediation | Unstated | Strategic | S | N |
| 9 | The adversary will not voluntarily withdraw or reduce operations due to diplomatic or deterrent pressure | Unstated | Adversary Intent | C | Y |
| 10 | Allied and international coordination will not produce a step-change in defensive effectiveness | Unstated | International | U | N |

**Bin Key:** S = Supported | C = Correct with Caveats | U = Unsupported

---

## Detailed Assumption Evaluation

### Assumption 1: US telecom infrastructure is too large and complex for comprehensive remediation within 2-3 years

- **Why assumed true**: US telecommunications networks are the product of decades of industry consolidation, creating what experts call "Frankenstein's monster of different equipment, technologies and architecture" [E04]. Censys identified 200,000+ publicly exposed network devices with Salt Typhoon-exploitable vulnerabilities, and exposure declined only 25% over six months despite heightened awareness [E05]. Legacy TDM infrastructure persists in critical systems such as FAA radar and railroad networks [E19].
- **Does it hold under all conditions?** Yes, under current conditions. No credible scenario exists for wholesale infrastructure replacement in 2-3 years. Even targeted remediation faces the problem that University of Florida researchers found 100+ distinct exploitable vulnerabilities in LTE/5G implementations, and maintainers "simply lacked the personnel or expertise to address the flaws" [E07].
- **Was it true in the past but less so now?** No -- the problem has worsened over time as networks grew more complex and legacy systems accumulated. The Salt Typhoon breach exploited seven-year-old unpatched Cisco vulnerabilities [E03], indicating the remediation backlog is growing, not shrinking.
- **Evidence supporting**: E03, E04, E05, E07, E19
- **Evidence challenging**: E15 (emerging regulatory responses including EU Cyber Resilience Act and US DoD SWFT Initiative suggest a policy shift toward mandatory security standards, which could accelerate remediation if implemented)
- **Bin**: **S (Supported)** -- Multiple independent sources confirm the scale problem with quantitative evidence.

### Assumption 2: Institutional and political obstacles will persist and impede a coordinated national response

- **Why assumed true**: The FCC rescinded binding cybersecurity orders for telecom carriers in late 2025, replacing them with a voluntary framework [E02]. CISA lost more than one-third of its workforce, with approximately 40% vacancy rates across key mission areas [E12]. These are not hypothetical risks but documented current conditions.
- **Does it hold under all conditions?** It could change if a major crisis (e.g., a Chinese cyberattack during a Taiwan contingency) created political will for emergency action. However, the OPM breach precedent shows that even major incidents produce only temporary policy responses before operations resume [E10].
- **Was it true in the past but less so now?** It has arguably worsened. The regulatory rollback is a reversal from the direction of travel in 2024, and workforce losses at CISA are recent developments [E02, E12].
- **Evidence supporting**: E02, E10, E12, E13, E19
- **Evidence challenging**: E13 (CISA is planning a hiring spree to rebuild by end of FY2026), E15 (the DoD SWFT Initiative and allied regulatory efforts suggest some institutional momentum toward stronger security requirements)
- **Bin**: **S (Supported)** -- Current policy direction and institutional degradation are well-documented, though rebuilding efforts introduce some uncertainty.

### Assumption 3: China's offensive cyber capabilities operate at industrial scale and will continue to do so

- **Why assumed true**: Salt Typhoon operates through a "contractor-enabled, industrial-scale" model using MSS-aligned front companies (i-SOON, Sichuan Juxinhe, Beijing Huanyu Tianqiong) with modular tooling and built-in deniability [E17, E18]. The operation breached 600+ organizations worldwide, 200+ in the US, across 80+ countries, and has been active since 2019 [E17]. New malware families like "Brickstorm" continue to emerge, maintaining long-term access for 17+ months in documented cases [E16].
- **Does it hold under all conditions?** It could be disrupted by internal Chinese political upheaval, a severe economic crisis reducing funding, or successful US offensive operations dismantling contractor infrastructure. None of these are likely in the 2-3 year timeframe.
- **Was it true in the past but less so now?** No -- the trend is toward increasing capability. After the OPM breach in 2015, operations "temporarily subsided" but surged by 2017, indicating resilience and growth [E10].
- **Evidence supporting**: E10, E16, E17, E18
- **Evidence challenging**: None in the evidence registry directly challenges this assumption.
- **Bin**: **S (Supported)** -- The industrial-scale model is well-documented and shows a trajectory of increasing capability.

### Assumption 4: The US will make at least incremental progress

- **Why assumed true**: The analytic line asserts "incremental progress" as a floor. Some evidence supports this: emerging regulatory frameworks [E15], CISA's planned hiring spree [E13], and heightened awareness across the sector.
- **Does it hold under all conditions?** Not necessarily. If CISA's rebuilding fails, if regulatory rollbacks continue, and if the adversary adapts faster than defenders, the net security posture could deteriorate. The 25% reduction in exposed devices over six months [E05] is progress, but it may be outpaced by new vulnerability discovery [E07] and new malware deployment [E16].
- **Was it true in the past but less so now?** The incremental progress assumption is questionable given that the FCC moved backward by rescinding binding requirements [E02] and CISA's workforce has been gutted [E12].
- **Evidence supporting**: E05 (25% reduction in exposures), E13 (CISA hiring plans), E15 (regulatory momentum)
- **Evidence challenging**: E02 (regulatory rollback), E12 (CISA workforce losses), E16 (continued adversary innovation)
- **Bin**: **C (Correct with Caveats)** -- Progress is possible but not guaranteed; current institutional degradation could stall or reverse gains.

### Assumption 5: "Complete removal" is the correct benchmark for evaluating success

- **Why assumed true**: The original question frames the problem as binary ("will the US be able to successfully remove...") [E20]. The analytic line implicitly treats complete removal as the standard against which the US will fall short.
- **Does it hold under all conditions?** This framing may obscure meaningful partial outcomes. A more useful benchmark might be whether the US can reduce Chinese access to a level that denies strategic intelligence collection and pre-positioned sabotage capabilities.
- **Evidence supporting**: E20 (analyst notes the binary framing problem)
- **Evidence challenging**: E20 itself challenges this framing by noting it "may obscure partial or incremental outcomes."
- **Bin**: **C (Correct with Caveats)** -- Complete removal is a valid upper bound for analysis, but the analytic line should be evaluated against more granular success criteria.

### Assumption 6: Current detection and attribution capabilities are insufficient to identify all Chinese footholds

- **Why assumed true**: Researchers report a lack of confirmed, granular indicators of compromise (IOCs) for Salt Typhoon [E06]. The FBI contacted 600+ impacted companies [E08], suggesting the scope of compromise may still be expanding. Telecom company claims of eviction are "difficult to reconcile" with the joint advisory stating Salt Typhoon maintains "persistent, long-term access" [E08].
- **Does it hold under all conditions?** A breakthrough in threat intelligence sharing or a major defection/leak of Chinese operational details could rapidly improve detection. This is low-probability but not impossible.
- **Evidence supporting**: E06, E08
- **Evidence challenging**: None in evidence registry.
- **Bin**: **S (Supported)** -- The IOC gap is specifically documented and constrains remediation.

### Assumption 7: Telecom companies lack sufficient incentive and capacity to self-remediate without regulatory compulsion

- **Why assumed true**: The Salt Typhoon breach exploited "basic maintenance failures" including weak passwords and seven-year-old unpatched vulnerabilities [E01, E03]. The Senate Commerce Committee concluded telecoms "still have not convincingly shown they have evicted the intruders" [E01]. When binding cybersecurity requirements were rescinded, they were replaced with voluntary collaboration [E02]. University of Florida researchers found that vulnerability maintainers "simply lacked the personnel or expertise to address the flaws" [E07].
- **Does it hold under all conditions?** Major liability lawsuits or customer defections could create market-based incentives. However, telecom is an oligopoly with limited competitive pressure on security.
- **Evidence supporting**: E01, E02, E03, E07
- **Evidence challenging**: E15 (DoD SWFT Initiative and international regulatory trends could eventually create compliance pressure, but these are nascent)
- **Bin**: **S (Supported)** -- The combination of regulatory rollback and demonstrated industry inaction is well-documented.

### Assumption 8: Offensive cyber operations ("hack back") cannot substitute for defensive remediation

- **Why assumed true**: Hack-back operations "do not remove vulnerabilities" and risk escalation [E11]. Anne Neuberger stated that US presidents "lack enough confidence that U.S. defenses could withstand a potentially escalatory tit-for-tat battle in cyberspace" [E11]. Historical precedent from the OPM breach shows that even when the US chose not to retaliate, Chinese operations only "temporarily subsided" before surging again [E10].
- **Does it hold under all conditions?** A sufficiently devastating offensive operation against Chinese cyber contractor infrastructure could theoretically set back capabilities for years. However, this carries significant escalation risk and does not address underlying US vulnerabilities.
- **Evidence supporting**: E10, E11
- **Evidence challenging**: None directly.
- **Bin**: **S (Supported)** -- Both logical reasoning and historical precedent confirm this.

### Assumption 9: The adversary will not voluntarily withdraw or reduce operations due to diplomatic or deterrent pressure

- **Why assumed true**: After the OPM breach, diplomatic pressure produced only a temporary pause before operations surged [E10]. China "perceives U.S. telecommunications networks as a key center of gravity in a possible conflict" [E19], giving it strong strategic motivation to maintain access. The MSS contractor model provides deniability that insulates the political leadership from diplomatic consequences [E18].
- **Does it hold under all conditions?** A fundamental shift in US-China relations (e.g., a comprehensive cyber arms control agreement or a broader detente) could alter the calculus. A credible demonstration of US offensive capability might also create temporary restraint, as seen post-OPM. However, the "temporary" nature of past pauses is the key caveat.
- **Evidence supporting**: E10, E17, E18, E19
- **Evidence challenging**: E10 (the temporary pause itself shows diplomatic pressure has some effect, even if not durable)
- **Bin**: **C (Correct with Caveats)** -- Diplomatic pressure can produce temporary reductions but has not produced lasting withdrawal. A major geopolitical shift could change this, but nothing in the current environment suggests one is imminent.

### Assumption 10: Allied and international coordination will not produce a step-change in defensive effectiveness

- **Why assumed true**: This is an unstated assumption in the analytic line, which focuses exclusively on US domestic factors. The evidence registry notes "minimal evidence on Five Eyes coordination effectiveness" as a gap [Evidence Registry, Quality Assessment].
- **Does it hold under all conditions?** Allied coordination on cybersecurity is increasing (EU Cyber Resilience Act [E15]), but no evidence in the registry demonstrates that international cooperation has materially improved the US defensive posture against Salt Typhoon specifically.
- **Evidence supporting**: No direct evidence -- this is an identified gap.
- **Evidence challenging**: E15 (emerging allied regulatory frameworks suggest growing international coordination, but effectiveness is unproven)
- **Bin**: **U (Unsupported)** -- Insufficient evidence to assess. This is a gap the analytic line should acknowledge.

---

## Linchpin Analysis

### Linchpin 1: US telecom infrastructure is too large and complex for comprehensive remediation within 2-3 years

- **Justification**: The analytic line's core prediction -- that complete removal will not occur -- rests fundamentally on the proposition that the attack surface is too vast to secure in the timeframe. If infrastructure were simpler or smaller, the problem would be tractable even with current institutional limitations.
- **Failure conditions**: (a) A "Manhattan Project"-scale federal investment in telecom infrastructure modernization; (b) emergence of automated vulnerability detection and patching tools that dramatically reduce the human labor required; (c) a decision to segment and abandon the most compromised legacy networks rather than remediate them.
- **Supporting evidence**: 200,000+ exposed devices with only 25% reduction over six months [E05]; 100+ distinct exploitable vulnerabilities in LTE/5G implementations [E07]; decades of consolidation creating architectural complexity [E04]; legacy TDM infrastructure in critical systems [E19]; seven-year-old unpatched vulnerabilities [E03].
- **Challenging evidence**: Emerging regulatory frameworks could accelerate remediation timelines [E15], though none are yet operational at scale.
- **Impact if wrong**: If the infrastructure problem is more tractable than assessed -- e.g., because 80% of the risk is concentrated in a small number of critical chokepoints that can be hardened quickly -- then "substantial removal" (not complete, but strategically meaningful) could be achievable within 2-3 years. The analytic line would need to shift from "incremental progress" to "significant but incomplete hardening."

### Linchpin 2: Institutional and political obstacles will persist

- **Justification**: Without sustained political will and institutional capacity, even technically feasible remediation will not be executed. The analytic line depends on the continued absence of a coordinated, well-resourced national effort.
- **Failure conditions**: (a) A galvanizing crisis event (e.g., Chinese cyber sabotage during a Taiwan scenario) that creates bipartisan political will for emergency cybersecurity legislation; (b) a new administration or Congressional majority that reverses the FCC's regulatory rollback and funds CISA at wartime levels; (c) successful rebuilding of CISA by end of FY2026 as planned [E13].
- **Supporting evidence**: FCC rescinded binding cybersecurity requirements [E02]; CISA lost one-third of its workforce [E12]; OPM breach precedent shows post-crisis reforms fade [E10].
- **Challenging evidence**: CISA hiring spree planned [E13]; DoD SWFT Initiative launched [E15]; experts warn of potential 2027 US-China crisis, creating timeline pressure that could generate political urgency [E13].
- **Impact if wrong**: If a crisis or political shift produces a mandatory, well-funded national cybersecurity effort, the analytic line's prediction of "incremental" progress would significantly understate likely outcomes. The assessment would need to shift toward a more optimistic trajectory, though complete removal would still face the technical constraints of Linchpin 1.

### Linchpin 3: China's offensive cyber capabilities will continue at industrial scale

- **Justification**: The analytic line assumes an adversary that continuously re-compromises networks even as defenders patch and harden. If Chinese capabilities were static or declining, the defense-offense balance could shift in the US's favor.
- **Failure conditions**: (a) Internal Chinese political upheaval that disrupts the MSS contractor model; (b) successful US/allied offensive operations that dismantle key contractor infrastructure (i-SOON, Sichuan Juxinhe, etc.); (c) a severe Chinese economic crisis that reduces funding for cyber operations; (d) a credible cyber arms control agreement with verification mechanisms.
- **Supporting evidence**: Contractor-enabled industrial-scale model with modular tooling and built-in deniability [E17, E18]; continued deployment of new malware (Brickstorm) maintaining 17-month persistence [E16]; operations surged after temporary post-OPM pause [E10]; 600+ organizations breached globally [E17].
- **Challenging evidence**: None in the evidence registry.
- **Impact if wrong**: If Chinese offensive capacity were significantly degraded -- e.g., through internal disruption or successful offensive operations -- the remediation challenge would become primarily a domestic technical and institutional problem. The analytic line would need revision to assess whether US domestic capacity alone is sufficient for meaningful progress, absent continuous adversary re-compromise.

### Linchpin 4: Telecom companies lack sufficient incentive and capacity to self-remediate without regulatory compulsion

- **Justification**: Even if political will existed at the federal level, remediation must be executed by private telecom operators. The analytic line depends on these companies remaining unable or unwilling to undertake comprehensive remediation on their own.
- **Failure conditions**: (a) Major liability lawsuits or class action settlements that create financial incentives for security investment; (b) a competitive dynamic where one carrier's demonstrated security posture attracts enterprise and government customers away from competitors; (c) insurance market pressure that makes premiums prohibitive without specific security benchmarks.
- **Supporting evidence**: Basic maintenance failures (weak passwords, seven-year-old unpatched vulnerabilities) persisted pre-breach [E01, E03]; Senate concluded carriers have not convincingly shown eviction [E01]; vulnerability maintainers lacked personnel or expertise [E07]; binding requirements replaced with voluntary framework [E02].
- **Challenging evidence**: E15 (emerging regulatory trends could eventually create compliance pressure).
- **Impact if wrong**: If telecoms were to undertake large-scale self-remediation -- driven by liability, market pressure, or leadership change -- the pace of hardening could accelerate significantly beyond what the analytic line predicts, even without federal regulatory compulsion. The analytic line would need to reassess the "incremental" qualifier upward.

---

## Fault Lines Summary

| Priority | Assumption | Risk | Recommended Action |
|----------|-----------|------|--------------------|
| 1 | Institutional and political obstacles will persist (#2) | Most volatile assumption -- a single crisis event or political shift could invalidate it rapidly, changing the trajectory from "incremental" to "accelerated" progress | Monitor for: reversal of FCC regulatory rollback; CISA workforce recovery metrics; Congressional cybersecurity legislation; any Chinese cyber operation that causes visible public harm |
| 2 | Telecom companies lack incentive to self-remediate (#7) | Market or legal forces could create incentives independent of government action; liability landscape is evolving | Monitor for: major lawsuits against breached carriers; insurance market shifts; enterprise customer demands for security attestation |
| 3 | The adversary will not voluntarily reduce operations (#9) | Diplomatic channels or deterrence could produce temporary pauses (as post-OPM), and even temporary pauses create windows for remediation | Monitor for: US-China diplomatic engagements on cyber norms; credible US offensive cyber demonstrations; shifts in Chinese strategic priorities |
| 4 | Allied coordination will not produce a step-change (#10) | Unsupported assumption that may understate international contributions; evidence gap means the analytic line may be missing a significant factor | Collect: Five Eyes cyber coordination outcomes; NATO cyber defense posture assessments; allied threat intelligence sharing effectiveness against Typhoon campaigns |
| 5 | Infrastructure is too large and complex (#1) | Most strongly supported assumption, but could be partially mitigated by risk-prioritized approaches that focus on the most strategically significant network segments rather than comprehensive remediation | Monitor for: risk-tiered remediation strategies; automated patching capabilities; decisions to decommission rather than remediate legacy systems |

---

## Sources & Citations

| Ref | Source | Retrieved | Reliability | Method |
|-----|--------|-----------|-------------|--------|
| [E01] | [Is America's Cyber Weakness Self-Inflicted? -- War on the Rocks](https://warontherocks.com/2026/01/is-americas-cyber-weakness-self-inflicted/) | 2026-02-15 | High | OSINT |
| [E02] | [Is America's Cyber Weakness Self-Inflicted? -- War on the Rocks](https://warontherocks.com/2026/01/is-americas-cyber-weakness-self-inflicted/) | 2026-02-15 | High | OSINT |
| [E03] | [Is America's Cyber Weakness Self-Inflicted? -- War on the Rocks](https://warontherocks.com/2026/01/is-americas-cyber-weakness-self-inflicted/) | 2026-02-15 | High | OSINT |
| [E04] | [Why telecoms may never purge Salt Typhoon -- CyberScoop](https://cyberscoop.com/salt-typhoon-chinese-hackers-us-telecom-breach/) | 2026-02-15 | High | OSINT |
| [E05] | [Why telecoms may never purge Salt Typhoon -- CyberScoop](https://cyberscoop.com/salt-typhoon-chinese-hackers-us-telecom-breach/) | 2026-02-15 | High | OSINT |
| [E06] | [Why telecoms may never purge Salt Typhoon -- CyberScoop](https://cyberscoop.com/salt-typhoon-chinese-hackers-us-telecom-breach/) | 2026-02-15 | Medium | OSINT |
| [E07] | [Why telecoms may never purge Salt Typhoon -- CyberScoop](https://cyberscoop.com/salt-typhoon-chinese-hackers-us-telecom-breach/) | 2026-02-15 | High | OSINT |
| [E08] | [Reconfiguring U.S. Cyber Strategy in the Wake of Salt Typhoon -- Lawfare](https://www.lawfaremedia.org/article/reconfiguring-u.s.-cyber-strategy-in-the-wake-of-salt-typhoon) | 2026-02-15 | High | OSINT |
| [E09] | [Reconfiguring U.S. Cyber Strategy in the Wake of Salt Typhoon -- Lawfare](https://www.lawfaremedia.org/article/reconfiguring-u.s.-cyber-strategy-in-the-wake-of-salt-typhoon) | 2026-02-15 | High | OSINT |
| [E10] | [Reconfiguring U.S. Cyber Strategy in the Wake of Salt Typhoon -- Lawfare](https://www.lawfaremedia.org/article/reconfiguring-u.s.-cyber-strategy-in-the-wake-of-salt-typhoon) | 2026-02-15 | High | OSINT |
| [E11] | [Reconfiguring U.S. Cyber Strategy in the Wake of Salt Typhoon -- Lawfare](https://www.lawfaremedia.org/article/reconfiguring-u.s.-cyber-strategy-in-the-wake-of-salt-typhoon) | 2026-02-15 | High | OSINT |
| [E12] | [CISA plans hiring spree to rebuild depleted ranks -- Cybersecurity Dive](https://www.cybersecuritydive.com/news/cisa-hiring-workforce-strategy/805733/) | 2026-02-15 | High | OSINT |
| [E13] | [CISA plans hiring spree to rebuild depleted ranks -- Cybersecurity Dive](https://www.cybersecuritydive.com/news/cisa-hiring-workforce-strategy/805733/) | 2026-02-15 | High | OSINT |
| [E14] | [China's Typhoon hackers have changed the rules -- SC World](https://www.scworld.com/perspective/chinas-typhoon-hackers-have-changed-the-rules-of-cybersecurity) | 2026-02-15 | Medium | OSINT |
| [E15] | [China's Typhoon hackers have changed the rules -- SC World](https://www.scworld.com/perspective/chinas-typhoon-hackers-have-changed-the-rules-of-cybersecurity) | 2026-02-15 | Medium | OSINT |
| [E16] | [Chinese-linked hackers use Brickstorm backdoor -- Reuters](https://www.reuters.com/world/china/chinese-linked-hackers-use-back-door-potential-sabotage-us-canada-say-2025-12-04/) | 2026-02-15 | High | OSINT |
| [E17] | [Inside Salt Typhoon -- DomainTools](https://dti.domaintools.com/research/inside-salt-typhoon-chinas-state-corporate-advanced-persistent-threat) | 2026-02-15 | High | OSINT |
| [E18] | [Inside Salt Typhoon -- DomainTools](https://dti.domaintools.com/research/inside-salt-typhoon-chinas-state-corporate-advanced-persistent-threat) | 2026-02-15 | High | OSINT |
| [E19] | [Advancing IP Interconnection -- FDD](https://www.fdd.org/analysis/2025/12/16/advancing-ip-interconnection/) | 2026-02-15 | High | OSINT |
| [E20] | User-provided, session context | 2026-02-15 | N/A | USER |

**Citation Methods:**
- **OSINT**: Retrieved via Firecrawl MCP or WebSearch/WebFetch fallback
- **FILE**: Read from local file system
- **USER**: Provided by analyst in conversation
- **ANALYSIS**: Analytical judgment produced via named technique protocol

> Every claim must be cited. Uncited claims are not permitted.
