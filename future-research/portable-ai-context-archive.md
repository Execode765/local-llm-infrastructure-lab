# Portable AI Context Archive — Future Research

**Status:** Shelved / future research — not started  
**Captured:** 2026-11-23  
**Priority:** After current infrastructure labs and existing engineering commitments  
**Purpose:** Preserve user-owned project continuity and working context independently of any one AI provider or model.

## Motivation and research question

How can a durable, human-readable, portable archive preserve enough accurate context from long-term AI-assisted work to let a different assistant (cloud or local) resume projects productively if a provider changes its memory, account features, or models?

This is **not** an attempt to export model weights, internal model state, subjective experience, or a complete recreation of a particular assistant interaction. The aim is to preserve **user-controlled knowledge, decisions, preferences, evidence, and next steps**.

## Proposed archive format (illustrative only)

```text
AI-CONTEXT-ARCHIVE/
├── README.md
├── personal-background.md
├── career-goals.md
├── learning-preferences.md
├── hardware-inventory.md
├── active-projects.md
├── project-decisions.md
├── future-projects.md
├── source-index.md
└── checkpoints/
```

Possible contents:
- **Projects:** Local LLM Infrastructure Lab, Vapor Loop, experiment milestones, verified findings, open issues, GitHub links and resume checkpoints.
- **Career/learning context:** Long-term professional goals, skill progression, preferred learning approach, pace, documentation rules.
- **Technical inventory:** Local devices, OS/VM setup, GPU/VRAM limits, software versions, environment constraints; dated and verified where possible.
- **Decision history:** Why a project or approach was adopted, deferred, or ruled out, with timestamps and source pointers.
- **Future research:** Parked ideas, reasons for shelving them, explicit triggers/prerequisites for revisiting.
- **Source index:** References to GitHub commits, documents and user-provided notes rather than treating AI recollections as ground truth.

## Core design requirements

1. **Portability:** Plain UTF-8 Markdown, stable paths, no dependency on a proprietary memory feature; optionally a manifest/index for navigation.
2. **Accuracy:** Mark each item as verified, user-reported, proposed, or unknown, with dates and provenance. Preserve corrections; do not invent missing history.
3. **Continuity:** Provide a concise “start here” summary and current next steps so another assistant can quickly orient itself without processing an entire chat export.
4. **Modularity:** Keep reusable long-term context separate from per-project details and dated session checkpoints.
5. **Privacy and security:** Store the full personal archive locally by default, **not in this public GitHub repository**. Review and redact personal, household, financial, account, authentication, device identifiers, and other sensitive data before sharing with any model or service. Never include secrets, passwords, API keys, private keys or unredacted credentials.
6. **Backup integrity:** Evaluate versioning, offline copies, integrity checks, and an understandable recovery workflow.
7. **Interoperability:** Test loading a controlled subset into at least one other assistant or local LLM, checking what context transfers correctly, what does not, and the practical context-window/token limitations.
8. **Limitations:** A chat history export, if available, is source material—not a portable copy of model memory or a guarantee that another model will reproduce identical behavior.

## Candidate future research process — not executed

1. Review ChatGPT’s **then-current** official export procedure, data formats, and any retention or privacy implications before exporting.
2. Design a minimum useful Markdown context schema and template.
3. Inventory existing public GitHub documentation first to avoid duplicating verified engineering facts.
4. Gather and curate user-approved background and relevant conversation/export data; distinguish firsthand information from inferred or generated claims.
5. Build a small, dated, cross-referenced archive; keep private sections local and selectively shareable summaries separately.
6. Run a controlled transfer test with another assistant or local model on a familiar lab scenario, compare factual continuity and missing context, and revise.
7. Define a lightweight ongoing update/checkpoint policy without creating busywork.

## Relationship to existing GitHub checkpoint rule

Every infrastructure working session already ends by updating its **GitHub experiment checkpoint**. Those technical checkpoints remain the primary authoritative source of verified lab progress. A future personal context archive should **reference** those sources, not replace them or copy sensitive personal context into a public repository.

## Decision / no-work boundary

**Future research only.** No export, ingestion, local tooling, sensitive-data collection, model migration, or new active lab is authorized by this document. Do not divert resources from Lab 04 or other existing priorities. Revisit only when requested.

**Working principles:** Build → Test → Understand → Document → Verify; Identify → Explain → Recommend → Document.
