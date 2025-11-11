# Zero Trust Spectrum — File Assurance One-Pager

Version 1.1.1 · Published 2025-11-10

## Audience and Purpose
- **Who this is for:** CISOs, Zero-Trust program owners, security engineers, platform/product teams, delivery partners.
- **Use it for:** Selecting the minimum file-assurance level that meets risk, usability, and compliance requirements.

## Mission Snapshot
- **Purpose:** Provide a declarative framework for classifying file assurance within Zero-Trust architectures.
- **Scope:** Email & collaboration, Web/SWG/ICAP, APIs, storage/archival, kiosks/air-gapped CDS, SDK/embedded, file import/export.
- **Alignment:** CyBOK, NCSC Zero-Trust Architecture Guidance, NIST SP 800-207.

## Rating Scale & Dimensions
Each ZS level is scored 0 → 4 across six dimensions.

| Score | Definition |
|-------|------------|
| 0 | None / Heuristic — detection only |
| 1 | Basic — coarse rules/feature stripping |
| 2 | Moderate — isolation/flattening; limited spec awareness |
| 3 | Strong — allow-only or spec-aware validation/repair |
| 4 | Deterministic — model-driven rebuild with reproducible outputs |

**Dimensions:** mechanism rigor · assurance evidence · usability fidelity · validation test rigor · auditability · threat coverage.

## Flat-file Export / Import
Flat-file export/import (CSV, XML, JSON, TXT, PDF/A) is an essential control boundary. It ensures deterministic separation between untrusted sources and clean destinations, supports air-gap transfers, audit trails, and deterministic rebuild from verified data structures.

## Decision Quick Picks
- **Baseline:** Target ZS-6 wherever formats are supported and service objectives are met. Use ZS-5 only as a documented fallback for unsupported formats/SLO exceptions. Use ZS-3 isolation for executables and unstructured binaries.
- **Operational note:** Modern ZS-6 rebuild engines routinely deliver sub-100 ms latency at scale, enabling deterministic rebuild as the default for mainstream collaboration platforms.

| Workflow | Target | Fallback | Notes |
|----------|--------|----------|-------|
| Supplier/customer uploads & data rooms | ZS-6 | ZS-5 (unsupported) | Deterministic outputs + audit trail; flat-file handoffs supported. |
| Exec / finance / legal attachments | ZS-6 | ZS-5 | Sensitive content needs evidencing + fidelity. |
| General collaboration (M365/Workspace, SharePoint/Drive) | ZS-6 | ZS-5 (unsupported/custom) | Deterministic rebuild preserves usability; ZS-3 only for non-rebuildable binaries/archives. |
| Web downloads & SWG | ZS-6 | ZS-5 | Unknown binaries/installers default to ZS-3 isolation. |
| Cross-domain / air-gap / OT/classified | ZS-6 | ZS-3 | Transfer path uses ZS-6 + reporting; review path via ZS-3 preview. |
| Storage & archival remediation | ZS-6 | ZS-5 | Flat-file export mode for governance/retention. |
| Automation & integration pipelines (API/SDK) | ZS-6 | ZS-5 | Require flat-file interchange (XML/JSON schemas) for downstream analytics, ETL, SIEM/DLP. |
| Developer tools & installers | ZS-5 | ZS-3 | Docs/packages rebuilt; executables isolated with repo/code-sign controls. |

## Zero Trust Spectrum Levels (ZS-1 → ZS-6)
- **ZS-1 — Pass-through / Uninspected:** AV/reputation only; maximum usability, minimal assurance.
- **ZS-2 — Rule-based Sanitization:** Policy strips risky features; structure largely unchanged; suitability limited by rule coverage.
- **ZS-3 — Flatten / Isolate:** Static rendering or remote isolation; strong containment with read-only experience.
- **ZS-4 — Allow-only (DDR):** Positive selection copies only allow-listed structures into safe templates; high usability for modeled features.
- **ZS-5 — Deep Parsing & Repair:** Spec-aware parsing, validation, repair, and sanitization; high fidelity with format-aware logging.
- **ZS-6 — True Rebuild (Safe Blueprint):** Model-driven rebuild produces new spec-compliant files with no unsafe bytes; supports neutral flat-file export/import.

Each tier specifies handling for encrypted payloads, nested archives, macros, scripts, active content, media, metadata, and unsupported formats.

## Evaluation Cues
- Per-format handling (pass, sanitize, isolate, repair, rebuild) including encrypted/nested cases.
- Evidence: validation traces, policy actions, forensic logging, SIEM/SOAR integration.
- Coverage: connectors for email, ICAP/SWG, APIs/SDK, storage/EDR, kiosk, air-gapped/CDS workflows.
- Operations: throughput, latency, offline cadence, update hygiene, failure-handling playbooks.
- Governance: KPIs, assurance statements, validation testing for each tier adoption.

## Evaluation Criteria
**Functional:** Mechanism visibility, encrypted/password handling, nested archive support, flat structured export/import, deterministic rebuild from validated flat data, per-format policy depth.

