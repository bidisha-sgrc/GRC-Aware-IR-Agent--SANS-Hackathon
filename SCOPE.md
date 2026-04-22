One-line pitch
An autonomous incident response agent that doesn't just find evil — it produces GRC-ready outputs (MITRE ATT&CK mappings, compliance control impact, risk scores, incident notification drafts) so a Security GRC team can act on IR findings immediately.
The problem
Existing autonomous IR agents (including the Protocol SIFT baseline) produce DFIR output — what happened, which processes are malicious, what the attacker did. But the gap between "here's what forensics found" and "here's what GRC/compliance/leadership needs" remains manual. In a 7-minute breakout-time world, that gap is the bottleneck.
What this agent adds
On top of Protocol SIFT's DFIR baseline, the agent produces in seconds:

Dual-taxonomy technique mapping — every observed behavior tagged with both MITRE ATT&CK (classic adversary TTPs) and MITRE ATLAS (AI-system attacks, agentic AI threats)
Paired control impact — NIST CSF for traditional controls; ISO 42001 for AI governance controls (clause-level references to preserve ISO copyright)
Risk scoring — asset criticality × technique severity → quantified risk
Incident notification draft — timing guidance (GDPR 72h, SOC 2 triggers) and who-to-notify list

Why this taxonomy pairing matters
The FIND EVIL! hackathon theme centers on AI-powered threats. Most submissions will map only to classic ATT&CK. This project maps to both adversary frameworks in parallel:
Threat taxonomyControls frameworkCoversMITRE ATT&CKNIST CSF 2.0Classic adversary TTPs and defensive controlsMITRE ATLASISO/IEC 42001AI-system attacks (prompt injection, poisoned tools) and AI governance
The symmetric structure is a deliberate design choice — it means a single agent produces both a traditional IR report and an AI-threat IR report from the same analysis run.
Architectural approach
Custom MCP Server. Per the FIND EVIL! "What to Build" options, this is the purest fit for GRC-specific outputs. It gives us:

Typed function signatures (no generic shell) — judges love this for auditability
Architectural guardrails (the agent physically cannot call non-GRC tools through this server) — Requirement 3 gold
Clean structured output — Requirement 8 near-free
Clear trust boundaries — Requirement 3's architectural vs. prompt-based guardrail distinction

Agent uses Claude Code (your existing Claude Pro). Protocol SIFT handles DFIR. Our custom MCP adds GRC layer.
The 4 MCP tools (minimum viable)
ToolInputOutputWhy it mattersmap_iocs_to_techniques(iocs, frameworks=["ATT&CK","ATLAS"])List of observed behaviors or IOCs + frameworks to queryGrouped technique IDs per framework: {"ATT&CK": [...], "ATLAS": [...]}Dual taxonomy coverage — catches both classic and AI-system attacksmap_techniques_to_controls(technique_ids, framework)Technique IDs + "NIST_CSF" or "ISO_42001"List of affected controls/clauses with identifiers (ISO clause references only — no verbatim text)Compliance-ready traceability across both traditional and AI governance frameworksscore_risk(findings, asset_context)Findings + asset metadata (criticality, data sensitivity)Numeric risk score + severity labelPrioritization for respondersgenerate_grc_report(case)Case context (uses all above)Markdown report with sections: Executive Summary, Dual-Taxonomy Coverage, Control Impact (NIST CSF + ISO 42001), Risk Register, Notification DraftThe actual deliverable to GRC/leadership
Reference data sources
FrameworkSourceLicense/DistributionMITRE ATT&CKSTIX JSON from github.com/mitre/ctiApache 2.0 — redistributableMITRE ATLASSTIX JSON from atlas.mitre.orgCC BY 4.0 — redistributable with attributionNIST CSF 2.0OSCAL/JSON from NISTPublic domain (US Government) — redistributableISO/IEC 42001:2023Clause references only (by ID/number)ISO-copyrighted — we do NOT ship clause text; we ship ID references and brief topical descriptions in our own words
Future work (out of scope for MVP)

CSA MAESTRO threat-modeling integration (complementary to ATLAS; different integration pattern)
ISO 27001 full security controls mapping (stretch goal Week 5 if time allows)
NIST SP 800-53 detailed control mapping
Multi-agent orchestration per LangGraph/AutoGen patterns

Non-goals (what this is NOT)

Not a replacement for Protocol SIFT's DFIR capability
Not a general-purpose GRC platform
Not a SIEM integration
Not a network-accessible service (runs locally on SIFT as MCP server)

Success criteria

Agent runs end-to-end against real rd01 memory evidence (SANS FOR508 subset)
Produces a GRC report judges can read without DFIR background and understand
At least 80% of known IOCs in the demo case correctly tagged with ATT&CK techniques
At least one self-correction event visible in execution logs

Licensing
MIT License. 

 4 MCP tools: map_iocs_to_techniques, map_techniques_to_controls, score_risk, generate_grc_report — kept as four tools with Tool 1 expanded to support both ATT&CK and ATLAS frameworks
 Primary technique taxonomies: MITRE ATT&CK + MITRE ATLAS (dual mapping)
 Controls frameworks: NIST CSF 2.0 + ISO/IEC 42001 (ISO referenced by clause ID to respect copyright)
 Project name: "GRC-Aware IR Agent" — working title, subject to revision closer to submission
 License: MIT
