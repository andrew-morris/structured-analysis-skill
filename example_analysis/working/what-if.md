# What If? Analysis: Will the US Successfully Remove Chinese Hackers from Their Networks?

> **Analysis ID**: 2026-02-15-us-chinese-hackers-network-removal | **Date**: 2026-02-15 | **Phase**: Challenge & Foresight
> **Evidence base**: 20 items | [Full registry](../evidence-registry.md)

---

## Conventional Analytic Line

The US will make incremental progress in hardening networks but will not achieve complete removal of Chinese state-sponsored hackers within 2-3 years due to the scale of US telecom infrastructure, institutional obstacles, and the adversary's industrial-scale capabilities. Emerging regulatory efforts (DoD SWFT Initiative, proposed FCC actions) and CISA rebuilding will improve the defensive posture at the margins, but the combination of 200,000+ exposed devices [E05], a gutted federal cybersecurity workforce [E12], voluntary rather than binding compliance frameworks [E02], and an adversary operating at industrial scale through contractor-enabled front companies [E17, E18] means that persistent Chinese access to US networks is more likely than not to continue through at least 2028.

---

## The "What If?" Event

**Assumed event**: It is February 2027. During a Taiwan Strait crisis, China activates pre-positioned access across US telecommunications and critical infrastructure networks, disrupting military communications, civilian emergency services, and financial systems simultaneously. The US discovers that Salt Typhoon and Volt Typhoon access was never effectively removed despite two years of remediation efforts, and new Brickstorm-class implants were deployed during the remediation period itself.

**Why this event**: This event challenges the implicit assumption embedded in US policy that remediation is possible given sufficient time and effort -- that the problem is one of will and resources rather than structural impossibility. It also challenges the assumption that Chinese cyber pre-positioning is primarily an intelligence collection activity rather than a wartime capability held in reserve. Experts have already warned of a potential US-China crisis in 2027 [E13], and China perceives US telecommunications networks as "a key center of gravity in a possible conflict" [E19]. If this event occurred, it would demonstrate that the conventional analytic line -- focusing on incremental hardening -- fundamentally misunderstood both the adversary's intent and the defender's structural limitations.

---

## Triggering Events / Catalysts

| # | Triggering Event | Plausibility | Evidence | Ref |
|---|-----------------|--------------|----------|-----|
| 1 | CISA fails to rebuild capacity in time: the 40% vacancy rate persists through FY2026 as hiring cannot keep pace with attrition and the 2027 crisis timeline | High | CISA lost more than one-third of its workforce; acting director acknowledged the agency must hire by end of FY2026 to strengthen defensive posture, while experts predict a potential crisis in 2027 | [E12, E13] |
| 2 | Voluntary compliance framework proves insufficient: telecoms underinvest in remediation after FCC rescinds binding cybersecurity orders, leaving the same basic maintenance failures (unpatched routers, weak passwords) that Salt Typhoon originally exploited | High | FCC rescinded binding orders in late 2025, replacing them with voluntary collaboration; Salt Typhoon exploited basic failures including 7-year-old unpatched Cisco vulnerabilities, not sophisticated zero-days | [E01, E02, E03] |
| 3 | China deploys new implant families during the remediation window itself, exploiting the disruption and visibility gaps created by remediation activities | High | Brickstorm malware already demonstrated ability to maintain 17-month persistent access (Apr 2024 - Sep 2025) against VMware infrastructure; Salt Typhoon's modular tooling and contractor-enabled model allow rapid deployment of new capabilities | [E16, E18] |
| 4 | Lack of reliable IOCs prevents defenders from confirming eviction, creating false confidence that networks are clean | High | Researchers report insufficient granular indicators of compromise for Salt Typhoon to conduct reliable threat hunting; telecom claims of eviction are contradicted by joint government advisory stating persistent access continues | [E06, E08] |
| 5 | Taiwan Strait crisis escalation triggers Chinese decision to activate pre-positioned access as a deterrent or warfighting measure | Medium | China perceives US telecom networks as a key center of gravity in a possible conflict; Salt Typhoon represents a dual-use capability (espionage + wartime disruption) aligned with MSS | [E17, E19] |

---

## Backward Chain of Argumentation

### Step 5 (The Event): China Activates Pre-Positioned Network Access During Taiwan Crisis

- **What happens**: In February 2027, as US naval forces move toward the Taiwan Strait, Chinese operators activate dormant implants across US telecom and critical infrastructure, degrading military C2 links, disrupting 911 emergency services in major metropolitan areas, and causing cascading failures in financial transaction networks.
- **Why it follows**: China has explicitly positioned its Typhoon-family operations as dual-use capabilities -- espionage in peacetime, disruption in wartime. The MSS-aligned Salt Typhoon contractor model provides the organizational capacity to coordinate simultaneous activation across hundreds of compromised networks.
- **Evidence**: Salt Typhoon = MSS-aligned dual-use capability (espionage + wartime disruption); 600+ organizations breached worldwide, 200+ in US [E17]. China perceives US telecom networks as "a key center of gravity in a possible conflict" [E19].