**Governance & Interop:** Policy versioning, tamper-evident logs, DLP/SOAR/SIEM integration, export of sanitized/rebuilt outputs (incl. flat files) for audit/cross-domain handoffs, open standards and reproducible SLO reporting.

## Integration Patterns
- **Core flows:** Email/Collaboration (M365, Google Workspace, SEG, SMTP, Graph/Drive APIs); Web/SWG/Proxy (ICAP, reverse-proxy, WAF); Storage/Archive (S3, NAS, SharePoint, Azure Blob); Application/SDK/API (sync/async, zero-trust gateways).
- **Operational/isolated:** Kiosk/air-gap/CDS (flat-file transfer); OT/industrial/classified networks (directional policies + deterministic enforcement).
- **Automation ecosystem:** SOAR/DLP/SIEM/UEBA, EDR/XDR, Email Security, CASB, data governance/compliance/audit telemetry.
- **Developer enablement:** CLI/container/K8s operators, API gateways/ESB/workflow engines, flat-file interchange for analytics/ETL/SIEM/DLP ingestion.

## File Coverage Reality Check
- Maintain a format register by family (Office, PDF, images, archives, CAD/media, text/flat, custom).
- Document per-format ZS level, limitations, nested content handling, password/encrypted behavior, max size/complexity, fallback policy (e.g., unsupported → ZS-3 isolate/block).
- Remember breadth ≠ depth: structural enforcement quality matters more than raw format count.

## Validation Tests & Signals
- **Dataset:** macros/OLE, PDF JS/launch, nested archives (>5), polyglots, malformed EXIF/media metadata, passworded samples, malformed containers.
- **Expected behavior:**
  - ZS-3 → enforced isolation/static outputs.
  - ZS-4/5 → per-element logs for copied/repaired/removed content + explicit exceptions.
  - ZS-6 → no source-byte carryover, validator report, provenance with input/output hashes and policy IDs.
- **Signals:** outcome code, reason, elements copied/removed/repaired, validator findings, archive tree, hashes, policy version, latency/error stats.

## Guardrails & Defaults
- **Encrypted/passworded:** Block or decrypt in trusted zone; never re-emit original bytes.
- **Archives/nesting:** Recurse with safe depth/size limits; rebuild deterministically; detect polyglots/MIME mismatches.
- **Unknown/unsupported:** Default to block or ZS-3 isolate; time-bound exceptions with telemetry.
- **Signed content:** Validate before processing; re-sign outputs in trusted zone when required.

## Implementation Moments that Matter
- **Integration surface:** ICAP, REST, event-driven, inline/asynchronous, SDK kits (M365 Graph, SharePoint, Google Drive, zero-trust gateways).
- **Assurance proof:** Signed manifests, replayable logs, policy versioning, attestation packages per submission.
- **Human factors:** Release UX, exception handling, user messaging, governance dashboards.
- **PoV & SLO discipline:** Measure p95/p99 latency & throughput per format/level, track fidelity, error handling, fallback behavior, publish SLO matrix by flow (email/web/storage/API).

## Alignment Signals
- **CyBOK:** Isolation and least-common-mechanism, malware/obfuscation coverage, client-side risk mitigation, formal assurance realism, forensic readiness, SDL practices.
- **NCSC:** Discrete policy enforcement stages, explicit verification (“never trust, always verify”), least privilege data flow, assume-breach stance, continuous assurance, composability with layered controls.
- **NIST SP 800-207:** Content-centric PEP mapping, dynamic trust evaluation, continuous diagnostics & mitigation, separation of control/data planes, least-privilege workflows, evidence-driven architecture.

## Glossary Highlights
- **Content Disarm & Reconstruction (CDR):** Deterministic extraction/rebuild of file content ([Wikipedia](https://en.wikipedia.org/wiki/Content_disarm_and_reconstruction)).
- **Positive Selection / DDR:** Copy only allow-listed elements into safe templates (ZS-4).
- **Flat-file export/import:** Neutral formats (CSV/XML/JSON/TXT/PDF-A) used for cross-domain transfer, archival, deterministic re-ingestion.
- **Policy Enforcement Point (PEP):** Control plane mediating data before trust decisions (per NIST SP 800-207).
- **Deterministic rebuild:** ZS-6 capability to emit new spec-compliant files with provenance.
- **Validation test harness:** Repeatable dataset + telemetry capture for independent verification.

## References
1. [NIST SP 800-207 – Zero Trust Architecture (Dec 2020)](https://csrc.nist.gov/publications/detail/sp/800-207/final)
2. [NCSC Zero-Trust Architecture Collection](https://www.ncsc.gov.uk/collection/zero-trust-architecture)
3. [CyBOK – Cyber Security Body of Knowledge](https://www.cybok.org/)
4. [Creative Commons BY 4.0 License](https://creativecommons.org/licenses/by/4.0/)

## Contact
Questions or suggestions? Contact the maintainers: [editor@zerotrustspectrum.org](mailto:editor@zerotrustspectrum.org)
