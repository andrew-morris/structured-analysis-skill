# Contrasting Narratives: Will the US Successfully Remove Chinese Hackers from Their Networks?

> **Analysis ID**: 2026-02-15-us-chinese-hackers-network-removal | **Date**: 2026-02-15 | **Phase**: Challenge & Foresight
> **Evidence base**: 20 items | [Full registry](../evidence-registry.md)

---

## Primary Narrative: "We Are Making Progress"

| Element | Description |
|---------|------------|
| **Protagonist** | US Government (CISA, FCC, DoD), telecom carriers (AT&T, Verizon, Lumen), Congress |
| **Antagonist** | Chinese state-sponsored hackers (Salt Typhoon, Volt Typhoon), legacy network vulnerabilities |
| **Crisis** | Chinese APT groups penetrated US telecom infrastructure at scale, compromising call records and surveillance systems; the nation's critical communications backbone is at risk |
| **Resolution** | Carriers are evicting intruders, CISA is rebuilding capacity, new regulatory frameworks (DoD SWFT, EU Cyber Resilience Act) are emerging, and offensive deterrence will raise the cost of Chinese operations [E02, E08, E12, E13, E15] |
| **Evidence basis** | AT&T and Verizon have publicly claimed eviction of Salt Typhoon [E08]; CISA is conducting a hiring surge to restore workforce [E12, E13]; emerging regulatory responses include DoD SWFT and allied government cyber action plans [E15]; FCC initiated (then rescinded) binding cybersecurity orders [E02] |

**Narrative summary**: The US government and industry narrative frames the Salt Typhoon crisis as a serious but manageable challenge. The story arc follows a familiar pattern: breach is discovered, government mobilizes, industry cooperates, new regulations are enacted, and deterrence is restored. This narrative emphasizes forward momentum -- carriers claiming eviction, CISA hiring, new frameworks emerging -- and positions the US as actively closing the gap. The implicit promise is that with sufficient investment, coordination, and political will, the intruders can be removed and future intrusions prevented.

---

## Competing Narrative 1: "Removal Is Essentially Impossible"

| Element | Description |
|---------|------------|
| **Protagonist** | Security researchers, independent cybersecurity experts, academic analysts |
| **Antagonist** | Network complexity itself, decades of underinvestment, institutional inertia, political dysfunction |
| **Crisis** | The problem is not a single breach but a systemic failure: US telecom networks are "Frankenstein's monster" architectures with 200,000+ exposed devices, and the adversary exploited basic maintenance failures -- not exotic zero-days [E01, E03, E04, E05] |
| **Resolution** | No clean resolution is available; the US must accept persistent compromise and redesign networks from the ground up, which will take years or decades. The analogy is failing a safety inspection, not losing a chess match [E04, E05, E07] |
| **Evidence basis** | Experts assess telecoms "may never fully expunge" Salt Typhoon due to network size, identity management difficulty, and decades of consolidation [E04]; Censys found 200,000+ exposed devices with only 25% reduction in six months [E05]; Salt Typhoon exploited 7-year-old unpatched Cisco vulnerabilities, not zero-days [E01, E03]; university researchers found 100+ exploitable vulnerabilities in LTE/5G implementations where maintainers lacked personnel to fix them [E07]; lack of confirmed IOCs makes reliable threat hunting impossible [E06] |

**Narrative summary**: This narrative reframes the problem from a discrete incident requiring a response to a structural condition requiring transformation. The crisis is not that China hacked US networks; it is that US networks were fundamentally unhardened against any competent adversary. The "safety inspection" metaphor shifts blame from the attacker to the defender and implies that eviction claims are performative rather than substantive. The resolution this narrative offers is uncomfortable: there is no quick fix, and removal may be technically impossible given current architectures.

**Source/origin of this narrative**: Cybersecurity researchers (CyberScoop, Censys), academic vulnerability researchers (University of Florida), independent analysts (War on the Rocks, Lawfare)

---

## Competing Narrative 2: "Western Accusations Are Politically Motivated"

| Element | Description |
|---------|------------|
| **Protagonist** | China (Chinese government, Chinese technology sector) |
| **Antagonist** | Western governments, US intelligence community, politically motivated accusers |
| **Crisis** | China faces unfair, irresponsible accusations of cyberattacks that are designed to contain China's rise and justify Western cyber militarization |
| **Resolution** | China rejects the assertions; the international community should see through Western political manipulation and pursue dialogue rather than confrontation |
| **Evidence basis** | Chinese embassy denial: "we reject the relevant parties' irresponsible assertion" [E19 deception indicators]; no evidence in the registry directly supports the Chinese counter-narrative; the evidence registry's own quality assessment notes the absence of Chinese strategic calculus analysis |

