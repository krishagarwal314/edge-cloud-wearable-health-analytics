# Work Distribution

**Project:** Cloud-Based Wearable Health Analytics Platform using Edge–Cloud Intelligence
**Course:** Cloud Computing
**Last updated:** 2026-07-31

> Replace `<Member N>` with actual names and roll numbers before submission. Keep the
> "Status" column current — it is the single source of truth for who owns what.

---

## 1. Role Summary

| Member | Role | Owns these folders |
|---|---|---|
| **<Member 1>** | Cloud Infrastructure & DevOps Lead | `infrastructure/`, `.github/workflows/`, `docs/aws/` |
| **<Member 2>** | Backend / Serverless Services Lead | `backend/`, `database/`, `tests/integration/` |
| **<Member 3>** | AI/ML & Edge Intelligence Lead | `ai-models/`, `edge/`, `dataset/` |
| **<Member 4>** | Frontend, Visualisation & Documentation Lead | `frontend/`, `docs/`, `results/`, `presentation/` |

Shared responsibility: code review (every PR needs one approval from a member who did not
author it), `README.md`, and demo rehearsal.

---

## 2. Detailed Task Allocation

### <Member 1> — Cloud Infrastructure & DevOps Lead

| ID | Task | Deliverable | Status |
|---|---|---|---|
| I-1 | AWS account setup, IAM users, MFA, budget + billing alarm at $1 | Account runbook in `docs/setup/SETUP.md` | ☐ |
| I-2 | Author SAM/CloudFormation template for the full stack | `infrastructure/cloudformation/template.yaml` | ☐ |
| I-3 | IoT Core: thing type, policy, X.509 certificate provisioning script | `infrastructure/scripts/provision_device.sh` | ☐ |
| I-4 | Least-privilege IAM roles per Lambda | `docs/aws/IAM_NOTES.md` | ☐ |
| I-5 | CI/CD: GitHub Actions with OIDC federation to AWS, `sam deploy` on merge to `main` | `.github/workflows/deploy.yml` | ☐ |
| I-6 | CloudWatch dashboards, log-retention policies (7 days, to stay free), alarms | Dashboard JSON + screenshots in `results/` | ☐ |
| I-7 | Free Tier quota tracking; weekly cost report | `docs/aws/FREE_TIER_BUDGET.md` | ☐ |
| I-8 | S3 + CloudFront static hosting for the SPA, cache invalidation on deploy | Template section + deploy script | ☐ |

### <Member 2> — Backend / Serverless Services Lead

| ID | Task | Deliverable | Status |
|---|---|---|---|
| B-1 | Define the telemetry message schema and validate it | `backend/common/schema.py`, `docs/architecture/API_CONTRACT.md` | ☐ |
| B-2 | `ingest_handler` Lambda: validate, normalise, fan out to DynamoDB + S3 | `backend/lambdas/ingest_handler/` | ☐ |
| B-3 | `anomaly_processor` Lambda: cloud-side confirmation inference | `backend/lambdas/anomaly_processor/` | ☐ |
| B-4 | `alert_dispatcher` Lambda: dedupe/debounce, publish to SNS | `backend/lambdas/alert_dispatcher/` | ☐ |
| B-5 | `api_handler` Lambda + API Gateway routes (`/vitals`, `/alerts`, `/devices`) | `backend/lambdas/api_handler/`, OpenAPI spec | ☐ |
| B-6 | DynamoDB single-table design, GSIs, TTL policy | `database/schemas/` | ☐ |
| B-7 | Cognito user pool integration + JWT authorizer wiring | Template section + `backend/api/` | ☐ |
| B-8 | Integration tests with `moto`; load test harness | `tests/integration/` | ☐ |

### <Member 3> — AI/ML & Edge Intelligence Lead

