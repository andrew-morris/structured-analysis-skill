# Monitoring Plan: US Removal of Chinese Hackers from Networks

> **Analysis ID**: 2026-02-15-us-chinese-hackers-network-removal
> **Created**: 2026-02-15
> **Review Cadence**: Monthly (escalate to weekly if Taiwan Strait indicators activate)
> **Parent Analysis**: [Full Report](report.md)

---

## Key Judgments Under Watch

| # | Judgment | Current Confidence |
|---|---------|-------------------|
| J1 | Complete removal not achievable within 2-3 years under current conditions | HIGH |
| J2 | Managed persistence is the most plausible outcome | MODERATE |
| J3 | 12-18 month window of peak vulnerability (mid-2026 to 2027) | MODERATE-HIGH |
| J4 | Political will is the highest-leverage variable, currently at weakest state | HIGH |
| J5 | Publicly known campaigns may not represent full extent of Chinese access | LOW-MODERATE |

---

## Confirming Indicators

_Signals that the analysis is holding._

| # | Indicator | What to Look For | Source to Check | Status |
|---|-----------|------------------|-----------------|--------|
| C1 | Exposed device count remains high | Censys/Shodan scans continue showing >150,000 exposed network devices with Typhoon-associated vulnerabilities | Censys research reports, security researcher publications | ⬜ Not yet observed |
| C2 | CISA vacancy rates persist | CISA workforce reporting shows >25% vacancy rates through FY2026 | Congressional testimony, CISA workforce reports, Cybersecurity Dive coverage | ⬜ Not yet observed |
| C3 | Voluntary compliance framework continues | No binding federal cybersecurity mandates enacted for telecom carriers; FCC maintains voluntary approach | Federal Register, Congressional legislation tracker, FCC announcements | ⬜ Not yet observed |
| C4 | New Chinese campaigns discovered | Additional Typhoon-family or Brickstorm-class campaigns disclosed in US critical infrastructure | CISA advisories, vendor threat reports (CrowdStrike, Mandiant), Reuters/CyberScoop | ⬜ Not yet observed |
| C5 | Telecom eviction claims remain unverified | No independent audit confirms Salt Typhoon removal from tier-1 carriers; government advisories continue citing persistent access | Joint government advisories, Senate Commerce Committee hearings | ⬜ Not yet observed |
| C6 | Government rhetoric shifts to "resilience" | Official language moves from "removal" to "management," "containment," or "resilience" framing | White House statements, CISA guidance, Congressional hearing transcripts | ⬜ Not yet observed |
| C7 | OPM pattern repeats | If any diplomatic/punitive response occurs, Chinese operations temporarily pause then resume within 12-24 months | Threat intelligence reports, CISA advisories | ⬜ Not yet observed |

---

## Disconfirming Indicators

_Signals that the analysis may need revision._