**Narrative summary**: The Chinese government narrative is a categorical denial paired with a counter-accusation of political motivation. It does not engage with the technical evidence and instead reframes the entire discussion as geopolitical theater. This narrative serves domestic legitimacy purposes and provides diplomatic cover but makes no attempt to address the specific technical findings documented in multiple independent Western sources. The evidence registry flags this denial as a "standard diplomatic response" carrying "no analytical weight."

**Source/origin of this narrative**: Chinese government official statements via embassy channels

---

## Competing Narrative 3: "Go on Offense -- Hack Back and Impose Massive Costs"

| Element | Description |
|---------|------------|
| **Protagonist** | US offensive cyber forces (Cyber Command, NSA TAO), strategic hawks in Congress and think tanks |
| **Antagonist** | Chinese state cyber apparatus (MSS, contractor networks like i-SOON, Sichuan Juxinhe) [E17, E18] |
| **Crisis** | China is preparing for war; Salt Typhoon is pre-positioning for conflict, and passive defense is inadequate against an adversary operating at industrial scale across 80+ countries [E17, E18, E19] |
| **Resolution** | The US must shift to offensive operations: hack back, impose massive economic and operational costs, disrupt Chinese contractor networks, and treat cyber operations as a warfighting domain -- defense alone will never work |
| **Evidence basis** | Salt Typhoon operates as an MSS-aligned dual-use capability (espionage + wartime disruption) with contractor-enabled, industrial-scale operations [E17, E18]; China perceives US telecom networks as "a key center of gravity in a possible conflict" [E19]; experts predict a potential US-China crisis in 2027, creating timeline pressure [E13]; the OPM breach precedent shows that diplomatic restraint led only to temporary subsidence followed by surge [E10] |

**Narrative summary**: The strategic hawks narrative reframes the cyber intrusion as an act of war preparation rather than espionage. It draws on evidence that Salt Typhoon's dual-use architecture (espionage + disruption) and its targeting of critical infrastructure suggest wartime pre-positioning. The narrative demands escalation: if defense cannot work given the scale of Chinese operations, then offense must impose costs that change the adversary's calculus. The OPM precedent is invoked to argue that restraint has been tried and failed.

**Source/origin of this narrative**: Defense hawks, think tanks (FDD), some members of Congress, former national security officials

---

## Evidence Stress-Test

| # | Narrative Element | Supporting Evidence | Contradicting Evidence | Resilience Rating |
|---|------------------|--------------------|-----------------------|-------------------|
| 1 | **Primary: Carriers are evicting intruders** | AT&T/Verizon public claims of eviction [E08] | Joint advisory says Salt Typhoon maintains "persistent, long-term access" [E08]; experts say telecoms "may never fully expunge" [E04]; Brickstorm malware maintained 17-month access through Sep 2025 [E16] | LOW |
| 2 | **Primary: CISA is rebuilding and strengthening defense** | CISA hiring spree planned for FY2026 [E12, E13] | CISA lost one-third of workforce with ~40% vacancy rate [E12]; rebuilding to prior capacity, not net improvement; 2027 crisis timeline may not allow sufficient recovery [E13] | LOW-MED |
| 3 | **Primary: New regulations will close gaps** | DoD SWFT Initiative, EU Cyber Resilience Act, UK Cyber Action Plan [E15] | FCC rescinded binding cybersecurity orders in late 2025 [E02]; voluntary frameworks replace mandatory compliance [E02]; Sen. Cantwell criticized stripping regulators of enforcement authority [E02] | LOW |
| 4 | **Primary: Offensive deterrence will raise costs** | OPM precedent shows diplomatic restraint failed -- operations surged by 2017 [E10] | Hack-back does not remove vulnerabilities [E11]; US presidents lack confidence in ability to withstand escalatory tit-for-tat [E11]; China's contractor model builds deniability into every layer [E18] | LOW-MED |
| 5 | **Critics: Removal is technically impossible** | Network complexity / "Frankenstein's monster" architecture [E04]; 200,000+ exposed devices with only 25% reduction [E05]; no reliable IOCs for hunting [E06]; 100+ exploitable LTE/5G vulns, maintainers lack expertise [E07] | No direct contradicting evidence in registry; however, evidence gap exists on specific hardened segments and remediation progress | HIGH |
| 6 | **Critics: Attack exploited basic failures, not sophistication** | 7-year-old unpatched Cisco vulnerabilities, weak passwords [E01, E03]; lateral movement through "avoidable weaknesses" [E14] | Salt Typhoon also deploys sophisticated modular tooling and new malware (Brickstorm) [E16, E18], suggesting capabilities beyond exploiting basic failures | HIGH |
| 7 | **China: Accusations are politically motivated** | No supporting evidence in registry | Massive technical evidence base across multiple independent sources [E01-E09, E14, E16-E18]; 600+ breached organizations [E17]; specific IOCs and TTPs documented | LOW |
| 8 | **Hawks: This is war preparation, not espionage** | Dual-use capability (espionage + wartime disruption) [E17]; China views telecom as "center of gravity in conflict" [E19]; 2027 crisis timeline [E13] | Evidence supports espionage interpretation equally well [E08, E09]; dual-use framing is analyst interpretation, not confirmed Chinese doctrine in evidence base | MED |
| 9 | **Hawks: Offense will change adversary calculus** | OPM restraint precedent -- operations surged after diplomatic response [E10] | US lacks confidence in escalation tolerance [E11]; hack-back does not fix vulnerabilities [E11]; China's industrial-scale contractor model can absorb disruption [E17, E18] | LOW-MED |
| 10 | **Hawks: Defense alone will never work** | 200,000+ exposed devices [E05]; structural network complexity [E04]; ongoing new intrusions like Brickstorm [E16] | Emerging regulatory frameworks represent untested but potentially effective structural reform [E15]; absence of evidence on defense effectiveness is not evidence of absence | MED |