### Step 4 (Late 2026): US Declares Remediation Progress While Adversary Maintains and Expands Access

- **What happens**: By late 2026, major telecoms publicly report that Salt Typhoon has been evicted. However, without reliable IOCs, these claims cannot be independently verified. Meanwhile, new Brickstorm-class implants deployed during 2025-2026 remain undetected in virtualization infrastructure and edge devices.
- **Why it follows**: Telecoms have institutional incentives to declare success. The absence of granular IOCs means defenders cannot distinguish between "threat eliminated" and "threat no longer visible." Brickstorm has already demonstrated the ability to persist undetected for 17+ months in enterprise environments.
- **Evidence**: AT&T/Verizon eviction claims are "difficult to reconcile" with joint advisory stating persistent access [E08]. Researchers lack sufficient IOCs for reliable threat hunting [E06]. Brickstorm maintained access Apr 2024 - Sep 2025 undetected [E16].

### Step 3 (Mid-2026): Remediation Efforts Stall on Structural Barriers

- **What happens**: Remediation campaigns focus on known compromised systems but cannot address the full attack surface. The "Frankenstein's monster" of legacy telecom infrastructure, combined with 200,000+ exposed edge devices, creates an unbridgeable gap between remediation capacity and the scope of the problem. CISA hiring proceeds slowly; critical expertise gaps remain.
- **Why it follows**: The scale problem is arithmetic: even aggressive remediation cannot patch and monitor 200,000+ exposed devices across dozens of carriers when the workforce is depleted and the regulatory framework is voluntary. Legacy TDM infrastructure in FAA and railroad systems cannot be patched at all without full replacement.
- **Evidence**: 200,000+ exposed network/edge devices with only 25% reduction in six months despite growing awareness [E05]. Decades of consolidation created "Frankenstein's monster of different equipment, technologies and architecture" [E04]. Legacy TDM infrastructure in FAA and railroad systems remains persistently vulnerable [E19]. CISA at 40% vacancy rate [E12].

### Step 2 (Early 2026): Regulatory Rollback Removes the Primary Forcing Function

- **What happens**: With binding FCC cybersecurity orders rescinded, telecoms revert to cost-minimizing behavior. Voluntary frameworks produce inconsistent compliance. Smaller carriers -- which lack dedicated security teams -- do essentially nothing. The basic maintenance failures that enabled Salt Typhoon (weak passwords, unpatched routers) persist across large segments of the network.
- **Why it follows**: The history of telecom cybersecurity is one of underinvestment absent regulatory compulsion. The FCC rollback removed the only mechanism that could have forced industry-wide remediation at the pace and scale required.
- **Evidence**: FCC rescinded binding cybersecurity orders in late 2025 [E02]. Salt Typhoon exploited basic maintenance failures, not zero-days [E01, E03]. When University of Florida researchers reported LTE/5G vulnerabilities, maintainers "simply lacked the personnel or expertise to address the flaws" [E07].

### Step 1 (Present - Feb 2026): Conditions Are Already Set

- **What happens**: As of February 2026, every precondition for the What If event is already in place. Salt Typhoon access has not been confirmed evicted. CISA is depleted. Binding regulations have been removed. The adversary's industrial-scale contractor model continues to operate. New implant families (Brickstorm) are being deployed. And the conventional wisdom focuses on incremental hardening rather than the structural impossibility of full remediation.
- **Why it follows**: This is not a projection -- it is a description of current conditions as documented in the evidence base.
- **Evidence**: Senate Commerce Committee concluded telecoms "still have not convincingly shown they have evicted the intruders" as of Dec 2025 [E01]. FBI contacted 600+ impacted companies [E08]. Salt Typhoon has been active since 2019 with operations in 80+ countries [E17]. Brickstorm actively deploying against VMware infrastructure [E16]. CISA at 40% vacancy [E12]. FCC binding orders rescinded [E02].

---

## Plausibility Assessment

| Criterion | Assessment |
|-----------|-----------|
| **Overall pathway plausibility** | HIGH |
| **Weakest link in the chain** | Step 5: The decision to activate -- China must calculate that the benefits of disruption outweigh the risks of escalation and exposure of capabilities |
| **Historical precedent** | Partial: Russia's pre-positioning in Ukrainian infrastructure before 2022 invasion (disruption of Viasat); China's own pattern of surging operations after temporary restraint post-OPM [E10] |
| **Key dependency** | A Taiwan Strait crisis of sufficient severity to trigger Chinese activation of pre-positioned capabilities |

