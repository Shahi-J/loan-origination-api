# Loan Origination API — Postman Onboarding

This repo contains the automated Postman onboarding workflow for FinServCo's 
Loan Origination API service.

---

## What This Does

Triggering this workflow automatically creates a fully structured Postman 
workspace for the Loan Origination API, including:

- A named workspace: [LND] loan-origination-api
- The service spec uploaded to Postman Spec Hub
- Three auto-generated test collections built from the spec:
  - Baseline — every endpoint in the API, ready to call
  - Smoke — lightweight health checks to verify the API is responding
  - Contract — tests that verify the API behaves exactly as the spec describes
- Prod and staging environments configured with their respective base URLs

---

## Service Details

| Field | Value |
|---|---|
| Service | Loan Origination API |
| Domain | Lending |
| Domain Code | LND |
| Infrastructure | ECS behind Application Load Balancer |
| Auth Pattern | mTLS + JWT |
| Spec Source | postman-cs/cse-exercise |

---

## How to Run

1. Go to the Actions tab in this repo
2. Click Postman API Onboarding - Loan Origination API
3. Click Run workflow
4. Click the green Run workflow button
5. Open Postman — the workspace will be created automatically

---

## What Changes Per Service

Only four lines in the workflow file need to change for each new service:

- project-name — the name of the service
- domain — the business domain it belongs to
- domain-code — the short prefix used in workspace naming
- spec-url — the web address where the service's OpenAPI spec is hosted

Everything else — the secrets, the permissions, the action reference — 
stays identical.

---

## Why This Service Was Chosen

The Loan Origination API was deliberately chosen as the second pilot service 
because it differs from the Payment Refund API across every dimension that 
matters for proving scalability:

- Different infrastructure: ECS/ALB vs Lambda/API Gateway
- Different auth: mTLS + JWT vs OAuth + JWT

This demonstrates that the onboarding pattern handles meaningfully different 
services without the workflow itself needing to change. Only four lines of 
config changed between the two services — proving the template scales across 
the platform team's entire portfolio.

---

## What the Platform Team Needs to Provide

Before onboarding any service, the platform team needs:

- The web address of the service's OpenAPI spec
- The production and staging base URLs for the service
- Auth credentials stored as GitHub secrets — never in the workflow file itself

These are collected via the service intake form before onboarding begins.

---

## Secrets Required

Three secrets are configured at the repo level for this exercise. In a 
production engagement these would be set once at the GitHub organisation 
level by the Senior DevOps Engineer:

- POSTMAN_API_KEY — long-lived Postman API key for all Postman operations
- POSTMAN_ACCESS_TOKEN — session token for governance and workspace linking. 
  Expires periodically and requires manual refresh until Postman ships the 
  GA fix
- GH_FALLBACK_TOKEN — GitHub Personal Access Token with repo and workflow 
  permissions, used for writing generated workflow files back to the repo

---

## What Happens on a Rerun

The bootstrap action checks for stored repository variables before creating 
anything new. If the workspace already exists, it updates rather than 
duplicates. This means the workflow is safe to rerun whenever the spec changes.

---

## Issues Encountered

No additional issues were encountered onboarding this service. The same 
workflow template from the Payment Refund API was reused with four lines 
changed — validating that the pattern works across services with different 
infrastructure and auth profiles without any additional troubleshooting.

---

## How AI Was Used

Claude was used to:
- Read and interpret the action READMEs
- Generate the initial workflow YAML based on the README documentation
- Troubleshoot errors encountered during the build
- Writing the Intake Form based on my design and requirements

The following was done and validated personally:
- All customer consultation, engagement strategy, and solution design
- Identifying the right two services to pilot based on infrastructure and 
  auth differences — proving the pattern scales across different service types
- Designing the low friction onboarding model with a focus on scalability — one YAML template, four 
  lines of config, no spec copying, no manual setup per service
- Designing the intake form so the platform team never has to chase 
  information or make decisions about naming and governance
- Identifying that secrets should live at the org level so the platform 
  team never touches credentials when onboarding a new service
- Identifying that domain and domain code should be standardised dropdowns 
  to prevent workspace sprawl at scale
- Recognising that the spec URL comes directly from the service team — 
  the platform team never downloads, copies or manages spec files
- Correcting workflow errors encountered during the build and understanding 
  why they occurred
- Triggering and validating both workflow runs end to end
- Confirming the Postman workspaces, collections and environments were 
  created correctly in both cases
