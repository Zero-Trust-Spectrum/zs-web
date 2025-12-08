# Zero-Trust Spectrum — Files

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.17573363.svg)](https://doi.org/10.5281/zenodo.17573363)


**Framework defining measurable Zero-Trust assurance levels for file content (ZS-1 → ZS-6).**  
Aligned with **CyBOK · NCSC · NIST SP-800-207** · Licensed under **Creative Commons Attribution 4.0 International (CC BY 4.0)**.

---

## Overview
The **Zero-Trust Spectrum (ZTS)** provides a reference model to classify and evaluate **file assurance levels** within Zero-Trust architectures.

It defines six measurable levels (ZS-1 to ZS-6) describing how files are validated, sanitized, or rebuilt before use — from basic hygiene to deterministic reconstruction.

This repository maintains the open specification, supporting materials, and reference one-pager for implementers, auditors, and architects.

---

## Scope
Applicable to:
- Email and collaboration platforms (M365, Google Workspace)
- Web/SWG/Proxy (ICAP, reverse-proxy)
- APIs and SDK integrations
- Storage and archival remediation
- Kiosk, air-gap, and cross-domain deployments
- Automation and SOAR/DLP/SIEM workflows
- Flat-file export/import pipelines

---

## Spectrum Levels
| Level | Mechanism | Assurance Summary |
|-------|-----------|-------------------|
| **ZS-1** | Pass-through | Implicit trust; no verification |
| **ZS-2** | Detection | Signature/hash-based identification of known threats |
| **ZS-3** | Isolation / Flattening | Behavioural containment / read-only rendering |
| **ZS-4** | Sanitisation (DDR) | Rule-based removal or allow-only copying |
| **ZS-5** | Transform to templates | Software rebuild from known-good models |
| **ZS-6** | Deterministic rebuild | Clean intermediary model; no source bytes retained; spec-level validation with audit evidence |

For formal definitions and criteria, see the [Zero-Trust Spectrum (Files) reference](https://doi.org/10.5281/zenodo.17573363).

---

## Alignment
The Zero-Trust Spectrum aligns conceptually with:
- [NCSC Zero-Trust Architecture Guidance](https://www.ncsc.gov.uk/collection/zero-trust-architecture)  
- [NIST SP-800-207 Zero Trust Architecture](https://csrc.nist.gov/publications/detail/sp/800-207/final)  
- [CyBOK – Cyber Security Body of Knowledge](https://www.cybok.org/)

See `alignment.md` for signal mappings to each framework.

---

## License
This work is licensed under the **Creative Commons Attribution 4.0 International (CC BY 4.0)** license.  
You may copy, adapt, and redistribute with attribution. See [`LICENSE.md`](LICENSE.md).

---

## Contributing
Feedback and corrections are welcome.  
Open an [issue](../../issues) or email the maintainers at **editor@zerotrustspectrum.org**.  
All contributions are released under CC BY 4.0.

---

## Citation
If citing in research or policy:
> Zero-Trust Spectrum — Files (ZS-1 → ZS-6), version 2025-11-10.  
> DOI: [10.5281/zenodo.17573363](https://doi.org/10.5281/zenodo.17573363)

---

## Attribution
Zero-Trust Spectrum © 2025 · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)  
An open reference for measurable file assurance within Zero-Trust architectures.  
Maintained by independent contributors.

---

Maintained by independent contributors · ORCID: [0009-0003-9242-4790](https://orcid.org/0009-0003-9242-4790)