**Assessment narrative**: The pathway from present conditions to the What If event is disturbingly well-supported by evidence. Steps 1 through 4 are not speculative -- they describe dynamics already in motion and documented by multiple high-reliability sources. The only genuinely uncertain step is the final one: China's decision to activate. This depends on the geopolitical trajectory of the Taiwan Strait situation and Chinese leadership's risk calculus. However, the entire point of pre-positioning is to create the option for activation. The evidence that China views US telecom networks as "a key center of gravity in a possible conflict" [E19] and has built a dual-use capability explicitly designed for both espionage and wartime disruption [E17] strongly suggests that activation is at minimum a planned option. The historical precedent of Russia's Viasat attack in February 2022 -- timed precisely to the opening hours of the Ukraine invasion -- demonstrates that state actors do activate pre-positioned cyber capabilities at the onset of military operations. The OPM precedent [E10] further shows that even when Chinese operations are detected and diplomatically addressed, the adversary reorganizes and returns rather than permanently withdrawing.

---

## Indicators to Monitor

| # | Indicator | Observable? | Current Status | Monitoring Frequency |
|---|-----------|-------------|----------------|---------------------|
| 1 | Rate of exposed network/edge devices with Salt Typhoon-associated vulnerabilities (Censys baseline: 200K+) | Yes | Only 25% reduction in 6 months as of early 2026 [E05] | Monthly |
| 2 | CISA workforce levels and vacancy rates across key mission areas | Yes | ~40% vacancy rate as of early 2026 [E12] | Quarterly |
| 3 | Detection of new implant families (Brickstorm-class or successor) in US telecom or critical infrastructure | Partially | Brickstorm disclosed Dec 2025 [E16]; detection depends on IOC availability | Continuous |
| 4 | Telecom compliance with voluntary cybersecurity framework benchmarks (patch cadence, credential hygiene, segmentation) | Partially | No public reporting mechanism established post-FCC rollback [E02] | Quarterly |
| 5 | Release of granular, actionable Salt Typhoon IOCs by government or security researchers | Yes | Insufficient IOCs available as of current date [E06] | Monthly |
| 6 | Taiwan Strait military activity, diplomatic signals, and Chinese military exercises opposite Taiwan | Yes | Elevated but below crisis threshold | Weekly |
| 7 | Congressional or executive action to restore binding telecom cybersecurity requirements | Yes | FCC binding orders rescinded; no replacement legislation enacted [E02] | Quarterly |
| 8 | Evidence of Salt Typhoon or affiliated groups exploiting remediation-period visibility gaps | Partially | Not yet publicly reported but consistent with adversary tradecraft [E18] | Continuous |

---

## Scope of Consequences

**If the "What If?" event occurred, the consequences would include:**

| Domain | Consequence | Severity | Timeframe |
|--------|------------|----------|-----------|
| Military | Degraded C2 communications during initial phase of Taiwan Strait operations; inability to coordinate allied naval and air assets; loss of situational awareness in Western Pacific | High | Immediate (hours-days) |
| Civilian Infrastructure | Disruption of 911 emergency services across major metropolitan areas; cascading failures in utilities dependent on telecom backhaul | High | Immediate (hours-days) |
| Financial | Transaction processing delays or failures across banking and payment networks; potential for panic-driven bank runs if outages persist | High | Immediate to short-term (days-weeks) |
| Intelligence | Confirmation that Chinese actors maintained real-time access to US communications intelligence, including wiretap systems compromised by Salt Typhoon; reassessment of all intelligence shared over compromised channels since 2019 | High | Medium-term (weeks-months) |
| Political | Severe erosion of public trust in government's ability to defend critical infrastructure; bipartisan political crisis over regulatory rollback decisions; potential for overreaction in cyber policy | High | Medium-term (months) |
| Strategic Deterrence | Demonstration that pre-positioned cyber capabilities can impose costs on the US homeland during a conventional conflict, potentially deterring US intervention in Taiwan; fundamental shift in US-China escalation dynamics | High | Long-term (years) |
| Alliance Relationships | Allied governments reassess reliance on US telecom infrastructure and intelligence-sharing arrangements; Five Eyes partners may restrict information sharing pending US remediation | Medium | Medium-term (months) |
| Regulatory | Emergency imposition of binding cybersecurity mandates across telecom and critical infrastructure sectors; likely overcorrection with significant compliance costs | Medium | Medium-term (months-years) |

---

## Sources & Citations

