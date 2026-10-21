# Data Classification Standard

| | |
|---|---|
| **Organization** | Contoso (fictional example) |
| **Owner** | Information Security Officer |
| **Version** | 1.0 · Template |
| **Review cycle** | Annual, or when a new system or data type is introduced |
| **Framework references** | NIST SP 800-60, FIPS 199, NIST CSF 2.0 ID.AM-07, HIPAA §164.308(a)(1) |

## 1. Purpose
Give every system owner one repeatable way to decide how much protection information needs, based on what happens if it's disclosed (**C**onfidentiality), altered (**I**ntegrity), or unavailable (**A**vailability).

## 2. Method
1. **Use the eight fixed information types below.** Every assessment uses the same eight. Only the system changes.
2. Rate each type that the system stores, processes, or transmits as **Low / Moderate / High** for C, I, and A separately, using the impact definitions in §3.
3. The **system's overall category is the highest rating across all of its types** (FIPS 199 "high-water mark").
4. Apply the handling requirements in §5 for that level.

Keeping the eight types fixed is deliberate. Results stay comparable across systems and over time, and reviewers learn one table instead of a new one per assessment.

## 3. Impact definitions
| Level | Meaning if C, I, or A is lost |
|---|---|
| **Low** | Limited adverse effect: minor inconvenience, no regulatory exposure, easily corrected |
| **Moderate** | Serious adverse effect: financial loss, contractual breach, reportable incident possible, significant rework |
| **High** | Severe or catastrophic effect: regulatory penalties, harm to individuals, loss of a major contract, inability to perform core mission |

## 4. Information types and baseline ratings
Baselines are a starting point. The system owner may raise a rating with justification. Lowering one needs ISO approval.

| # | Information type | Examples | C | I | A | Notes |
|---|---|---|---|---|---|---|
| 1 | **PII** | Names with SSN/DOB, HR records, PHI, benefits data | High | Moderate | Moderate | PHI makes HIPAA apply; breach notification rules apply |
| 2 | **No PII (general / company-owned)** | Policies, internal wikis, project plans with no personal data | Low | Low | Low | Most information falls here |
| 3 | **Security** | Firewall configs, vulnerability scans, incident records, credentials | High | High | Moderate | Disclosure directly helps an attacker |
| 4 | **Contracts** | Signed agreements, SOWs, pricing, NDAs | Moderate | Moderate | Low | Obligations may impose their own handling |
| 5 | **Financial** | GL, payroll totals, bank details, invoices | Moderate | High | Moderate | Integrity drives fraud risk |
| 6 | **Deliverables** | Work product owed to clients | Moderate | High | Moderate | Client agreements may raise C |
| 7 | **Communications** | Email, chat, meeting recordings | Moderate | Low | Moderate | Inherits the highest type discussed in it |
| 8 | **Attorney–Client Privilege** | Legal advice, litigation material | High | Moderate | Low | Disclosure can waive privilege |

## 5. Handling requirements by level
| Control | Low | Moderate | High |
|---|---|---|---|
| Label | None required | "Internal" | "Confidential" (sensitivity label enforced) |
| Encryption at rest | Platform default | Required | Required; customer-managed keys where available |
| Encryption in transit | TLS 1.2+ | TLS 1.2+ | TLS 1.2+; no email to personal addresses |
| Access | All staff | Role-based | Role-based + quarterly access review |
| MFA | Required (all) | Required | Phishing-resistant MFA for admins |
| Sharing externally | Allowed | Owner approval | ISO approval + DLP enforced |
| Logging | Platform default | Access logged | Access logged + alerting on bulk download |
| Backup / recovery | Best effort | Daily, tested annually | Daily + immutable copy, tested semi-annually |
| Disposal | Standard delete | Secure delete | Certified destruction / crypto-erase |

## 6. Worked example: a cloud HR system
| Type | Present? | C | I | A |
|---|---|---|---|---|
| PII | Yes (employee records) | High | Moderate | Moderate |
| No PII | Yes (org chart) | Low | Low | Low |
| Security | Yes (admin audit logs) | High | High | Moderate |
| Financial | Yes (salary bands) | Moderate | High | Moderate |
| Communications | Yes (case notes) | Moderate | Low | Moderate |
| Contracts / Deliverables / Privilege | No | — | — | — |
| **System high-water mark** | | **High** | **High** | **Moderate** |

**Result:** handle as **High** for confidentiality and integrity, and **Moderate** for availability. That drives quarterly access reviews, sensitivity labels, DLP on exports, and daily backups.

## 7. Assessment worksheet
Copy this table for each system:

| Type | Present? | C | I | A | Justification for any change from baseline |
|---|---|---|---|---|---|
| PII | | | | | |
| No PII | | | | | |
| Security | | | | | |
| Contracts | | | | | |
| Financial | | | | | |
| Deliverables | | | | | |
| Communications | | | | | |
| Attorney–Client Privilege | | | | | |
| **High-water mark** | | | | | |
