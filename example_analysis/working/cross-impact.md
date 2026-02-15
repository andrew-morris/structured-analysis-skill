# Cross-Impact Matrix: Will the US Successfully Remove Chinese Hackers from Their Networks?

> **Analysis ID**: 2026-02-15-us-chinese-hackers-network-removal | **Date**: 2026-02-15 | **Phase**: Diagnostic
> **Evidence base**: 20 items | [Full registry](../evidence-registry.md)

---

## Variable Inventory

| ID | Variable | Description | Source |
|----|----------|-------------|--------|
| V1 | US Political Will & Regulatory Environment | Whether the federal government imposes mandatory cybersecurity standards on critical infrastructure operators or relies on voluntary compliance. The FCC rescinded binding cybersecurity orders in late 2025, replacing them with a voluntary framework [E02]. Emerging counter-signals include DoD's SWFT Initiative (May 2025) and proposed legislation [E15]. The regulatory posture directly determines whether telecoms face enforceable obligations or can self-regulate. | [E02, E15, E19] |
| V2 | CISA Institutional Capacity | The federal government's primary civilian cybersecurity agency lost over one-third of its workforce, with ~40% vacancy rates across key mission areas [E12]. CISA must rebuild by end of FY2026 against an anticipated 2027 crisis timeline [E13]. Capacity determines the government's ability to coordinate threat hunting, issue advisories, and support private-sector remediation. | [E12, E13] |
| V3 | Telecom Industry Cooperation & Investment | Private-sector willingness and ability to invest in remediation, patching, and architectural overhaul. Telecoms allowed 7-year-old Cisco vulnerabilities to persist [E01, E03]. Over 200,000 exposed network/edge devices remain, with only 25% reduction in six months [E05]. Some carriers (AT&T, Verizon) claim eviction, but these claims are contradicted by government advisories [E08]. | [E01, E03, E04, E05, E07, E08] |
| V4 | Chinese Offensive Capability & Adaptation | China's cyber operations follow a contractor-enabled, industrial-scale model using MSS-aligned front companies (i-SOON, Sichuan Juxinhe) with modular tooling and built-in deniability [E17, E18]. New tools like Brickstorm maintain 17-month persistent access [E16]. Historical pattern: after the OPM breach, operations temporarily subsided then surged back by 2017 [E10]. The adversary adapts to defensive measures. | [E10, E16, E17, E18] |
| V5 | Technical Complexity of US Networks | Decades of consolidation created "Frankenstein's monster" architectures [E04]. Legacy TDM infrastructure persists in FAA and railroad systems [E19]. University researchers found 100+ exploitable vulnerabilities in LTE/5G implementations, and maintainers lacked personnel to patch them [E07]. Lack of granular IOCs makes reliable threat hunting difficult [E06]. This variable constrains how quickly any remediation effort can succeed regardless of resources applied. | [E04, E06, E07, E19] |
| V6 | International Coordination | Five Eyes and allied intelligence sharing, joint advisories (e.g., CISA joint advisory on Salt Typhoon) [E08, E14]. EU Cyber Resilience Act and UK Government Cyber Action Plan represent emerging allied regulatory frameworks [E15]. Evidence on coordination effectiveness is limited, representing an identified gap in the evidence base. | [E08, E14, E15] |

---

## Cross-Impact Matrix

Reading guide: Each cell answers "How does the **column** variable affect the **row** variable?" Direction: + means a strengthening/reinforcing effect, - means a weakening/opposing effect. Magnitude: H = High, M = Medium, L = Low.

| Effect on (row) / Caused by (col) | **V1: Political Will** | **V2: CISA Capacity** | **V3: Telecom Cooperation** | **V4: Chinese Capability** | **V5: Network Complexity** | **V6: Intl Coordination** |
|---|---|---|---|---|---|---|
| **V1: Political Will** | -- | +M | -M | +H | -L | +M |
| **V2: CISA Capacity** | +H | -- | +L | +M | -M | +M |
| **V3: Telecom Cooperation** | +H | +M | -- | +M | -H | +L |
| **V4: Chinese Capability** | -L | -M | -M | -- | +H | -M |
| **V5: Network Complexity** | -M | -L | -H | +M | -- | -L |
| **V6: Intl Coordination** | +H | +M | +L | +M | -L | -- |

