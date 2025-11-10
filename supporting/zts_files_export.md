{
  "title": "Zero-Trust Spectrum (ZS-1 → ZS-6) — Files",
  "version_date": "2025-11-10",
  "purpose": "A practical framework for classifying file assurance levels within a Zero-Trust architecture.",
  "scope": [
    "Email and collaboration (M365, Google Workspace, Notes)",
    "Web gateways, SWG, proxy, and ICAP flows (upload/download)",
    "API gateways and application middleware",
    "File transfer systems and content services",
    "Kiosks, field collection systems, and document import/export",
    "Air-gapped and cross-domain networks",
    "Storage and archive platforms (NAS, S3, DLP connectors)",
    "Deep integrations via SDK, API, or plug-in",
    "Export/import workflows to flat files (CSV, XML, JSON, text, archives)"
  ],
  "levels": [
    {
      "level": "ZS-1",
      "name": "Pass-through / Uninspected",
      "mechanism": "Allow or scan-only; no structural enforcement",
      "assurance": "Minimal; relies on AV or reputation",
      "usability": "Full",
      "buyer_test": "Send malformed/polyglot samples; verify unchanged delivery and minimal logging.",
      "examples": [
        "Basic pass lists",
        "AV-only mail flow"
      ],
      "threats_mitigated": [
        "Known malware via AV signatures/reputation (limited)."
      ],
      "residual_risks": [
        "Zero-day format exploits; active content executes.",
        "Polyglots and format confusion bypass.",
        "No structural enforcement; relies on detection efficacy."
      ],
      "ideal_use_cases": [
        "Low-risk internal transfers where upstream controls already enforce higher assurance.",
        "Temporary exception paths with compensating monitoring."
      ],
      "signals_expected": [
        "Basic AV verdicts; minimal telemetry."
      ]
    },
    {
      "level": "ZS-2",
      "name": "Rule-based Sanitization",
      "mechanism": "Strip/deactivate risky features (macros, scripts, links, embedded objects)",
      "assurance": "Policy-based; structure remains largely unchanged",
      "usability": "Mostly preserved; some features removed",
      "buyer_test": "Seed macros/scripts/links/OLE; verify removal/neutralisation with explicit reason codes.",
      "examples": [
        "Macro removal",
        "Metadata cleansing"
      ],
      "threats_mitigated": [
        "Macro/script auto-execution.",
        "Basic embedded object risks.",
        "Simple link-based attacks (if rewritten or removed)."
      ],
      "residual_risks": [
        "Spec ambiguities and parser differentials.",
        "Evasive active content via obscure features.",
        "No guarantees about structural correctness."
      ],
      "ideal_use_cases": [
        "Legacy environments where editability is favored and threat model is moderate."
      ],
      "signals_expected": [
        "Lists of removed features; policy hits; before/after counts."
      ]
    },
    {
      "level": "ZS-3",
      "name": "Flatten / Isolate",
      "mechanism": "Render to static (PDF/image) and/or interact via isolation (RBI/container).",
      "assurance": "Strong containment; source bytes may still reach endpoints if isolation is bypassable; no active content in flattened outputs.",
      "usability": "Reduced (read-only/static)",
      "buyer_test": "Open via isolation and request flattened export; confirm static output and no active features.",
      "examples": [
        "PDF rendering",
        "Sandboxed viewers"
      ],
      "threats_mitigated": [
        "RCE via active content during viewing.",
        "Drive-by downloads when isolation strips delivery."
      ],
      "residual_risks": [
        "Business processes requiring editability impacted.",
        "If isolation streams original bytes, egress of unsafe content remains possible."
      ],
      "ideal_use_cases": [
        "Web browsing and unknown file types where read-only is acceptable.",
        "Triage review before allowing downstream processing."
      ],
      "signals_expected": [
        "Rendering mode, isolation policy, evidence that no editable features remain."
      ]
    },
    {
      "level": "ZS-4",
      "name": "Allow-Only / Positive Selection (DDR)",
      "mechanism": "Dissect file; copy allowed elements into safe templates; drop disallowed",
      "assurance": "Moderate to high; depends on allow-lists/templates",
      "usability": "High for allowed features",
      "buyer_test": "Inject allowed and disallowed elements; verify only allow‑listed items carry into the output template with element‑level logs.",
      "examples": [
        "Template-based reconstruction",
        "Whitelist copy"
      ],
      "threats_mitigated": [
        "Unknown/novel active content by omitting everything not explicitly allowed.",
        "Common embedded object risks when not on the allow-list."
      ],
      "residual_risks": [
        "Template/allow-list gaps can permit risky constructs.",
        "Some source bytes may carry over if copy-based implementation.",
        "Coverage varies per format."
      ],
      "ideal_use_cases": [
        "Business workflows needing high usability for common features with strong safety for what is modeled."
      ],
      "signals_expected": [
        "Elements copied vs. dropped, template ID, policy version, output hash."
      ]
    },
    {
      "level": "ZS-5",
      "name": "Deep Parsing + Validate/Repair & Sanitize",
      "mechanism": "Parse to spec; repair or sanitize elements; some formats partially reconstructed",
      "assurance": "High; format-aware; varies by file type/policy",
      "usability": "High; function preserved",
      "buyer_test": "Provide spec‑deviant files; verify repair/sanitise actions per element with conformance logs.",
      "examples": [
        "Spec-aware sanitization",
        "Repair routines"
      ],
      "threats_mitigated": [
        "Malformed structures and spec deviations.",
        "Many embedded threats via repair/sanitize.",
        "Archive recursion/resource-abuse through bounded parsing."
      ],
      "residual_risks": [
        "Partial carryover of source bytes for repaired elements.",
        "Unknown/unsupported features may be sanitized in place rather than rebuilt."
      ],
      "ideal_use_cases": [
        "Default for office/PDF-heavy collaboration where editability is required."
      ],
      "signals_expected": [
        "Per-object validation/repair logs, spec conformance checks, unsupported feature counters."
      ]
    },
    {
      "level": "ZS-6",
      "name": "True Rebuild (Safe Blueprint)",
      "mechanism": "Build a clean intermediary model of the file; write a new file to spec (no unsafe source bytes carried over)",
      "assurance": "Highest; deterministic; non-conformant items excluded/normalized; per-file analysis available",
      "usability": "Full (no flattening)",
      "buyer_test": "Diff input/output bytes; expect no carryover. Validate output with strict validators and provenance report.",
      "examples": [
        "Deterministic rebuild",
        "Structural regeneration"
      ],
      "threats_mitigated": [
        "Spec-level exploits, active content, and parser differential attacks via clean-model rewrite.",
        "Polyglot/type confusion by normalizing and re-emitting from a canonical model."
      ],
      "residual_risks": [
        "Unsupported formats fall back per policy (block or lower level).",
        "Business processes relying on byte-identical artifacts (e.g., digital signatures) require re-signing."
      ],
      "ideal_use_cases": [
        "Highest-risk ingress/egress points, air gaps, supplier/customer exchange, data rooms."
      ],
      "signals_expected": [
        "Proof of no source byte carryover (source vs. output hash class), spec validator outputs, model build ID."
      ]
    }
  ],
  "file_coverage_considerations": [
    "Different solutions vary in breadth (number of types) and depth (quality per type).",
    "Breadth does not guarantee assurance—evaluate structural enforcement depth for each format.",
    "Check handling for nested content (archives, embedded files) and encrypted/password-protected files.",
    "Confirm behavior per format: detect-only, sanitize, rebuild, bypass/block."
  ],
  "evaluation_criteria": {
    "functional": [
      "Mechanism type and process visibility (pass, sanitize, rebuild)",
      "Encrypted/password-protected handling",
      "Support for nested/archived content",
      "Import/export capability to flat or neutral formats",
      "Policy configuration depth (per format/feature)"
    ],
    "assurance_reporting": [
      "Validation or conformance logs",
      "Evidence of repair, removal, or rebuild actions",
      "Unsupported content handling (block/bypass/transform)",
      "Reporting/telemetry: API, syslog, SIEM export"
    ],
    "deployment_integration": [
      "Email, web/ICAP, API, SDK, storage, air-gapped, kiosk, or application plug-in options",
      "Scaling characteristics (throughput, concurrency, resilience)",
      "Update/patch mechanisms; offline operation support"
    ],
    "governance_interop": [
      "Policy versioning and audit trails",
      "DLP/SOAR/SIEM integration",
      "Export of sanitized/rebuilt outputs to safe zones",
      "Open standards and cross-vendor compatibility"
    ],
    "security_engineering": [
      "Language/runtime safety of parsers (memory‑safe implementations, fuzzing coverage).",
      "Supply‑chain posture (SBOM, reproducible builds, CVE response SLAs).",
      "Anti‑evasion coverage (polyglots, ambiguous encodings, bidi/RTL tricks, content‑type spoofing).",
      "Resilience to resource abuse (ZIP bombs, decompression limits, streaming safeguards)."
    ],
    "operations_kpi": [
      "Throughput (docs/sec) at N concurrency; P50/P95/P99 latency per format",
      "Error and bypass rates with reason codes",
      "Policy drift detection and config as code (GitOps)",
      "Observability (structured logs/metrics/traces) and SIEM/SOAR integration"
    ]
  },
  "architecture_patterns": [
    "Email/Collaboration — inline or API processing (inbound/outbound)",
    "Web/SWG/Proxy — ICAP or reverse-proxy integration for uploads/downloads",
    "Storage/Archive — on-ingest and scheduled remediation",
    "Application/SDK — embedded libraries; sync/async REST",
    "Kiosk/Air-Gap/CDS — offline operation; policy-controlled transfer",
    "Automation/SOAR/DLP — policy orchestration and event correlation"
  ],
  "faq": [
    {
      "q": "How are ZS levels used?",
      "a": "Each level represents a measurable assurance state. Set targets per workflow based on usability, risk, and compliance."
    },
    {
      "q": "Do higher levels replace lower ones?",
      "a": "Not always. Many architectures blend levels—high-assurance rebuild for critical paths, lighter hygiene elsewhere."
    },
    {
      "q": "What determines a level?",
      "a": "The underlying mechanism used to enforce file integrity and policy, not vendor branding."
    },
    {
      "q": "Does ZS-6 eliminate all threats?",
      "a": "ZS-6 minimizes dependence on detection by enforcing structural and policy correctness. Monitor residual risks, e.g., unsupported formats."
    }
  ],
  "notes": "This taxonomy is intended for open reference by architects, procurement teams, and evaluators. No vendor claims implied.",
  "version_semver": "1.1.0",
  "last_editor": "GPT-5 Pro",
  "status": "Draft for one-pager site",
  "license": "CC BY 4.0",
  "changelog": [
    {
      "version": "1.1.0",
      "date": "2025-11-10",
      "changes": [
        "Clarified level definitions and boundaries (ZS‑3 isolation vs. flattening).",
        "Added threats mitigated, residual risks, and ideal use‑cases per level.",
        "Introduced cross‑cutting policy defaults for encrypted, nested, signed, and unknown formats.",
        "Expanded evaluation criteria with security engineering and operations KPIs.",
        "Added buyer test harness guidance and signals/telemetry expectations.",
        "Provided architecture decision cues and glossary."
      ]
    }
  ],
  "cross_cutting_policies": {
    "encrypted_or_password_protected": [
      "Block or quarantine by default; optionally request passphrase via secure channel.",
      "Decrypt only in a trusted zone; then process at target ZS level; never re‑emit original bytes.",
      "Record cryptographic metadata (algorithms, key length) in logs; do not persist plaintext beyond processing."
    ],
    "archives_and_nesting": [
      "Parse recursively with safe depth/size bounds to prevent resource exhaustion.",
      "Apply target ZS level to each extracted item; rebuild archives deterministically.",
      "Detect and neutralize polyglot/format‑confusion and MIME/type mismatches."
    ],
    "unknown_or_unsupported_types": [
      "Policy‑driven: block, isolate (ZS‑3), or allow with explicit exception and time‑bound approval.",
      "Emit precise reason codes and telemetry for governance and vendor roadmap."
    ],
    "signed_and_time_stamped_content": [
      "Validate signatures and timestamps pre‑processing; preserve provenance in output metadata.",
      "After ZS‑4/5/6, signatures will not survive byte‑for‑byte—re‑sign in a trusted zone if required."
    ]
  },
  "signals": [
    "Per‑file outcome: pass, sanitize, repair, rebuild, block (with codes).",
    "Elements acted on (macros removed, links rewritten, objects dropped) and counts per type.",
    "Validation artifacts (schema checks, spec conformance IDs, parser versions).",
    "Provenance: input hash, output hash, transformation ID, policy version.",
    "Recursive archive map with depth/size statistics."
  ],
  "glossary": [
    {
      "term": "Allow‑only / Positive Selection",
      "def": "Copy only known‑good elements into a safe template; drop or rewrite everything else."
    },
    {
      "term": "Deep Parsing",
      "def": "Format‑aware parsing down to object model per file spec including ambiguous/edge constructs."
    },
    {
      "term": "Rebuild / Safe Blueprint",
      "def": "Construct a clean intermediate model and write a new file to spec with no source bytes carried over."
    },
    {
      "term": "Flattening",
      "def": "Render to static (PDF/image) removing active content and editability; often read‑only."
    },
    {
      "term": "Isolation",
      "def": "Execute or render in a segregated environment; risks are contained but source bytes may still be delivered."
    },
    {
      "term": "Polyglot",
      "def": "A file valid under multiple formats; can bypass naive type checks."
    }
  ],
  "decision_guide": [
    {
      "workflow": "Inbound third‑party uploads to portals or supplier data rooms",
      "recommended_levels": [
        "ZS-6"
      ],
      "rationale": "Highest assurance without user friction; zero source byte carryover."
    },
    {
      "workflow": "External email attachments to executive/finance/legal",
      "recommended_levels": [
        "ZS-6",
        "ZS-5"
      ],
      "rationale": "Critical role targeting risk; prefer ZS‑6, fall back to ZS‑5 for unsupported types."
    },
    {
      "workflow": "General employee collaboration (M365/Google)",
      "recommended_levels": [
        "ZS-6",
        "ZS-5"
      ],
      "rationale": "Default to deterministic rebuild for all supported formats; fall back to ZS-5 only when deterministic coverage is unavailable."
    },
    {
      "workflow": "Developer file downloads/tools installers",
      "recommended_levels": [
        "ZS-5",
        "ZS-3"
      ],
      "rationale": "Allow validation/repair; isolate where rebuild coverage is lacking."
    },
    {
      "workflow": "Cross‑domain/air‑gap transfer (ICS/OT, classified)",
      "recommended_levels": [
        "ZS-6",
        "ZS-3"
      ],
      "rationale": "Deterministic rebuild; isolation for interactive review prior to release."
    },
    {
      "workflow": "Public web browsing/downloads via SWG",
      "recommended_levels": [
        "ZS-3",
        "ZS-4"
      ],
      "rationale": "Flatten or isolate by default; allow‑only for common office formats."
    }
  ],
  "buyer_test_harness": {
    "dataset_components": [
      "Office docs with macros, embedded OLE/ActiveX, links, external templates.",
      "PDFs with JavaScript, launch actions, embedded files, form fields.",
      "Nested archives with depth>5, mixed encodings, and large files (resource abuse).",
      "Image files with malformed metadata/EXIF and steganography test vectors.",
      "Polyglot files (e.g., PDF/ZIP) and MIME/type confusion samples.",
      "Password‑protected archives and documents with known passphrases."
    ],
    "success_criteria": [
      "No source bytes carried over at ZS‑6; output passes strict validators.",
      "At ZS‑4/5, precise logs of removed/repaired elements; unsupported items handled per policy.",
      "No interactive features remain after ZS‑3; static rendering verified.",
      "Throughput and latency meet SLOs; error/bypass rates within defined thresholds."
    ],
    "tooling": [
      "Deterministic validators per format; hash‑based provenance tracker.",
      "Replay harness to compare outputs across policy versions (regression testing)."
    ]
  },
  "reference_slo_targets": {
    "default": {
      "p50_ms": 150,
      "p95_ms": 500,
      "p99_ms": 1500
    },
    "large_office_docs": {
      "p50_ms": 500,
      "p95_ms": 1500,
      "p99_ms": 5000
    },
    "archives_100mb_5depth": {
      "p50_ms": 1200,
      "p95_ms": 4000,
      "p99_ms": 12000
    }
  }
}
