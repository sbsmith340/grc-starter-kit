# NIST CSF 2.0 mapping

How each artifact in this kit, and in the companion repos, supports the six CSF 2.0 functions.

| Function | Category | Artifact |
|---|---|---|
| **Govern (GV)** | GV.RM Risk management strategy | [Risk methodology](02-risk-methodology.md) (appetite, scoring, acceptance rules) |
| | GV.RR Roles & responsibilities | [IRP](03-incident-response-plan.md) §2; risk owners in the register |
| | GV.SC Supply chain risk | Third Parties tab of the [inventory workbook](../inventory/asset-inventory-template.xlsx); register R-006 |
| | GV.OV Oversight | [Risk report](../register/REPORT.md) heat maps; [remediation metrics](https://github.com/sbsmith340/vuln-remediation-metrics) |
| **Identify (ID)** | ID.AM Asset management | [Inventory workbook](../inventory/asset-inventory-template.xlsx) + [collection queries](../inventory/collection-queries.md) |
| | ID.AM-07 Data inventory & classification | [Data classification standard](01-data-classification-standard.md) |
| | ID.RA Risk assessment | [Risk register](../register/risk_register.csv) |
| **Protect (PR)** | PR.AA Identity & access | [Deprovisioning audit, identity hygiene](https://github.com/sbsmith340/powershell-security-toolkit) |
| | PR.DS Data security | Handling rules by classification level (standard §5) |
| | PR.PS Platform security | [AllSigned rollout](https://github.com/sbsmith340/powershell-security-toolkit/blob/main/docs/AllSigned-rollout.md); server lifecycle inventory |
| **Detect (DE)** | DE.CM Continuous monitoring | [KQL detection library](https://github.com/sbsmith340/kql-detection-library) |
| | DE.AE Adverse event analysis | [Phishing triage playbook](https://github.com/sbsmith340/sentinel-phishing-triage) |
| **Respond (RS)** | RS.MA / RS.AN / RS.CO | [Incident response plan](03-incident-response-plan.md) |
| **Recover (RC)** | RC.RP Recovery plan execution | IRP §4.3; register R-004 (immutable backups, restore testing) |

## Gaps this kit does not cover yet
- Business continuity / disaster recovery plan (RC, beyond incident recovery)
- Security awareness training program (PR.AT)
- Formal policy set (acceptable use, access control, change management)