**Resilience Key:** HIGH = strong factual support across multiple independent sources; MED = partial factual support with significant gaps or caveats; LOW = mostly rhetorical/aspirational with evidence contradicting key claims

---

## Vulnerability Assessment

| # | Vulnerable Element | Emotional Resonance | Factual Support | Risk |
|---|-------------------|--------------------|-----------------|----- |
| 1 | **Primary narrative: "Carriers have evicted the intruders"** | HIGH -- reassures public and policymakers that the crisis is being managed | LOW -- directly contradicted by joint advisory on persistent access [E08], expert assessments [E04], and ongoing new intrusions [E16] | Public complacency: if eviction is believed, urgency for structural reform collapses. Policymakers may reduce funding and oversight. Risk of "mission accomplished" moment that delays necessary network redesign. |
| 2 | **Hawks: "Hack back will solve the problem"** | HIGH -- appeals to desire for action, strength, and retribution; emotionally satisfying alternative to perceived passivity | LOW -- hack-back does not remove vulnerabilities [E11]; US presidents lack confidence in escalation management [E11]; adversary's contractor model absorbs disruption [E18] | Escalation risk: offensive operations could provoke retaliatory attacks on already-vulnerable infrastructure. Policy resources diverted from defense to offense. Creates false sense of security without addressing root causes. |
| 3 | **China: "We do not conduct cyberattacks"** | HIGH within Chinese domestic audience and among nations skeptical of US geopolitical motives | LOW -- zero evidence supports denial; overwhelming technical evidence contradicts it [E01-E09, E14, E16-E18] | International coalition fragmentation: if denial narrative gains traction among non-aligned nations, multilateral pressure on China weakens. May delay international norms development. |
| 4 | **Primary narrative: "New regulations will close the gap"** | MED -- satisfies demand for government action and systemic response | LOW -- FCC rescinded binding orders [E02]; voluntary frameworks historically ineffective; regulatory timeline unlikely to match threat timeline [E13] | Regulatory theater: appearance of action substitutes for actual enforcement. Carriers continue operating under voluntary frameworks that have already failed. Gap between regulatory promise and implementation widens. |
| 5 | **Hawks: "China is preparing for war -- 2027 crisis"** | HIGH -- creates urgency, justifies resource mobilization, frames inaction as existential risk | MED -- dual-use capability is documented [E17], but war-preparation framing is analyst interpretation, not confirmed intent; 2027 timeline is speculative [E13] | Threat inflation: overstating certainty of conflict could distort resource allocation, justify preemptive actions, or create self-fulfilling prophecy dynamics. May also crowd out investment in long-term defensive infrastructure. |

**Key vulnerabilities**: The most dangerous narrative vulnerabilities are (1) the "carriers have evicted" claim, which has the highest gap between emotional reassurance and factual support, risking public complacency at a critical moment; (2) the "hack back" narrative, which channels legitimate frustration into a strategy that experts assess could provoke escalation without fixing underlying vulnerabilities; and (3) the Chinese denial narrative, which, while analytically worthless, may gain traction internationally and weaken coalition responses.

---

## Pre-Bunking Strategies