---

## Cell-by-Cell Rationale

### Row V1: Political Will & Regulatory Environment

- **V2 -> V1 (+M)**: A capable CISA can produce threat assessments and public briefings that galvanize political action. The FBI's disclosure that Salt Typhoon obtained "more than one million call records" [E08] and CISA advisories [E14] have historically been catalysts for legislative attention. A weakened CISA produces less pressure.
- **V3 -> V1 (-M)**: Telecom industry lobbying resists mandatory regulation. The FCC rescinded binding orders, which Sen. Cantwell attributed to industry influence [E02]. Active industry resistance dampens political will for enforceable standards.
- **V4 -> V1 (+H)**: Escalating Chinese operations are the single strongest driver of political will. Salt Typhoon's scale (600+ companies, intelligence on "nearly every American" [E19]) creates political urgency that can override industry lobbying. Historical pattern: OPM breach triggered policy responses [E10].
- **V5 -> V1 (-L)**: Overwhelming technical complexity can induce policy fatalism ("this is too hard to fix"), but effect is weak because policymakers often lack the technical understanding to be deterred by complexity.
- **V6 -> V1 (+M)**: Allied action creates normative pressure and political cover. When the EU passes the Cyber Resilience Act [E15], US policymakers face pressure not to fall behind allies.

### Row V2: CISA Capacity

- **V1 -> V2 (+H)**: Political will directly determines CISA's budget, hiring authority, and mandate. The current ~40% vacancy rate is a direct consequence of political decisions under the Trump administration [E12]. Restoration of political support is the strongest lever for rebuilding capacity.
- **V3 -> V2 (+L)**: Industry cooperation with CISA (sharing telemetry, participating in threat intelligence) marginally enhances CISA's situational awareness, but the agency's core capacity problem is staffing and expertise, not information access.
- **V4 -> V2 (+M)**: Escalating Chinese threats create urgency for CISA rebuilding. The anticipated 2027 crisis timeline is explicitly cited as a driver for CISA's hiring sprint [E13]. However, threat escalation can also overwhelm an already depleted agency.
- **V5 -> V2 (-M)**: Network complexity increases the scope of CISA's mission. The sheer number of exposed devices (200,000+) [E05] and heterogeneous architectures dilute CISA's limited resources across too many fronts.
- **V6 -> V2 (+M)**: International partners (Five Eyes) can partially compensate for CISA gaps through intelligence sharing and joint advisories [E14]. Allied agencies provide threat intelligence that CISA lacks capacity to generate independently.

### Row V3: Telecom Cooperation

- **V1 -> V3 (+H)**: Mandatory regulation is the strongest driver of industry investment. The rescission of FCC binding orders [E02] removed the primary enforcement mechanism. Without regulatory teeth, telecoms under-invest in security relative to the threat — as demonstrated by 7-year unpatched vulnerabilities [E01, E03].
- **V2 -> V3 (+M)**: A capable CISA can provide technical assistance, threat intelligence, and coordination that reduces the cost of compliance for telecoms. When CISA partnerships are "frozen" [E12], industry loses a key support mechanism.
- **V4 -> V3 (+M)**: Visible Chinese intrusions create reputational risk and customer concern that motivate industry investment. However, the effect is attenuated: AT&T and Verizon have chosen to claim eviction rather than transparently invest [E08], suggesting reputational management may substitute for genuine remediation.
- **V5 -> V3 (-H)**: This is among the strongest interactions in the matrix. Network complexity directly constrains industry remediation capacity. The "Frankenstein's monster" architecture [E04], maintainers who lack expertise to patch [E07], and the 200,000+ exposed devices [E05] mean that even willing telecoms face enormous technical barriers to compliance.
- **V6 -> V3 (+L)**: International regulatory standards (EU Cyber Resilience Act) [E15] create compliance expectations for global carriers, but the effect on domestic US telecom cooperation is indirect and weak.