| # | Indicator | What to Look For | Source to Check | Status |
|---|-----------|------------------|-----------------|--------|
| D1 | Binding federal cybersecurity mandates enacted | Congress passes legislation imposing mandatory patch timelines, credential rotation, or network segmentation requirements on telecom carriers with enforcement penalties | Congressional Record, Federal Register, White House signing statements | ⬜ Not yet observed |
| D2 | Rapid exposed device reduction | Censys/Shodan exposed device count drops below 100,000 within 6 months of measurement baseline | Censys research reports, security researcher publications | ⬜ Not yet observed |
| D3 | CISA workforce restored | CISA achieves <15% vacancy rate across key mission areas by end of FY2026 | CISA workforce reports, Congressional testimony | ⬜ Not yet observed |
| D4 | Verified independent eviction audit | A credible third-party (not carrier self-reporting) confirms Salt Typhoon removal from at least two tier-1 carriers | Government audit reports, independent security firm assessments | ⬜ Not yet observed |
| D5 | Insurance market imposes cybersecurity requirements | Major cyber insurers condition policy renewal for telecom carriers on specific security benchmarks | Insurance industry publications, carrier SEC filings, industry press | ⬜ Not yet observed |
| D6 | Chinese contractor ecosystem disrupted | Evidence of material disruption to i-SOON, Sichuan Juxinhe, or other Salt Typhoon contractor entities through sanctions, offensive operations, or internal failures | Treasury OFAC announcements, Five Eyes joint advisories, security researcher publications | ⬜ Not yet observed |
| D7 | AI-assisted remediation deployed at scale | DARPA or commercial tools enabling automated vulnerability scanning and patching deployed across telecom networks, significantly accelerating remediation pace | DoD program announcements, vendor press releases, Congressional testimony | ⬜ Not yet observed |
| D8 | Allied coordination produces measurable results | Five Eyes or broader allied coordination demonstrably degrades Chinese operations across multiple countries simultaneously | Joint government advisories, allied government announcements | ⬜ Not yet observed |
| D9 | No new Chinese campaigns for 12+ months | Absence of newly discovered Chinese APT campaigns in US critical infrastructure for a sustained period | CISA advisories, threat intelligence reports | ⬜ Not yet observed |
| D10 | Political activation event occurs | A galvanizing crisis (classified briefing leak, visible infrastructure disruption, Taiwan Strait escalation) triggers emergency political mobilization for cybersecurity | Major news coverage, Congressional emergency sessions, executive orders | ⬜ Not yet observed |

---

## Trigger Thresholds

### Reassessment Triggers

| Condition | Action |
|-----------|--------|
| **2+ disconfirming indicators observed** | Initiate partial reassessment — re-evaluate affected key judgments |
| **D1 (binding mandates) observed** | Immediate full reassessment — this is the highest-impact single indicator, invalidating the political trajectory assumption |
| **D10 (political activation) observed** | Immediate full reassessment — construct "activated political will" branch scenario per Premortem mitigation #1 |
| **D2 + D3 observed together** | Full reassessment — simultaneous rapid remediation and institutional rebuilding would challenge the core assessment |
| **3+ confirming indicators observed** | Increase confidence in current assessment; extend review cadence to quarterly |
| **Taiwan Strait activity reaches crisis threshold** | Shift review cadence to weekly; activate What If? scenario monitoring at full indicator set |

### Judgment-Specific Triggers

| Judgment | Trigger for Revision |
|----------|---------------------|
| J1 (removal not achievable) | D1 + D2 + D3 observed — mandatory regulation, rapid device reduction, and CISA restoration together could make significant removal plausible |
| J2 (managed persistence) | D4 observed — verified eviction would challenge the persistence assessment |
| J3 (vulnerability window) | D3 observed before mid-2026 — early CISA restoration would narrow the window |
| J4 (political will) | D1 or D10 observed — binding mandates or political activation event would transform this variable |
| J5 (deeper access) | Discovery of campaigns with >24-month dwell in previously "cleared" networks — would elevate this from LOW-MODERATE to MODERATE-HIGH |

---

## Evidence Collection Priorities

_Targeted collection to fill gaps identified in the analysis._

| Priority | Gap | Collection Target | Timeline |
|----------|-----|-------------------|----------|
| 1 | Telecom industry remediation progress | Carrier quarterly security reports, SEC filings on cybersecurity investment, industry press | Q2 2026 |
| 2 | Cyber insurance market dynamics | Insurance industry publications, broker reports on telecom sector risk pricing | Q2 2026 |
| 3 | DARPA/DoD automation programs | Program announcements, Congressional testimony, defense industry press | Q2 2026 |
| 4 | Allied coordination effectiveness | Five Eyes joint advisory outcomes, allied government cybersecurity assessments | Q2-Q3 2026 |
| 5 | Chinese contractor ecosystem vulnerabilities | i-SOON leak analysis updates, OFAC sanctions tracking, OPSEC failure reporting | Ongoing |

---

## Review Log

| Date | Reviewer | Indicators Updated | Action Taken |
|------|----------|-------------------|--------------|
| 2026-02-15 | Structured Analysis Skill | Initial creation | Monitoring plan established with 7 confirming and 10 disconfirming indicators |

---

*Generated by Structured Analysis Skill | 2026-02-15*