| # | Targeted Narrative Element | Pre-Bunking Strategy | Mechanism | Priority |
|---|--------------------------|---------------------|-----------|----------|
| 1 | "Carriers have evicted Salt Typhoon" | **Inoculate with complexity framing** -- proactively explain that "eviction" in networks of this scale is not a binary event; publicize the 200,000+ exposed device figure and the "Frankenstein architecture" reality before carrier PR campaigns gain traction | Audiences who understand network complexity will be resistant to simplistic eviction claims. Anchor on the Censys 25%-in-six-months metric [E05] as an objective progress indicator rather than carrier self-reporting [E08]. | HIGH |
| 2 | "Hack back will impose costs and deter China" | **Pre-empt with escalation literacy** -- publicly disseminate the Neuberger assessment that US presidents lack confidence in withstanding tit-for-tat [E11] and the OPM precedent showing that punitive measures produced only temporary subsidence [E10] | Audiences exposed to escalation risks before the "hack back" narrative gains momentum will be less susceptible to it. Frame offensive operations as a complement to defense, not a substitute, and highlight that hack-back does not patch vulnerabilities [E11]. | HIGH |
| 3 | "Chinese denial of involvement" | **Pre-empt with technical specificity** -- release granular IOCs, contractor attribution details (i-SOON, Sichuan Juxinhe) [E17], and specific TTP documentation before Chinese diplomatic counter-messaging reaches international audiences | Technical specificity makes categorical denial less credible. The contractor-enabled model with named entities [E17, E18] is harder to dismiss than generic "state-sponsored hacker" attribution. Target non-aligned nation audiences specifically. | MED |
| 4 | "New regulations will fix the problem" | **Anchor on the FCC reversal** -- proactively highlight that the FCC rescinded binding orders [E02] and that voluntary frameworks are the current baseline, so audiences evaluate new proposals against actual regulatory trajectory, not aspirational language | By establishing the FCC reversal as the factual baseline, audiences will apply appropriate skepticism to future regulatory announcements. Demand specific enforcement mechanisms and timelines rather than framework announcements [E02, E15]. | MED |
| 5 | "2027 war preparation" framing | **Calibrate urgency without inflation** -- acknowledge the dual-use evidence [E17] and legitimate urgency while clearly distinguishing confirmed capability from inferred intent; present the 2027 timeline as a planning scenario, not a prediction | Prevents threat inflation from distorting resource allocation while preserving appropriate urgency. Audiences inoculated against certainty-of-war framing will still support defensive investment but resist panic-driven policy choices [E13, E17, E19]. | MED |

---

## Narrative Resilience Summary

| Narrative | Overall Resilience | Key Strength | Key Weakness |
|-----------|-------------------|-------------|--------------|
| **Primary (Progress)** | **LOW** | Institutional momentum is real; CISA hiring and regulatory frameworks exist [E12, E13, E15] | Core claims (eviction, regulatory closure) are contradicted by strongest evidence [E04, E05, E08]; FCC rescission undercuts regulatory narrative [E02] |
| **Critics (Impossible)** | **HIGH** | Broadest and deepest evidence support across multiple independent sources [E01, E03-E07, E14] | Risks fatalism; does not account for potential evidence gaps on remediation successes; "impossible" framing may be overstated for specific network segments |
| **China (Denial)** | **LOW** | Serves diplomatic function; may resonate with audiences predisposed to distrust US claims | Zero factual support in evidence base; contradicted by overwhelming technical documentation [E01-E09, E14, E16-E18] |
| **Hawks (Offense)** | **LOW-MED** | Correctly identifies that defensive posture alone has historical limitations [E10]; dual-use threat is real [E17] | Core prescription (hack-back) is assessed as counterproductive by senior officials [E11]; does not address root vulnerability problem |

---

## Analytical Implications

The evidence stress-test reveals a significant asymmetry: the narrative with the highest resilience (critics/researchers arguing removal is essentially impossible) is also the narrative with the least political appeal. The narrative with the most political appeal (government/industry claiming progress) has the lowest factual resilience. This gap between evidence and political narrative is the central analytical vulnerability in the US response to Salt Typhoon.

The most productive analytical frame draws selectively from multiple narratives: the critics' structural diagnosis is the most evidence-supported starting point [E01, E03-E07]; the primary narrative's institutional mobilization represents necessary but insufficient action [E12, E13, E15]; and the hawks' urgency about timeline pressure is warranted even if their prescribed solution is counterproductive [E11, E13]. The Chinese denial narrative contributes nothing analytically but must be monitored for its diplomatic effects on coalition cohesion.

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
| [E20] | User-provided analytical framing note | 2026-02-15 | N/A | USER |

**Citation Methods:**
- **OSINT**: Retrieved via Firecrawl MCP or WebSearch/WebFetch fallback
- **FILE**: Read from local file system
- **USER**: Provided by analyst in conversation
- **ANALYSIS**: Analytical judgment produced via named technique protocol

> Every claim must be cited. Uncited claims are not permitted.

---

*Generated by Structured Analysis Skill -- Contrasting Narratives Technique | 2026-02-15*