### Row V4: Chinese Capability

- **V1 -> V4 (-L)**: US regulation primarily affects defensive posture, not Chinese offensive capability directly. Mandatory standards could close exploitable doors, marginally degrading Chinese access, but the contractor ecosystem adapts [E18]. Effect is low because regulation addresses known vulnerabilities while China develops new tools [E16].
- **V2 -> V4 (-M)**: A capable CISA can degrade Chinese operations through timely advisories, coordinated patching campaigns, and threat hunting. CISA's joint advisory on Salt Typhoon's techniques [E14] helps defenders close specific attack vectors. But China's modular tooling allows rapid retooling [E18].
- **V3 -> V4 (-M)**: Industry patching and network hardening directly closes the "unlocked doors" that China walked through [E01]. The 25% reduction in exposed devices [E05], while insufficient, demonstrates that industry action can degrade Chinese access to some degree.
- **V5 -> V4 (+H)**: Network complexity is the strongest enabler of Chinese capability. Every unpatched device, legacy system, and architectural seam provides attack surface. The 100+ exploitable vulnerabilities in LTE/5G [E07], persistent legacy TDM systems [E19], and lack of IOCs for threat hunting [E06] all directly enhance Chinese operational effectiveness.
- **V6 -> V4 (-M)**: Allied intelligence sharing can expose Chinese infrastructure, burn contractor front companies, and coordinate defensive responses across multiple countries. Salt Typhoon operates across 80+ countries [E17], making it vulnerable to multi-national disruption.

### Row V5: Network Complexity

- **V1 -> V5 (-M)**: Regulation can mandate architectural modernization (e.g., IP transition of legacy TDM [E19]), forcing reduction of complexity over time. But regulatory timelines are slow, and the effect is medium because mandates to simplify do not make simplification technically easier.
- **V2 -> V5 (-L)**: CISA can provide architectural guidance and best practices but has no direct authority to reduce private-sector network complexity. Effect is marginal.
- **V3 -> V5 (-H)**: Industry investment is the most direct lever for reducing complexity. Patching, equipment replacement, architectural consolidation, and network segmentation all reduce attack surface. However, the evidence shows that even six months after public awareness, reduction in exposed devices was only 25% [E05], suggesting the effect is slow.
- **V4 -> V5 (+M)**: Chinese operations can increase effective complexity by compromising network elements and using stolen network diagrams to penetrate additional systems [E09]. Each successful intrusion creates new unknowns (compromised credentials, backdoors) that add to the remediation challenge.
- **V6 -> V5 (-L)**: International coordination has minimal direct effect on US network complexity, though allied technical standards could influence long-term architectural decisions.

### Row V6: International Coordination

- **V1 -> V6 (+H)**: US political commitment to cybersecurity is the primary driver of alliance coordination. US leadership sets the agenda for Five Eyes and broader allied responses. A US government that deprioritizes cybersecurity regulation [E02] undermines allied coordination.
- **V2 -> V6 (+M)**: CISA is the primary US node for international cyber coordination. A depleted CISA [E12] weakens the US contribution to joint advisories and intelligence sharing [E14].
- **V3 -> V6 (+L)**: Industry cooperation with government creates intelligence products that can be shared with allies, but the effect is indirect.
- **V4 -> V6 (+M)**: Escalating Chinese operations across 80+ countries [E17] create shared threat perception that strengthens allied motivation to coordinate. Salt Typhoon's global scope makes it a common concern.
- **V5 -> V6 (-L)**: US network complexity has minimal direct effect on international coordination, though it may reduce confidence in US defensive capabilities among allies.

---

## Reinforcing Loops (Escalation Risks)

### R1: The Regulatory Hollowing Loop (V1 -> V3 -> V5 -> V4 -> V1)
**Cycle**: Weak political will [E02] -> reduced industry compliance pressure -> persistent network complexity [E05] -> enhanced Chinese exploitation success [E01, E03] -> which should increase political will but may instead produce fatalism or blame-shifting -> further regulatory inaction.

