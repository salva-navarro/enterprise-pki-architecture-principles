# Enterprise PKI Architecture Design Principles

Author: Salva Navarro  
Role: Enterprise PKI Architect  
Location: Spain  

---

## Portfolio Context

This repository is the architecture core of my PKI portfolio.

- Website and demos: https://salva-navarro.github.io
- Spanish architecture companion: https://github.com/salva-navarro/arquitectura-pki-empresarial
- Tooling index: https://github.com/salva-navarro/Tools-Hub

The goal is practical: move from abstract guidance to implementation-ready design decisions.

---

## Executive Summary

Enterprise PKI is a **trust platform**, not a certificate factory. Its mission is to provide:

- Strong and durable cryptographic trust anchors.
- Verifiable identity binding for users, devices, workloads, and services.
- Policy-enforced certificate lifecycle controls.
- High availability for validation paths (CRL/OCSP/metadata).
- Forensic-grade auditability and compliance evidence.
- Algorithm agility for classical and post-quantum transitions.

A robust PKI architecture must survive three realities simultaneously:

1. **Compromise pressure** (credential theft, lateral movement, insider misuse).
2. **Scale pressure** (thousands to millions of certificates across hybrid estates).
3. **Change pressure** (new regulations, cloud migrations, cryptographic evolution).

---

## Table of Contents

