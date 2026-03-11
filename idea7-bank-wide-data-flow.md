# Idea 7 — Bank-wide Data Flow Auto-Discovery & Golden-Source Guardrails

## Clarifying Questions (to sponsors/SMEs)
- What are the authoritative systems of record per domain (payments, deposits, CRM, risk)? Are there existing data contracts?
- Which data platforms are in scope first (warehouse, lake, streaming)? What are current lineage/metadata tools (e.g., Collibra, Alation, OpenMetadata)?
- Required freshness for lineage updates (near-real-time vs daily)? Any regulatory SLAs (e.g., BCBS 239 lineage depth)?
- Do we already have a schema registry in production? Can we mandate registration before production release?
- Which languages/runtimes need probes (Java, Python, Spark, SQL, Kafka Connect)? Any mainframe feeds?
- Do we need active enforcement (block non-authoritative sources) or only detect/score initially?
- How is “golden” decided today—by data owner, catalog certification, or policy tags? Who approves?
- Reporting: which audiences need dashboards—engineering, risk, data governance? Tooling preference (PowerBI/Tableau/QuickSight)?

## Solution Architecture (textual)
- **Data Plane:** Kafka/MSK streams, RDBMS (Aurora/Oracle), data lake (S3/Delta/Iceberg), warehouse (Redshift/Snowflake), ETL/ELT jobs (Spark/Flink/DBT/Glue).
- **Discovery & Lineage:** OpenMetadata/Collibra ingesting from databases, Kafka Schema Registry, DBT/Glue/Spark lineage extractors, code scanners (SQL parsers), runtime probes on Kafka consumers/producers and JDBC clients.
- **Schema Registry:** Confluent/Glue Schema Registry; mandatory for all event schemas. Versioning with compatibility checks (backward/forward).
- **Golden Source Catalog:** Certified datasets with ownership, quality SLAs, retention, and allowed-use policies; surfaced via catalog UI and APIs.
- **Risk & Policy Engine:** OPA-based policy checks for source authorization, freshness, and certification; scores non-authoritative usage; can fail builds or raise advisories.
- **Enforcement Hooks:** CI/CD plugins (e.g., GitHub Actions/Jenkins) for schema registration and lineage assertions; runtime sidecars/filters on Kafka and JDBC to block/flag unregistered sources.
- **Observability & Dashboards:** Metrics on lineage completeness, unauthorized source usage, freshness breaches; dashboards in Grafana/BI and alerts in PagerDuty/Slack.

## Implementation Plan (12–16 weeks)
1. **Discovery & Targeting (Weeks 1–2)**: Inventory platforms, choose pilot domains (e.g., payments + customer master), select tooling (OpenMetadata + Schema Registry + OPA), define golden certification criteria and policies.
2. **Foundation (Weeks 3–5)**: Stand up catalog/lineage service, connect to Kafka, DBT/Glue, warehouses; enable schema registry with compatibility rules; establish data contracts templates.
3. **Pilot Enforcement (Weeks 6–9)**: Add CI/CD checks for schema registration and lineage assertions; deploy runtime probes for Kafka producers/consumers; publish golden datasets list; start advisory-only risk scoring.
4. **Guardrails to Prevent (Weeks 10–12)**: Enable block/deny for non-registered schemas in non-prod; add freshness SLO monitors; rollout dashboard to engineering/risk leads.
5. **Scale & Hardening (Weeks 13–16)**: Extend to additional domains, automate certification workflow, add drift detection, finalize operating model (ownership, runbooks, audits).

## Key Deliverables
- Central catalog with automated lineage coverage >80% for pilot domains.
- Schema registry with enforced compatibility checks and CI/CD gate.
- Golden-source certification workflow and published list with owners.
- Dashboards for unauthorized source usage, freshness, and lineage completeness.
- Runbooks and policy-as-code repository (OPA/Rego) for enforcement.

## Risks & Mitigations
- **Tool sprawl/integration complexity:** Start with one catalog; use open connectors; avoid parallel pilots.  
- **Developer friction:** Start advisory, provide templates/examples, add exemptions workflow.  
- **Lineage blind spots (legacy/mainframe):** Use sampling probes, manual stubs for critical flows; prioritize replacements.  
- **False positives in blocking:** Phased rollout with canaries and bypass tokens; strong observability.

## Success Metrics
- % data products with certified golden source tag.  
- % events/tables with registered schema and lineage.  
- Unauthorized/non-authoritative consumption incidents trend.  
- Time to assess change impact (baseline vs after).  
