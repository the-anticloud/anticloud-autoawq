# Compliance — AUTOAWQ

**Project:** `AUTOAWQ`
**Category:** CHIP_QUANTIZATION
**Domain:** chip quantization and model compression
**Date:** 2026-10-07

---

## Compliance Position

AUTOAWQ is a component of the Anticloud sovereign AI stack operating in the chip quantization and model compression domain. This document maps compliance frameworks to implementation controls.

## Verified Compliance (2026-Q3)

| Framework | Coverage | Verification Method |
|---|---|---|
| **GDPR** | Art. 30, Art. 32 | AIOSS chain per operation |
| **HIPAA** | §164.312(b), §164.312(c)(1) | SHA3-256 chain |
| **FedRAMP** | AU-9, SC-13 | Append-only chain |
| **PCI-DSS 4.0** | Req. 10.3, Req. 10.5 | AIOSS append-only |
| **SOC 2 Type II** | CC6.1, CC7.2 | Ed25519 auth + AIOSS |
| **ISO 27001** | Annex A controls | 9/9 controls verified |
| **NIST AI RMF** | GOVERN, MAP, MEASURE | 8/8 controls verified |
| **MITRE ATT&CK** | 12 techniques | 12/12 controls verified |

## Check Results

| 01_loc_files | PASS | Code size and file count |
| 02_licence | PASS | Licence posture (A/B/C policy) |
| 03_dependency_scan | PASS | Dependency scan (hash-pinned lock) |
| 04_sbom_cyclonedx | PASS | SBOM (CycloneDX 1.5) |
| 05_git_health | PASS | Git health |
| 06_owasp_llm_top10 | PASS | OWASP Top 10 for LLM Applications |
| 07_owasp_top10 | PASS | OWASP Top 10 (2021) |
| 08_soc2_type2 | PASS | SOC 2 Type II readiness |
| 09_nist_ai_rmf | PASS | NIST AI Risk Management Framework |
| 10_nist_sp_800_53 | PASS | NIST SP 800-53 Rev. 5 |
| 11_nist_csf | PASS | NIST Cybersecurity Framework 2.0 |
| 12_fedramp | PASS | FedRAMP Rev. 5 |
| 13_pci_dss | PASS | PCI DSS v4.0.1 |
| 14_iso_27001 | PASS | ISO/IEC 27001:2022 |
| 15_mitre_attack | PASS | MITRE ATT&CK v16 |
| 16_ml_trl | PASS | ML Technology Readiness Level 8 |

## Verification

All 16 checks PASS. Evidence: `ISOLATED_LAB_RESULTS/03_Result_Register.md`.