1. [Architectural Objectives](#1-architectural-objectives)
2. [Core Design Models](#2-core-design-models)
3. [Trust Domains and PKI Segmentation](#3-trust-domains-and-pki-segmentation)
4. [Security Hardening Principles](#4-security-hardening-principles)
5. [Cryptographic Policy Framework](#5-cryptographic-policy-framework)
6. [Certificate Lifecycle Governance](#6-certificate-lifecycle-governance)
7. [Revocation and Validation Architecture](#7-revocation-and-validation-architecture)
8. [Identity and Enrollment Architecture](#8-identity-and-enrollment-architecture)
9. [Operational Model and RACI](#9-operational-model-and-raci)
10. [Monitoring, Telemetry, and SLOs](#10-monitoring-telemetry-and-slos)
11. [Business Continuity and Disaster Recovery](#11-business-continuity-and-disaster-recovery)
12. [Compliance and Audit Readiness](#12-compliance-and-audit-readiness)
13. [Zero Trust Alignment](#13-zero-trust-alignment)
14. [Cloud and Multi-Cloud PKI Patterns](#14-cloud-and-multi-cloud-pki-patterns)
15. [Post-Quantum Readiness](#15-post-quantum-readiness)
16. [Reference Certificate Profiles](#16-reference-certificate-profiles)
17. [Common Anti-Patterns](#17-common-anti-patterns)
18. [Implementation Roadmap](#18-implementation-roadmap)
19. [PKI Maturity Model](#19-pki-maturity-model)
20. [Architecture Review Checklist](#20-architecture-review-checklist)
21. [Glossary](#21-glossary)

---

## 1. Architectural Objectives

An enterprise-grade PKI must be:

- **Segmented**: blast-radius containment by design.
- **Scalable**: predictable operation under growth.
- **Cryptographically governed**: policy-driven algorithm and key controls.
- **Operationally resilient**: highly available validation and deterministic recovery.
- **Audit-ready**: complete and immutable evidence of key ceremonies and issuance actions.
- **Interoperable**: standards-aligned profiles across heterogeneous systems.
- **Automatable**: repeatable issuance/renewal/revocation flows via secure APIs.

### 1.1 Design Tenets

- Trust is **hierarchical and explicit**, never implicit.
- Keys are **assets of highest criticality**.
- Administrative convenience is always subordinate to trust assurance.
- Revocation, renewal, and rollover are first-class design concerns (not afterthoughts).
- Every certificate must map to a named owner and service purpose.

---

## 2. Core Design Models

### 2.1 2-Tier Architecture (Enterprise Baseline)

- Offline Root CA
- Online Issuing CA(s)
- HSM-backed private key storage
- Segregated publication services (CRL/AIA/OCSP)
- Strict role separation and dual control

**When to use**: most enterprises requiring strong assurance without extreme policy compartmentalization.

### 2.2 3-Tier Architecture (High-Segmentation)

- Offline Root CA
- Policy/Intermediate CA(s)
- Issuing CA(s) per trust domain

**Benefits**:

- Better isolation between business units, regions, or data sensitivity tiers.
- Easier algorithm transition pilots by intermediate.
- Reduced reissuance blast radius during compromise or migration.

### 2.3 Federated Enterprise PKI Pattern

- Corporate Root of Trust
- Regional/Domain intermediates
- Domain-local issuance policies

**Use case**: globally distributed organizations with legal sovereignty constraints.

---

## 3. Trust Domains and PKI Segmentation

Treat trust domains as architectural boundaries, not naming conventions.

### 3.1 Domain Types

- **User Identity PKI**: smart cards, user auth, S/MIME.
- **Device PKI**: endpoints, MDM-bound identities, Wi-Fi/EAP-TLS.
- **Server/Workload PKI**: TLS for data center, cloud, microservices.
- **Code Signing PKI**: build pipelines, release signing.
- **Document Signing PKI**: legal/non-repudiation workflows.
- **IoT/OT PKI**: constrained devices, long-lived deployments.

### 3.2 Segmentation Controls

- Dedicated issuing CAs per domain.
- Unique certificate templates/policies per domain.
- Distinct EKU sets and key usage constraints.
- Separate revocation and telemetry channels where risk warrants.
- Prevent cross-domain enrollment by policy and network controls.

### 3.3 Naming and OID Governance

- Maintain enterprise OID registry.
- Enforce naming standards (CN/SAN/subject attributes).
- Version policy OIDs for traceability.
- Retire OIDs explicitly; never reuse semantically.

---

## 4. Security Hardening Principles

### 4.1 Root CA Security

- Root CA permanently offline except during controlled ceremonies.
- No domain join.
- Boot media integrity controls and measured startup validation.
- Multi-person control for activation and signing operations.
- Physical isolation with tamper evidence and ceremony recording.

### 4.2 Issuing CA Security

- Hardened OS baseline with minimal services.
- HSM for CA signing keys (FIPS-aligned where required).
- Tier-0 equivalent protection.
- Protected admin workstations and privileged access management.
- Network micro-segmentation and explicit egress allow-lists.

### 4.3 Administrative Tiering

- Separation of duties:
  - PKI Policy Authority
  - CA Operations
  - HSM Custodian
  - Security Monitoring
  - Audit/Compliance
- Just-in-time administration with approval workflows.
- No shared privileged accounts.

### 4.4 Key Ceremony Discipline

- Formal runbooks.
- Witnessed and recorded execution.
- Hash-sealed ceremony artifacts.
- Signed minutes and chain-of-custody records.

---

## 5. Cryptographic Policy Framework

### 5.1 Baseline Policy Dimensions

- Approved algorithms by use case.
- Minimum key sizes.
- Signature/hash combinations.
- Certificate validity periods.
- Renewal and overlap windows.
- Key archival/recovery rules.

### 5.2 Profile Strategy

Define profiles for each certificate class:

- Server TLS internal
- Server TLS internet-facing
- Mutual TLS client cert
- User auth cert
- Code signing cert
- OCSP responder cert
- CA cert (root/intermediate/issuing)

### 5.3 Algorithm Agility Controls

- Cryptographic inventory with owner mapping.
- “No hard-coded algorithm” standards in app teams.
- Controlled pilots for new suites.
- Sunset calendar for legacy primitives.

---

## 6. Certificate Lifecycle Governance

### 6.1 Issuance Policy

Every issuance path must define:

- Who may request.
- How identity is proven.
- Which profile may be issued.
- Approval requirements.
- Maximum validity.
- Logging and retention obligations.

### 6.2 Renewal Strategy

- Prefer automated renewal for machine identities.
- Enforce overlap windows to avoid outages.
- Block silent profile drift at renewal.
- Alert on renewal failures before expiry thresholds.

### 6.3 Revocation Governance

- Define revocation SLAs by criticality class.
- Immediate revocation for key compromise/high-risk departures.
- Deterministic revocation workflows with incident integration.

### 6.4 Ownership and Inventory

Maintain authoritative inventory for each cert:

- Certificate fingerprint/serial.
- Owner (team + accountable person).
- System/workload mapping.
- Issuance profile and policy OID.
- Expiry and renewal state.

---

## 7. Revocation and Validation Architecture

### 7.1 CRL Design

- Partition CRLs when scale requires.
- Publish delta CRLs where supported.
- Use redundant distribution points (internal/external if needed).
- Define CRL freshness and publication SLOs.

### 7.2 OCSP Design

- Stateless responders behind load balancers.
- Signed OCSP responses with dedicated responder certs.
- Capacity planning for peak handshake events.
- Response caching tuned to security/performance balance.

### 7.3 AIA/CDP Reliability

- Highly available publication endpoints.
- DNS and certificate pinning strategy for validation services.
- Continuous synthetic monitoring from representative network zones.

---

## 8. Identity and Enrollment Architecture

### 8.1 Enrollment Channels

- AD-integrated enrollment (when relevant).
- SCEP/EST for managed devices.
- ACME for workload and platform automation.
- API-based enrollment for controlled service identities.

### 8.2 Identity Proofing

- Bind enrollment to authoritative identity systems.
- Enforce attestation for device/workload issuance where feasible.
- Require stronger proofing for high-privilege templates.

### 8.3 Template/Profile Security

- Least privilege on enrollment rights.
- Manager approval for sensitive templates.
- Strong issuance requirements (subject/SAN restrictions).
- Continuous review of template ACL drift.

---

## 9. Operational Model and RACI

### 9.1 Recommended Roles

- **PKI Service Owner**: strategy, risk acceptance, roadmap.
- **PKI Architect**: design authority and standards.
- **PKI Operations**: run, monitor, maintain service.
- **HSM Custodians**: key material custody and ceremony operations.
- **Security Engineering**: detection, threat modeling, controls.
- **Compliance/Internal Audit**: evidence validation and control testing.

### 9.2 RACI Baseline

- Policy updates: Architect (R), Service Owner (A), Security (C), Audit (I)
- Key ceremonies: Custodian/Ops (R), Service Owner (A), Audit (C)
- Template changes: Ops (R), Architect (A), Security (C)
- Incident revocation: Ops/Sec (R), Service Owner (A), App Owner (C)

---

## 10. Monitoring, Telemetry, and SLOs

### 10.1 Mandatory Observability

- CA operational logs (issuance/revocation/config changes).
- HSM event logs.
- Enrollment endpoint logs.
- CRL/OCSP publication and response metrics.
- Failed authentication and privilege escalation events.

### 10.2 Core KPIs/SLOs

- Certificate issuance latency by profile.
- Renewal success rate.
- Revocation execution time vs SLA.
- OCSP response time and error rate.
- CRL publication timeliness.
- % certificates with known owner.
- % machine certificates auto-renewed.

### 10.3 Security Analytics

- Detect anomalous issuance bursts.
- Detect unusual SAN patterns.
- Detect privileged access outside approved windows.
- Correlate revocations with incident records.

---

## 11. Business Continuity and Disaster Recovery

### 11.1 Backup Strategy

- Encrypted backups of CA databases/configuration.
- HSM backup procedures (vendor-validated).
- Offsite secure storage with custody controls.

### 11.2 Recovery Objectives

- Define RTO/RPO per PKI component:
  - Issuing CA
  - OCSP services
  - CRL publication
  - Enrollment gateways

### 11.3 Recovery Testing

- Conduct periodic restore drills.
- Validate end-to-end issuance after restore.
- Validate revocation and publication correctness post-recovery.

### 11.4 Compromise Recovery

Document and test procedures for:

- Issuing CA compromise.
- Key compromise (signing key or responder key).
- Emergency CRL acceleration.
- Mass reissuance campaigns.

---

## 12. Compliance and Audit Readiness

### 12.1 Control Evidence Model

Collect durable evidence for:

- Key generation and storage controls.
- Access control and SoD enforcement.
- Change management approvals.
- Incident and revocation handling.
- Periodic access and policy reviews.

### 12.2 Documentation Stack

- CP/CPS or equivalent policy corpus.
- PKI standards and profile catalogue.
- Key ceremony runbooks.
- Operational procedures and escalation matrix.
- Exception register with risk acceptance.

### 12.3 Audit Facilitation

- Pre-map controls to applicable frameworks.
- Maintain evidence indexes by control ID.
- Time-bound remediation tracking.

---

## 13. Zero Trust Alignment

PKI should strengthen, not bypass, Zero Trust.

### 13.1 Principles

- Certificate possession is necessary, not sufficient.
- Combine cert auth with device posture, user risk, and policy engine decisions.
- Enforce continuous verification for long-lived sessions.

### 13.2 mTLS Governance

- Standardize trust bundles by environment.
- Enforce SAN and SPIFFE-like identity discipline where applicable.
- Rotate service identities automatically.
- Prevent unmanaged private key sprawl in CI/CD and containers.

---

## 14. Cloud and Multi-Cloud PKI Patterns

### 14.1 Hybrid Trust Model

- Keep enterprise root offline and enterprise-controlled.
- Delegate issuance to cloud-integrated intermediates when justified.
- Maintain consistent policy OIDs and profile semantics across environments.

### 14.2 Secrets and Key Management Integration

- Integrate with cloud KMS/HSM services carefully.
- Validate tenancy boundaries and operational ownership.
- Enforce export restrictions for sensitive keys.

### 14.3 Kubernetes and Service Mesh Considerations

- Avoid unmanaged, ad hoc in-cluster CAs for critical workloads.
- Define trust domain mapping between mesh identities and enterprise PKI.
- Govern certificate lifetime and rotation frequency to balance risk and load.

---

## 15. Post-Quantum Readiness

### 15.1 Why Prepare Now

- Long-lived trust artifacts created today may be vulnerable tomorrow.
- “Harvest now, decrypt later” impacts sensitive data with long confidentiality horizons.

### 15.2 Readiness Program

1. Build cryptographic asset inventory.
2. Classify by lifetime and sensitivity.
3. Identify algorithm dependencies in applications/protocols.
4. Pilot hybrid certificate/signature approaches where ecosystem support exists.
5. Track standards/vendor roadmaps and interoperability milestones.

### 15.3 Transition Design Principles

- Prefer crypto-agile protocol stacks.
- Decouple certificate profile logic from fixed primitives.
- Plan phased migration by risk domain, not big-bang replacement.

---

## 16. Reference Certificate Profiles

> Exact values depend on enterprise policy and compatibility needs.

### 16.1 Internal TLS Server (Example)

- Subject/SAN: DNS names only from approved zones.
- EKU: Server Authentication.
- Key usage: Digital Signature, Key Encipherment (or key agreement depending stack).
- Validity: short-to-medium (e.g., 90–397 days by policy).

### 16.2 Workload mTLS Client (Example)

- SAN: URI/SPIFFE-like identity or workload ID.
- EKU: Client Authentication.
- Validity: short-lived with automated renewal.
- Issuance: attested identity source only.

### 16.3 Code Signing (Example)

- Strong identity vetting for requestors.
- Hardware-protected signing key requirement.
- Optional dual-signing strategy during algorithm transitions.

---

## 17. Common Anti-Patterns

- Online root CA for convenience.
- Shared admin credentials for PKI operations.
- Single issuing CA for every identity class.
- “Set and forget” templates with no periodic review.
- Revocation endpoints treated as best effort.
- Unknown certificate ownership.
- Manual-only renewal for machine certificates at scale.
- Hard-coded cryptographic assumptions in applications.

---

## 18. Implementation Roadmap

### Phase 0 — Discovery

- Asset and dependency inventory.
- Risk and regulatory mapping.
- Current-state architecture and gap analysis.

### Phase 1 — Foundation

- Define target PKI architecture.
- Establish policy corpus (CP/CPS/standards).
- Build root and issuing tiers with hardening baseline.

### Phase 2 — Operationalization

- Establish enrollment automation channels.
- Implement observability and SLOs.
- Deploy inventory and ownership workflows.

### Phase 3 — Scale and Governance

- Enforce segmentation per trust domain.
- Integrate Zero Trust policy engines.
- Institutionalize audit evidence lifecycle.

### Phase 4 — Evolution

- Crypto-agility exercises.
- PQ-readiness pilots.
- Continuous architecture review and modernization.

---

## 19. PKI Maturity Model

### Level 1 — Foundational

- Basic CA hierarchy exists.
- Limited policy standardization.
- Predominantly manual operations.

### Level 2 — Controlled

- Defined certificate profiles.
- Documented ceremonies and procedures.
- Core monitoring and periodic reviews.

### Level 3 — Governed

- Strong segmentation and SoD enforcement.
- Automated renewal at meaningful scale.
- Measured SLOs and audit-ready evidence model.

### Level 4 — Adaptive

- Full lifecycle automation with guardrails.
- Advanced detection analytics.
- Tested compromise and migration playbooks.

### Level 5 — Strategic

- PKI deeply integrated with enterprise identity and Zero Trust.
- Crypto-agility and PQ transition program operationalized.
- Continuous improvement loop tied to business risk.

---

## 20. Architecture Review Checklist

Use this list in design boards and periodic assurance reviews:

- [ ] Is the root CA offline and operationally controlled?
- [ ] Are issuing CAs segmented by trust domain and risk?
- [ ] Are HSM controls aligned with policy and regulations?
- [ ] Are enrollment rights least-privilege and regularly reviewed?
- [ ] Is certificate ownership complete and current?
- [ ] Are renewal paths automated for machine identities?
- [ ] Are revocation SLAs defined and measured?
- [ ] Are CRL/OCSP endpoints resilient and monitored?
- [ ] Are template/profile changes controlled and audited?
- [ ] Are recovery and compromise drills executed periodically?
- [ ] Is algorithm agility documented and tested?
- [ ] Is there a PQ-readiness roadmap with milestones?

---

## 21. Glossary

- **AIA**: Authority Information Access.
- **CA**: Certificate Authority.
- **CDP**: CRL Distribution Point.
- **CP/CPS**: Certificate Policy / Certification Practice Statement.
- **CRL**: Certificate Revocation List.
- **EKU**: Extended Key Usage.
- **HSM**: Hardware Security Module.
- **OCSP**: Online Certificate Status Protocol.
- **OID**: Object Identifier.
- **PKI**: Public Key Infrastructure.
- **RACI**: Responsible, Accountable, Consulted, Informed.
- **RPO/RTO**: Recovery Point/Time Objective.
- **SLO**: Service Level Objective.
- **SoD**: Separation of Duties.
- **Zero Trust**: Security model based on continuous verification and least privilege.

---

## Positioning Statement

This repository focuses on **architectural design patterns**, **operating principles**, and **enterprise-grade PKI governance**. It intentionally avoids generic cybersecurity commentary and prioritizes practical, scalable trust architecture guidance.

Sitio oficial: https://salva-navarro.github.io