**Evidence**: The FCC rescinded binding orders [E02] precisely after Salt Typhoon was disclosed, the opposite of the expected response. This suggests the loop can become pathological: escalating threat produces deregulation rather than regulation, accelerating the cycle.

**Escalation risk**: HIGH. This is the most dangerous dynamic in the system. If political will remains captured by industry lobbying despite escalating Chinese operations, the defensive gap widens with each cycle.

### R2: The Capacity Erosion Loop (V1 -> V2 -> V3 -> V1)
**Cycle**: Political decisions gut CISA [E12] -> reduced government support for industry remediation -> industry faces higher costs to act alone -> industry lobbies against unfunded mandates -> political will further erodes.

**Evidence**: CISA's ~40% vacancy rate [E12] and frozen partnerships directly reduce the government's ability to support industry, which increases the perceived cost of mandatory regulation, which reduces political appetite for mandates.

**Escalation risk**: MEDIUM-HIGH. The FY2026 hiring sprint [E13] is an attempted circuit-breaker, but success depends on sustained political support that is not yet assured.

### R3: The Adversary Adaptation Loop (V4 -> V5 -> V4)
**Cycle**: Chinese operations compromise systems and steal network diagrams [E09] -> stolen information increases effective network complexity and attack surface -> expanded surface enables further Chinese operations -> cycle continues.

**Evidence**: Salt Typhoon stole network traffic diagrams from the Army National Guard and used them to compromise another government agency [E09]. Each successful intrusion generates intelligence that enables the next intrusion. Brickstorm malware demonstrates continued tooling evolution [E16].

**Escalation risk**: HIGH. This loop is self-accelerating and largely independent of US defensive actions. Even partial remediation may be undermined by intelligence already exfiltrated.

---

## Balancing Loops (Stabilizing)

### B1: The Threat-Response Loop (V4 -> V1 -> V3 -> V4)
**Cycle**: Dramatic Chinese breaches [E08, E19] -> increased political urgency -> new mandatory standards -> industry hardening -> reduced Chinese access.

**Assessment**: This is the "expected" democratic response cycle. Evidence suggests it is currently broken at the V4->V1 link: despite unprecedented breach scale, the FCC deregulated rather than tightened standards [E02]. For this loop to activate, the political response must overcome industry capture.

**Stabilization potential**: HIGH if activated, but currently DORMANT. The DoD SWFT Initiative [E15] and CISA rebuilding effort [E13] represent partial activation.

### B2: The International Pressure Loop (V4 -> V6 -> V4)
**Cycle**: Chinese operations across 80+ countries [E17] -> allied coordination strengthens -> joint advisories and intelligence sharing degrade Chinese operational security -> reduced Chinese effectiveness.

**Assessment**: Active but limited. Joint advisories have been issued [E14], and allied regulatory frameworks are emerging [E15]. However, evidence on the actual effectiveness of allied coordination in degrading Chinese operations is thin (identified gap in evidence base).

**Stabilization potential**: MEDIUM. Effectiveness depends on V1 (US political will to lead) and V2 (CISA capacity to coordinate).

### B3: The Reputational Pressure Loop (V4 -> V3 -> V4)
**Cycle**: Public disclosure of breaches [E08] -> reputational/customer pressure on telecoms -> increased security investment -> reduced Chinese access.

**Assessment**: Weakly active. AT&T and Verizon responded to disclosure with claims of eviction rather than transparent remediation investment [E08], suggesting reputational management substitutes for genuine security improvement. Only 25% reduction in exposed devices six months post-disclosure [E05].

**Stabilization potential**: LOW under current conditions. Would strengthen if mandatory disclosure and verification requirements were imposed (requires V1 activation).

---

## Second-Order Effects

### SO1: CISA Depletion Amplifies Network Complexity Impact
**Pathway**: V1(-) -> V2(-) -> V5(persists) -> V4(+)

Political decisions to gut CISA [E12] remove the primary coordinating body that helps telecoms manage complexity [E14]. Without CISA guidance and threat intelligence, telecoms face network complexity alone. This indirectly strengthens Chinese capability by removing the bridge between government intelligence and private-sector action.

