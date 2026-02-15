# Evidence Registry: Will the US Successfully Remove Chinese Hackers from Their Networks?

> **Analysis ID**: 2026-02-15-us-chinese-hackers-network-removal
> **Date**: 2026-02-15
> **Total Items**: 20

---

## Collection Summary

| Source Tier | Count | Methods Used |
|-------------|-------|-------------|
| Conversation Context | 1 | Direct extraction from analyst statements |
| Local Files | 0 | N/A |
| OSINT Research | 19 | firecrawl_search (3 themed queries) + firecrawl_scrape (8 full articles) |

---

## Evidence Inventory

| ID | Source | Type | Summary | Reliability | Diagnostic Value | Citation |
|----|--------|------|---------|-------------|-----------------|----------|
| E01 | War on the Rocks | OSINT | China "walked through unlocked doors" — Salt Typhoon exploited basic failures (weak passwords, 7-year-old unpatched Cisco vulnerabilities), not zero-days. Senate Commerce Committee concluded telecoms "still have not convincingly shown they have evicted the intruders" as of Dec 2025. | High | High | [Is America's Cyber Weakness Self-Inflicted?](https://warontherocks.com/2026/01/is-americas-cyber-weakness-self-inflicted/) — Retrieved: 2026-02-15 |
| E02 | War on the Rocks | OSINT | FCC rescinded binding cybersecurity orders for telecom carriers in late 2025, replacing them with voluntary industry collaboration framework. Sen. Cantwell criticized this as "stripping regulators of authority to fine carriers for the very vulnerabilities Salt Typhoon exploited." | High | High | [Is America's Cyber Weakness Self-Inflicted?](https://warontherocks.com/2026/01/is-americas-cyber-weakness-self-inflicted/) — Retrieved: 2026-02-15 |
| E03 | War on the Rocks | OSINT | Salt Typhoon breach "did not rely on Chinese hardware" but exploited "basic maintenance failures in U.S.-made equipment, including seven-year-old unpatched vulnerabilities in Cisco routers." Rip-and-replace of Chinese equipment does not address the actual attack vector. | High | High | [Is America's Cyber Weakness Self-Inflicted?](https://warontherocks.com/2026/01/is-americas-cyber-weakness-self-inflicted/) — Retrieved: 2026-02-15 |
| E04 | CyberScoop | OSINT | Multiple experts assess telecoms "may never fully expunge" Salt Typhoon. Three factors: network size/complexity, identity management difficulty, decades of industry consolidation creating "Frankenstein's monster of different equipment, technologies and architecture." | High | High | [Why telecoms may never purge Salt Typhoon](https://cyberscoop.com/salt-typhoon-chinese-hackers-us-telecom-breach/) — Retrieved: 2026-02-15 |
| E05 | CyberScoop/Censys | OSINT | Censys six-month analysis found 200,000+ public exposures of network/edge devices with vulnerabilities known to be exploited by Salt Typhoon. "Despite growing public awareness...there has been little meaningful reduction in exposed devices — just 25% since October 2024." | High | High | [Why telecoms may never purge Salt Typhoon](https://cyberscoop.com/salt-typhoon-chinese-hackers-us-telecom-breach/) — Retrieved: 2026-02-15 |
| E06 | CyberScoop | OSINT | Lack of confirmed, granular indicators of compromise (IOCs) for Salt Typhoon makes reliable threat hunting difficult. Researcher: "I feel like there's not enough for me to hunt on regularly and reliably to be able to say we have pretty good removal of this activity." | Medium | High | [Why telecoms may never purge Salt Typhoon](https://cyberscoop.com/salt-typhoon-chinese-hackers-us-telecom-breach/) — Retrieved: 2026-02-15 |
| E07 | CyberScoop/U. Florida | OSINT | University of Florida researchers found 100+ distinct exploitable vulnerabilities in common LTE/5G implementations. When they reported vulnerabilities, many maintainers "simply lacked the personnel or expertise to address the flaws." Lead researcher ended up writing most patches himself. | High | Medium | [Why telecoms may never purge Salt Typhoon](https://cyberscoop.com/salt-typhoon-chinese-hackers-us-telecom-breach/) — Retrieved: 2026-02-15 |
| E08 | Lawfare | OSINT | FBI estimates Salt Typhoon obtained "more than one million call records" and contacted 600+ companies impacted. AT&T/Verizon claims of eviction "difficult to reconcile" with joint advisory that Salt Typhoon maintains "persistent, long-term access to networks." | High | High | [Reconfiguring U.S. Cyber Strategy in the Wake of Salt Typhoon](https://www.lawfaremedia.org/article/reconfiguring-u.s.-cyber-strategy-in-the-wake-of-salt-typhoon) — Retrieved: 2026-02-15 |
| E09 | Lawfare | OSINT | Salt Typhoon compromised Army National Guard network (Mar-Dec 2024), stealing admin credentials, network traffic diagrams, cybersecurity personnel work locations, and PII. Stolen network diagram was then used to compromise another government agency's network. | High | Medium | [Reconfiguring U.S. Cyber Strategy in the Wake of Salt Typhoon](https://www.lawfaremedia.org/article/reconfiguring-u.s.-cyber-strategy-in-the-wake-of-salt-typhoon) — Retrieved: 2026-02-15 |
| E10 | Lawfare | OSINT | After OPM breach (2015), Obama chose not to retaliate; cyber operations "temporarily subsided" but surged by 2017. Pattern suggests diplomatic/punitive measures alone do not achieve lasting removal — adversary reorganizes and returns. | High | Medium | [Reconfiguring U.S. Cyber Strategy in the Wake of Salt Typhoon](https://www.lawfaremedia.org/article/reconfiguring-u.s.-cyber-strategy-in-the-wake-of-salt-typhoon) — Retrieved: 2026-02-15 |
| E11 | Lawfare | OSINT | "Hack-back operations do not remove vulnerabilities, the PRC could respond with further attacks on domestic networks." Anne Neuberger: US presidents "lack enough confidence that U.S. defenses could withstand a potentially escalatory tit-for-tat battle in cyberspace." | High | High | [Reconfiguring U.S. Cyber Strategy in the Wake of Salt Typhoon](https://www.lawfaremedia.org/article/reconfiguring-u.s.-cyber-strategy-in-the-wake-of-salt-typhoon) — Retrieved: 2026-02-15 |
| E12 | Cybersecurity Dive | OSINT | CISA lost more than one-third of its workforce under Trump administration, with "approximately 40% vacancy rate across key mission areas." Key experts pushed out, vital operations jeopardized, partnerships frozen. Acting director called this a "pivotal moment." | High | High | [CISA plans hiring spree to rebuild depleted ranks](https://www.cybersecuritydive.com/news/cisa-hiring-workforce-strategy/805733/) — Retrieved: 2026-02-15 |
| E13 | Cybersecurity Dive | OSINT | CISA acting director: "CISA must hire highly qualified professionals by the end of fiscal year 2026 to strengthen the agency's defensive posture." Experts predict potential US-China crisis in 2027, creating timeline pressure. | High | Medium | [CISA plans hiring spree to rebuild depleted ranks](https://www.cybersecuritydive.com/news/cisa-hiring-workforce-strategy/805733/) — Retrieved: 2026-02-15 |
| E14 | SC World | OSINT | Typhoon APT campaigns exploited flaws across Cisco, F5, Ivanti, Fortinet, Sonicwall, Netgear, Citrix, and Juniper equipment. CISA joint advisory noted success came from "lateral movement through exploitation of hidden software flaws and other avoidable weaknesses." | Medium | Medium | [China's Typhoon hackers have changed the rules](https://www.scworld.com/perspective/chinas-typhoon-hackers-have-changed-the-rules-of-cybersecurity) — Retrieved: 2026-02-15 |
| E15 | SC World | OSINT | EU Cyber Resilience Act, US DoD Software Fast Track (SWFT) Initiative (May 2025), and UK Government Cyber Action Plan represent emerging regulatory responses. "We're at the end of a three-decade-long hands-off approach to software security by governments." | Medium | Medium | [China's Typhoon hackers have changed the rules](https://www.scworld.com/perspective/chinas-typhoon-hackers-have-changed-the-rules-of-cybersecurity) — Retrieved: 2026-02-15 |
| E16 | Reuters | OSINT | Dec 2025: New "Brickstorm" malware used by Chinese-linked hackers to penetrate and maintain "long-term access" to government and IT entities. In one case, maintained access from April 2024 through at least September 2025 (17 months). Deployed against VMware vSphere. | High | Medium | [Chinese-linked hackers use Brickstorm backdoor](https://www.reuters.com/world/china/chinese-linked-hackers-use-back-door-potential-sabotage-us-canada-say-2025-12-04/) — Retrieved: 2026-02-15 |
| E17 | DomainTools | OSINT | Salt Typhoon = MSS-aligned dual-use capability (espionage + wartime disruption). Contractor-enabled model using i-SOON, Sichuan Juxinhe, Beijing Huanyu Tianqiong. 600+ organizations breached worldwide, 200+ in US, operations across 80+ countries. Active since 2019. | High | Medium | [Inside Salt Typhoon](https://dti.domaintools.com/research/inside-salt-typhoon-chinas-state-corporate-advanced-persistent-threat) — Retrieved: 2026-02-15 |
| E18 | DomainTools | OSINT | Salt Typhoon leverages "contractor-enabled, industrial-scale operations" with front companies, scalable infrastructure, and modular tooling. "Capabilities are modular, and deniability is built into every layer of the intrusion lifecycle." This is "China's next-generation cyber espionage model." | High | Medium | [Inside Salt Typhoon](https://dti.domaintools.com/research/inside-salt-typhoon-chinas-state-corporate-advanced-persistent-threat) — Retrieved: 2026-02-15 |
| E19 | FDD | OSINT | Salt Typhoon "collected intelligence on nearly every American." Legacy TDM infrastructure in FAA radar and railroad systems creates persistent vulnerability. China "perceives U.S. telecommunications networks as a key center of gravity in a possible conflict." FCC removed cybersecurity requirements after Salt Typhoon. | High | Medium | [Advancing IP Interconnection](https://www.fdd.org/analysis/2025/12/16/advancing-ip-interconnection/) — Retrieved: 2026-02-15 |
| E20 | Analyst | USER | The analytical question "Will the US be able to successfully remove Chinese hackers from their networks?" frames removal as a binary success/failure, which may obscure partial or incremental outcomes. | N/A | Low | [User-provided, session context] |

**Diagnostic Value Key:**
- **High**: Contradicts or strongly supports specific hypotheses
- **Medium**: Supports some hypotheses, neutral for others
- **Low**: Mildly relevant, does not discriminate
- **Zero**: Consistent with ALL hypotheses (per Law of Diagnostic Dominance — carries no analytical weight)

---

## Quality Assessment

### Evidence Balance
Evidence is heavily weighted toward skepticism about removal success (~16 items supporting difficulty/impossibility vs. ~3 items noting emerging regulatory responses and CISA rebuilding). This imbalance reflects the genuine state of reporting but analysts should be alert to confirmation bias toward pessimistic assessment. Notably absent: direct statements from telecom companies about remediation progress, Chinese strategic intent analysis beyond Western interpretation, classified intelligence assessments.

### Identified Gaps
- **Telecom industry perspective**: No direct carrier statements on remediation progress or investment
- **Chinese strategic calculus**: Limited analysis of whether China might voluntarily reduce operations under certain conditions
- **Allied/international response**: Minimal evidence on Five Eyes coordination effectiveness
- **Private sector investment**: No data on telecom cybersecurity spending trends
- **Technical remediation**: No detailed technical assessments of specific network segments that have been successfully hardened

### Deception Indicators
- Chinese embassy denial ("we reject the relevant parties' irresponsible assertion") is standard diplomatic response and carries no analytical weight
- Telecom company claims of eviction (AT&T, Verizon, Lumen) are contradicted by joint government advisory and expert assessment — possible corporate reputation management
- No disinformation indicators detected in OSINT sources; all are established Western publications and government/think tank sources

---

## Source Reliability Ratings

| Rating | Criteria |
|--------|----------|
| **High** | Established source with track record of accuracy; multiple corroborating sources |
| **Medium** | Known source but limited track record; partial corroboration available |
| **Low** | Unknown or untested source; no corroboration; potential bias identified |

---

*Generated by Structured Analysis Skill | 2026-02-15*
