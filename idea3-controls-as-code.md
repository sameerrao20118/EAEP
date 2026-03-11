# Idea 3 — Controls-as-Code: Preventative, Automated Controls

## Clarifying Questions
- Which control frameworks apply (CIS, NIST, SOC2, ISO, PCI)? Which controls are mandatory vs advisory?
- What CI/CD platforms are in scope (GitHub Actions, Jenkins, GitLab, Azure DevOps)? Language/runtime coverage?
- What environments need gates (dev, test, pre-prod, prod)? Current change approval model?
- Existing policy engines (OPA, Sentinel, Kyverno) in use? Appetite to standardize?
- Where is evidence stored today for audits? Required retention and attestation format?
- How are exceptions handled and approved? Who are control owners?
- Are infrastructure definitions IaC (Terraform/CloudFormation) and app configs (Helm/Kustomize) available to evaluate?
- Performance constraints for gates (max acceptable pipeline latency)?

## Solution Architecture
- **Policy Authoring:** Central repo for Rego (OPA) + policy packs per domain (infra, K8s, CI, data). Versioned, code-reviewed, unit-tested.
- **Policy Distribution:** OPA bundles served via artifact store; sidecar/admission controllers on K8s; CLI/CI plugins for Terraform/Helm; webhook for container registry admission.
- **Controls in Delivery Flow:**  
  - Pre-commit: linters + policy tests.  
  - CI: IaC validation (terraform plan), container scan (Trivy/Grype), SBOM gen (Syft), OPA checks.  
  - CD: K8s admission with Kyverno/OPA Gatekeeper; environment drift checks; runtime config guards.  
- **Exception Workflow:** Lightweight service backed by Jira/ServiceNow; time-bounded waivers with auto-expiry and audit trail.
- **Evidence & Reporting:** Central evidence store (S3/Blob) with metadata; dashboards for pass/fail by control, MTTR for fixes, exception volume; automated attestations per release.
- **Observability:** Metrics on gate latency, control coverage, and failure reasons; alerting on bypasses or expired waivers.

## Implementation Plan (12 weeks)
1. **Align & Select (Weeks 1–2):** Map top 10 priority controls; choose OPA/Gatekeeper + pipeline plugins; define evidence schema; set SLO for gate latency.
2. **Bootstrap (Weeks 3–4):** Create policy repo + CI for policy tests; ship first bundles (Terraform/K8s basics: tagging, network egress, secrets, image signing).
3. **CI/CD Integration (Weeks 5–7):** Add pipeline steps to two anchor services; enable admission controller in non-prod; start collecting evidence and dashboards.
4. **Scale & Exceptions (Weeks 8–10):** Build waiver workflow; expand control pack (data egress, PII tagging, TLS, SBOM + signing); rollout to additional teams.
5. **Harden & Certify (Weeks 11–12):** Performance tuning, SLO alerts, red/blue testing of controls, document operating model, prep audit attestation.

## Deliverables
- Policy-as-code repo with unit tests and bundle publishing.
- CI/CD plugins and K8s admission controllers enforcing agreed controls.
- Evidence store + dashboards + automated attestations per build/release.
- Waiver/exception workflow with auto-expiry and reporting.

## Risks & Mitigations
- **Pipeline slowdown:** Enforce SLOs, parallelize checks, cache scans.  
- **Low adoption:** Provide starter templates, pair with 2 pilot teams, advisory phase first.  
- **Policy drift:** CI tests + signed bundles; periodic conformance scans of live clusters.  
- **Audit gaps:** Standard evidence schema; fail if evidence missing; regular dry-run audits.  