**Evidence chain**: Political decisions depleted CISA by >33% [E12] -> partnerships frozen [E12] -> telecoms lack coordinated threat hunting support -> 200,000+ devices remain exposed [E05] -> Salt Typhoon maintains persistent access [E08].

### SO2: Deregulation Creates a Moral Hazard Feedback
**Pathway**: V1(-) -> V3(-) -> V5(persists) -> V6(-)

When the FCC rescinds binding orders [E02], telecoms rationally reduce security investment [E03, E05]. Persistent vulnerability undermines US credibility in demanding allied action, weakening international coordination [E15]. Allies may question whether US critical infrastructure operators are serious about defense, reducing willingness to share sensitive intelligence.

**Evidence chain**: FCC deregulation [E02] -> telecoms maintain 7-year unpatched vulnerabilities [E01, E03] -> 200,000+ exposed devices persist [E05] -> US credibility as cybersecurity leader weakened -> allied coordination effectiveness reduced (inferred from evidence gap).

### SO3: Intelligence Bleed-Through Across Domains
**Pathway**: V4(+) -> V5(+) -> V4(+) -> V2(-)

Chinese theft of network diagrams enables cross-domain compromise [E09]. Salt Typhoon's penetration of military networks (Army National Guard) [E09] means that civilian telecom remediation alone is insufficient -- compromised military intelligence can be used to re-enter civilian networks. This overwhelms CISA's scope, which is focused on civilian infrastructure.

**Evidence**: Stolen Army National Guard network diagrams were used to compromise another government agency [E09]. Salt Typhoon's dual-use model (espionage + wartime disruption) [E17] means military and civilian domains are operationally linked from the adversary's perspective.

### SO4: Contractor Ecosystem Provides Resilience Against Targeted Countermeasures
**Pathway**: V6(+) -> V4(adapts) -> V5(+)

Even if international coordination successfully identifies and sanctions specific Chinese front companies (i-SOON, Sichuan Juxinhe) [E17], the contractor-enabled model with built-in deniability [E18] allows rapid reconstitution. Burning one contractor's infrastructure degrades capability temporarily, but the modular architecture enables replacement. Historical precedent: post-OPM operations "temporarily subsided" then surged by 2017 [E10].

**Evidence**: Modular, contractor-enabled operations with deniability at every layer [E18]. Pattern of temporary suppression followed by resurgence [E10]. New tools (Brickstorm) continue to emerge [E16].

### SO5: Timeline Mismatch Creates a Window of Maximum Vulnerability
**Pathway**: V2(rebuilding) + V5(persistent) + V4(accelerating) -> crisis window

CISA must rebuild by FY2026 against an anticipated 2027 crisis [E13]. Network complexity reduction is measured in years, not months (25% reduction in 6 months) [E05]. Chinese capability continues to advance (Brickstorm, new contractor operations) [E16, E18]. The convergence creates a 12-18 month window (mid-2026 to 2027) where US defensive capacity is at its lowest relative to the threat.

---

## Key Interaction Chains

### Chain 1: The Critical Path to Remediation (Constructive)
```
V1 (mandatory regulation) -> V3 (forced industry investment) -> V5 (complexity reduction) -> V4 (degraded Chinese access)
         |                                                                                            |
         +---> V2 (CISA funding/hiring) ----> V6 (restored allied coordination) ---------+           |
                                                                                          |           |
                                                                                          +---> V4 (further degraded)
```
**Assessment**: This is the only pathway to significant remediation success. It requires V1 activation as the entry point. All other chains depend on political will translating into enforceable mandates and institutional capacity. Currently, V1 is the weakest link: the FCC deregulated after Salt Typhoon [E02], and CISA was gutted [E12].

### Chain 2: The Adversary Escalation Spiral (Destructive)
```
V1(-) deregulation [E02] -> V3(-) reduced compliance -> V5(+) complexity persists [E05]
                                                              |
                                                              v
                                              V4(+) Chinese access expands [E08, E16]
                                                              |
                                                              v
                                              V9(exfiltrated intelligence) [E09]
                                                              |
                                                              v
                                              V5(+) further complexity (backdoors, compromised credentials)
                                                              |
                                                              v
                                              V4(+) self-reinforcing advantage
```
**Assessment**: This chain is currently active. Evidence shows it operating: deregulation [E02] -> persistent exposure [E05] -> ongoing access [E08] -> intelligence theft enabling further penetration [E09]. The loop has no internal brake; it requires external intervention (V1 activation) to interrupt.