| Ref | Source | Retrieved | Reliability | Method |
|-----|--------|-----------|-------------|--------|
| [E01] | [Is America's Cyber Weakness Self-Inflicted?](https://warontherocks.com/2026/01/is-americas-cyber-weakness-self-inflicted/) -- War on the Rocks | 2026-02-15 | High | OSINT |
| [E02] | [Is America's Cyber Weakness Self-Inflicted?](https://warontherocks.com/2026/01/is-americas-cyber-weakness-self-inflicted/) -- War on the Rocks | 2026-02-15 | High | OSINT |
| [E03] | [Is America's Cyber Weakness Self-Inflicted?](https://warontherocks.com/2026/01/is-americas-cyber-weakness-self-inflicted/) -- War on the Rocks | 2026-02-15 | High | OSINT |
| [E04] | [Why telecoms may never purge Salt Typhoon](https://cyberscoop.com/salt-typhoon-chinese-hackers-us-telecom-breach/) -- CyberScoop | 2026-02-15 | High | OSINT |
| [E05] | [Why telecoms may never purge Salt Typhoon](https://cyberscoop.com/salt-typhoon-chinese-hackers-us-telecom-breach/) -- CyberScoop/Censys | 2026-02-15 | High | OSINT |
| [E06] | [Why telecoms may never purge Salt Typhoon](https://cyberscoop.com/salt-typhoon-chinese-hackers-us-telecom-breach/) -- CyberScoop | 2026-02-15 | Medium | OSINT |
| [E07] | [Why telecoms may never purge Salt Typhoon](https://cyberscoop.com/salt-typhoon-chinese-hackers-us-telecom-breach/) -- CyberScoop/U. Florida | 2026-02-15 | High | OSINT |
| [E08] | [Reconfiguring U.S. Cyber Strategy in the Wake of Salt Typhoon](https://www.lawfaremedia.org/article/reconfiguring-u.s.-cyber-strategy-in-the-wake-of-salt-typhoon) -- Lawfare | 2026-02-15 | High | OSINT |
| [E09] | [Reconfiguring U.S. Cyber Strategy in the Wake of Salt Typhoon](https://www.lawfaremedia.org/article/reconfiguring-u.s.-cyber-strategy-in-the-wake-of-salt-typhoon) -- Lawfare | 2026-02-15 | High | OSINT |
| [E10] | [Reconfiguring U.S. Cyber Strategy in the Wake of Salt Typhoon](https://www.lawfaremedia.org/article/reconfiguring-u.s.-cyber-strategy-in-the-wake-of-salt-typhoon) -- Lawfare | 2026-02-15 | High | OSINT |
| [E11] | [Reconfiguring U.S. Cyber Strategy in the Wake of Salt Typhoon](https://www.lawfaremedia.org/article/reconfiguring-u.s.-cyber-strategy-in-the-wake-of-salt-typhoon) -- Lawfare | 2026-02-15 | High | OSINT |
| [E12] | [CISA plans hiring spree to rebuild depleted ranks](https://www.cybersecuritydive.com/news/cisa-hiring-workforce-strategy/805733/) -- Cybersecurity Dive | 2026-02-15 | High | OSINT |
| [E13] | [CISA plans hiring spree to rebuild depleted ranks](https://www.cybersecuritydive.com/news/cisa-hiring-workforce-strategy/805733/) -- Cybersecurity Dive | 2026-02-15 | High | OSINT |
| [E14] | [China's Typhoon hackers have changed the rules](https://www.scworld.com/perspective/chinas-typhoon-hackers-have-changed-the-rules-of-cybersecurity) -- SC World | 2026-02-15 | Medium | OSINT |
| [E15] | [China's Typhoon hackers have changed the rules](https://www.scworld.com/perspective/chinas-typhoon-hackers-have-changed-the-rules-of-cybersecurity) -- SC World | 2026-02-15 | Medium | OSINT |
| [E16] | [Chinese-linked hackers use Brickstorm backdoor](https://www.reuters.com/world/china/chinese-linked-hackers-use-back-door-potential-sabotage-us-canada-say-2025-12-04/) -- Reuters | 2026-02-15 | High | OSINT |
| [E17] | [Inside Salt Typhoon](https://dti.domaintools.com/research/inside-salt-typhoon-chinas-state-corporate-advanced-persistent-threat) -- DomainTools | 2026-02-15 | High | OSINT |
| [E18] | [Inside Salt Typhoon](https://dti.domaintools.com/research/inside-salt-typhoon-chinas-state-corporate-advanced-persistent-threat) -- DomainTools | 2026-02-15 | High | OSINT |
| [E19] | [Advancing IP Interconnection](https://www.fdd.org/analysis/2025/12/16/advancing-ip-interconnection/) -- FDD | 2026-02-15 | High | OSINT |
| [E20] | Analyst-provided session context | 2026-02-15 | N/A | USER |

**Citation Methods:**
- **OSINT**: Retrieved via Firecrawl MCP or WebSearch/WebFetch fallback
- **FILE**: Read from local file system
- **USER**: Provided by analyst in conversation
- **ANALYSIS**: Analytical judgment produced via named technique protocol

> Every claim must be cited. Uncited claims are not permitted.

---

*Generated by Structured Analysis Skill -- What If? Analysis Protocol | 2026-02-15*
