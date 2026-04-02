# PKI Operating Model

## Objective

Translate architecture principles into repeatable day-2 PKI operations.

## Roles

- PKI Architecture Lead: owns target model and standards.
- PKI Operations: executes lifecycle workflows and controls.
- Security Monitoring: validates telemetry and anomaly response.
- Compliance and Audit: verifies evidence and policy adherence.

## Core Run Cycles

1. Daily: issuance/revocation anomalies, failed renewals, OCSP/CRL health.
2. Weekly: expiring certificate review and owner follow-up.
3. Monthly: template drift review and policy exceptions cleanup.
4. Quarterly: key material and crypto baseline review.

## Decision Gates

- No new profile without owner + lifecycle + telemetry mapping.
- No issuance policy changes without rollback plan.
- No production rollout without validation SLO checks.

## Operating KPIs

- % certificates with accountable owner.
- % certificates auto-renewed before threshold.
- Revocation SLA compliance.
- Validation endpoint availability (CRL/OCSP).