| ID | Task | Deliverable | Status |
|---|---|---|---|
| M-1 | Acquire and document MIT-BIH, PTB-XL, WESAD; write download scripts | `dataset/`, `ai-models/preprocessing/` | ☐ |
| M-2 | Preprocessing pipeline: filtering, resampling to 125 Hz, windowing, normalisation | `ai-models/preprocessing/` | ☐ |
| M-3 | Baseline models (threshold rules, isolation forest) for comparison | `ai-models/notebooks/01_baselines.ipynb` | ☐ |
| M-4 | 1-D convolutional autoencoder; train, tune reconstruction-error threshold | `ai-models/training/train_autoencoder.py` | ☐ |
| M-5 | INT8 post-training quantisation → TFLite; verify accuracy delta < 2 % | `ai-models/models/`, conversion script | ☐ |
| M-6 | Edge inference runtime + store-and-forward MQTT publisher | `edge/edge_inference/`, `edge/simulator/` | ☐ |
| M-7 | Cloud-side confirmation model (larger, non-quantised) packaged for Lambda | `ai-models/models/cloud/` | ☐ |
| M-8 | Model evaluation: precision, recall, F1, ROC-AUC, confusion matrices | `results/benchmarks/model_eval.md` | ☐ |

### <Member 4> — Frontend, Visualisation & Documentation Lead

| ID | Task | Deliverable | Status |
|---|---|---|---|
| F-1 | React + Vite app scaffold, routing, theming (light/dark) | `frontend/` | ☐ |
| F-2 | Cognito-backed login/logout flow | `frontend/src/services/auth.js` | ☐ |
| F-3 | Live vitals view (HR, SpO₂, temperature) with polling/WebSocket updates | `frontend/src/pages/LiveVitals.jsx` | ☐ |
| F-4 | Historical trends: time-range picker, aggregate charts | `frontend/src/pages/Trends.jsx` | ☐ |
| F-5 | Alert timeline + acknowledgement action | `frontend/src/pages/Alerts.jsx` | ☐ |
| F-6 | Device management view (register/deactivate a device) | `frontend/src/pages/Devices.jsx` | ☐ |
| F-7 | Literature survey + research-gap documents | `docs/literature-survey/` | ☐ |
| F-8 | Architecture diagrams, final report, slide deck, demo script | `docs/architecture/`, `presentation/` | ☐ |

---

## 3. Timeline (12-week plan)

| Week | Milestone | Lead |
|---|---|---|
| 1 | Problem finalisation, literature survey | M4 (all contribute) |
| 2 | Architecture design, repo scaffold, work split | All |
| 3 | AWS account, IAM, billing alarms, IoT Core skeleton | M1 |
| 4 | Telemetry schema + `ingest_handler` + DynamoDB tables | M2 |
| 5 | Dataset acquisition + preprocessing pipeline | M3 |
| 6 | Edge simulator publishing to IoT Core end-to-end | M3 + M1 |
| 7 | Autoencoder training + quantisation | M3 |
| 8 | Anomaly processor + SNS alerting path | M2 |
| 9 | REST API + Cognito auth | M2 + M4 |
| 10 | Dashboard: live view, trends, alerts | M4 |
| 11 | Benchmarking, elasticity demo, cost analysis | M1 + M3 |
| 12 | Report, slides, demo rehearsal, submission | All |

---

## 4. Collaboration Conventions

- **Branching:** `main` (protected) ← `dev` ← `feature/<member-initials>-<short-desc>`
- **Commits:** Conventional Commits (`feat:`, `fix:`, `docs:`, `infra:`, `test:`)
- **PRs:** must state which task ID they close; require one non-author approval
- **Issues:** one GitHub issue per task ID above, labelled by area
- **Stand-up:** async written update in the team channel, twice weekly
- **Secrets:** never committed — use AWS Systems Manager Parameter Store and GitHub
  Actions OIDC. `.env` is gitignored.

---

## 5. Contribution Ledger (fill in before submission)

| Member | Commits | PRs merged | Reviews given | Approx. contribution |
|---|---|---|---|---|
| <Member 1> | | | | 25 % |
| <Member 2> | | | | 25 % |
| <Member 3> | | | | 25 % |
| <Member 4> | | | | 25 % |
