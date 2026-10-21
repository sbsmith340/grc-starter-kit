# Incident Response Plan

| | |
|---|---|
| **Organization** | Contoso (fictional example) |
| **Plan owner** | Information Security Officer |
| **Version** | 1.0 · Template |
| **Tested** | Tabletop exercise semi-annually; last test: ____ |
| **Aligned to** | NIST SP 800-61 Rev. 3 / NIST CSF 2.0 (Respond, Recover), HIPAA §164.308(a)(6) |

## 1. Scope
Any event that threatens the confidentiality, integrity, or availability of Contoso information or systems. That includes systems run by third parties on our behalf.

## 2. Roles
| Role | Responsibility | Primary | Backup |
|---|---|---|---|
| Incident Commander | Owns the response, decisions, and timeline | ISO | IT Director |
| Technical Lead | Investigation, containment, eradication | Security Analyst | Systems Engineer |
| Communications Lead | Internal and external messaging | COO | HR Director |
| Legal / Privacy | Notification obligations, privilege, law enforcement | Counsel | Compliance Officer |
| Scribe | Timeline and evidence log | Any trained staff | — |

Keep the contact sheet (phone, personal email) **outside** the systems that might be down: a printed copy plus an offline copy.

## 3. Severity
| Severity | Definition | Examples | Response |
|---|---|---|---|
| **SEV-1** | Confirmed compromise of High-classified data or business-wide outage | Ransomware, confirmed PHI exfiltration, domain admin compromise | IC engaged within 30 min, 24/7; executives informed within 1 h |
| **SEV-2** | Confirmed compromise, contained to a few systems or accounts | Mailbox takeover, malware on a few endpoints | IC engaged within 2 h (business hours) |
| **SEV-3** | Suspicious activity, not yet confirmed | Phishing clicked with no credential entry, policy violation | Analyst triage within 1 business day |

## 4. Phases
### 4.1 Detect & analyze
- Sources: Sentinel incidents, Defender alerts, user reports, vendor or partner notices
- Open a ticket and start the timeline (UTC) **before** acting
- Decide: is it an incident? What severity? What data classification is involved?

### 4.2 Contain
| Scenario | Immediate containment |
|---|---|
| Compromised account | Revoke sessions (`Revoke-MgUserSignInSession`), reset password, remove MFA methods added by the attacker, check inbox rules and OAuth grants |
| Compromised endpoint | Isolate the device in Defender (keeps EDR telemetry), collect an investigation package |
| Ransomware | Isolate affected segments, **disconnect backups**, preserve encrypted samples and ransom notes |
| Data exposure (link/share) | Remove the share, capture access logs, identify what was accessed |

**Preserve evidence before you wipe anything.** Take a snapshot or investigation package first.

### 4.3 Eradicate & recover
- Remove persistence: scheduled tasks, services, inbox rules, OAuth apps, new admin accounts
- Patch the entry point
- Restore from known-good backups, then monitor at a higher level for 30 days

### 4.4 Post-incident
- Hold a blameless review within 10 business days
- Record: timeline, root cause, what worked, what didn't, actions with owners and dates
- Update the risk register and any detection that missed the incident

## 5. Notification decision
| Question | If yes |
|---|---|
| Was unsecured **PHI** involved? | HIPAA breach risk assessment; notify individuals within 60 days of discovery if it's a breach; HHS notification per the size thresholds |
| Was **PII** of state residents involved? | Check state breach laws (Georgia: O.C.G.A. § 10-1-912) |
| Does a **contract** require notice? | Follow its clause; federal contracts often require notice within hours |
| Is **cyber insurance** in place? | Call the carrier's hotline **before** engaging outside forensics |

Legal/Privacy owns this decision. Technical staff do not notify external parties on their own.

## 6. Playbook index
| Playbook | Trigger |
|---|---|
| Phishing triage | User-reported email ([automated triage](https://github.com/sbsmith340/sentinel-phishing-triage)) |
| Account compromise | Impossible travel, MFA fatigue, risky sign-in |
| Malware / ransomware | EDR high-severity alert |
| Lost or stolen device | User report |
| Insider data theft | DLP or mass-download alert |

## 7. Testing & maintenance
- Tabletop exercise every 6 months, rotating scenarios (ransomware, BEC, vendor breach)
- Review the plan after every SEV-1/SEV-2 incident and at least annually
- Metrics: mean time to detect, mean time to contain, % of incidents with a completed post-incident review
