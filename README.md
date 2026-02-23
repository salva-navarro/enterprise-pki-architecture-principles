# Enterprise PKI Architecture Design Principles

Author: Salva Navarro  
Role: Enterprise PKI Architect  
Location: Spain  

---

## 1. Architectural Objectives

An enterprise-grade Public Key Infrastructure must be:

- Segmented
- Scalable
- Cryptographically governed
- Operationally resilient
- Audit-ready

PKI is not a certificate factory. It is a trust infrastructure.

---

## 2. Core Design Model

### 2-Tier Architecture (Baseline)

- Offline Root CA
- Online Issuing CA
- Dedicated HSM-backed key storage
- Controlled CRL distribution
- Strict role separation

---

## 3. Security Hardening Principles

- Root CA permanently offline
- Issuing CA isolated from domain compromise
- Administrative tiering model
- Dedicated PKI management forest (when required)
- Restricted enrollment endpoints

---

## 4. Certificate Lifecycle Governance

- Defined issuance policies
- Automated renewal strategies
- Revocation SLAs
- CRL/OCSP performance planning
- Inventory visibility

---

## 5. Zero Trust Alignment

- PKI must integrate with identity posture
- Certificates must not bypass policy enforcement
- Mutual TLS strategy must be governed

---

## 6. Post-Quantum Readiness

- Cryptographic inventory baseline
- Algorithm agility planning
- Vendor capability assessment
- Transition roadmap definition

---

This repository focuses purely on architectural design patterns and enterprise-grade PKI thinking.

No generic cybersecurity commentary.
