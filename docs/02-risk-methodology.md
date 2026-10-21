# Risk Assessment Methodology

Every risk in [`register/risk_register.csv`](../register/risk_register.csv) is scored the same way, so the heat map and the "top risks" list mean something.

## 1. Describe the risk in one sentence
**[Threat] exploits [vulnerability] in [asset], causing [impact].**
> *A phishing actor uses stolen credentials against legacy authentication on the mail tenant, causing mailbox takeover and invoice fraud.*

## 2. Rate the CIA impact
Use the [classification standard](01-data-classification-standard.md) levels (Low / Moderate / High) for what the risk would do to the confidentiality, integrity, and availability of the affected information.

## 3. Score likelihood and business impact (1–5)

| Score | Likelihood (next 12 months) | Business impact |
|---|---|---|
| 1 | Rare: <5%, no known precedent | Negligible: absorbed in normal operations |
| 2 | Unlikely: 5–25% | Minor: < 1 day disruption, no external notice |
| 3 | Possible: 25–50%, has happened in the industry | Moderate: days of disruption, a client or regulator notified |
| 4 | Likely: 50–80%, has happened here or to peers | Major: contract loss, regulatory finding, material cost |
| 5 | Almost certain: >80%, active exploitation seen | Severe: existential, harm to individuals, sustained outage |

**Inherent risk = Likelihood × Impact** (1–25), before the planned controls.

| Score | Rating | Response expected |
|---|---|---|
| 20–25 | **Critical** | Executive owner; treatment plan within 30 days |
| 12–19 | **High** | Treatment plan within 90 days |
| 6–11 | **Medium** | Treat or formally accept; review semi-annually |
| 1–5 | **Low** | Accept and monitor; review annually |

## 4. Choose a treatment
**Mitigate** (add controls) · **Transfer** (insurance, contract) · **Avoid** (stop the activity) · **Accept**. Acceptance requires a named owner and an expiry date, and anything rated High or above needs executive sign-off.

## 5. Re-score after controls
**Residual risk = residual likelihood × residual impact**, assuming the recommended controls are in place and working. If the residual score is still above the organization's appetite (default: **≤ 11, Medium**), the treatment isn't finished.

## 6. Keep it honest
- Score from evidence: incident history, scan results, control test results. Don't score from how worried someone feels.
- Controls only lower a score once they've been **verified**, not when they're planned.
- `tools/risk_report.py` rejects rows where the residual score is higher than the inherent score, ratings are out of range, or an accepted risk has no owner or expiry.