### Chain 3: The Alliance Fragmentation Risk
```
V1(-) US deregulation -> V6(-) credibility loss with allies -> V2(-) reduced intel sharing
                                                                          |
                                                                          v
                                                              V4(+) Chinese operations face less coordinated resistance
                                                                          |
                                                                          v
                                                              V4 operates freely across 80+ countries [E17]
```
**Assessment**: Partially active. The evidence gap on allied coordination effectiveness makes this chain difficult to validate, but the logic is sound: a US government that deregulates its own telecoms after a massive Chinese breach sends a signal to allies about the seriousness of the threat response.

---

## Net Impact Assessment

**Highest-influence variable (driver)**: V1 (Political Will) -- appears as a strong causal factor in 4 of 5 other variables. It is the primary lever for activating balancing loops and interrupting reinforcing loops.

**Highest-vulnerability variable (receiver)**: V4 (Chinese Capability) -- affected by all other variables, but its internal resilience (contractor model, modular tooling, historical adaptation pattern) means that even strong defensive action produces only temporary degradation.

**System state assessment**: The system is currently dominated by reinforcing loops (R1, R2, R3) that amplify the threat, while balancing loops (B1, B2, B3) are dormant or weakly active. The critical variable (V1) is in its weakest state, meaning the system is trending toward adversary advantage. The 2026-2027 timeline pressure [E13] creates urgency that the current political trajectory does not match.

---

## Sources & Citations

| Ref | Source | Retrieved | Reliability | Method |
|-----|--------|-----------|-------------|--------|
| [E01] | [Is America's Cyber Weakness Self-Inflicted? -- War on the Rocks](https://warontherocks.com/2026/01/is-americas-cyber-weakness-self-inflicted/) | 2026-02-15 | High | OSINT |
| [E02] | [Is America's Cyber Weakness Self-Inflicted? -- War on the Rocks](https://warontherocks.com/2026/01/is-americas-cyber-weakness-self-inflicted/) | 2026-02-15 | High | OSINT |
| [E03] | [Is America's Cyber Weakness Self-Inflicted? -- War on the Rocks](https://warontherocks.com/2026/01/is-americas-cyber-weakness-self-inflicted/) | 2026-02-15 | High | OSINT |
| [E04] | [Why telecoms may never purge Salt Typhoon -- CyberScoop](https://cyberscoop.com/salt-typhoon-chinese-hackers-us-telecom-breach/) | 2026-02-15 | High | OSINT |
| [E05] | [Why telecoms may never purge Salt Typhoon -- CyberScoop/Censys](https://cyberscoop.com/salt-typhoon-chinese-hackers-us-telecom-breach/) | 2026-02-15 | High | OSINT |
| [E06] | [Why telecoms may never purge Salt Typhoon -- CyberScoop](https://cyberscoop.com/salt-typhoon-chinese-hackers-us-telecom-breach/) | 2026-02-15 | Medium | OSINT |
| [E07] | [Why telecoms may never purge Salt Typhoon -- CyberScoop/U. Florida](https://cyberscoop.com/salt-typhoon-chinese-hackers-us-telecom-breach/) | 2026-02-15 | High | OSINT |
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
| [E20] | Analyst (user-provided, session context) | 2026-02-15 | N/A | USER |

**Citation Methods:**
- **OSINT**: Retrieved via Firecrawl MCP or WebSearch/WebFetch fallback
- **FILE**: Read from local file system
- **USER**: Provided by analyst in conversation
- **ANALYSIS**: Analytical judgment produced via named technique protocol

> Every claim must be cited. Uncited claims are not permitted.

---

*Generated by Structured Analysis Skill -- Cross-Impact Matrix Protocol | 2026-02-15*
