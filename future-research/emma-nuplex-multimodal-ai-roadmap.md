# Real-Time Multimodal AI Reverse Engineering — Future Research Roadmap

**Status:** PARKED / PLANNING ONLY (2026-11-22)  
**Research target:** Nuance Labs' Emma / Nuplex real-time audiovisual conversational AI, subject to re-verification of current public claims and access terms when resumed.  
**Decision:** Path B — publish reproducible findings and limitations so researchers with greater compute can continue. GPU rental is an optional later decision, not a dependency.  
**Priority:** Do not divert from Local LLM Infrastructure Lab 04 or Vapor Loop engineering milestones.  
**Repository strategy:** This issue is a temporary discovery/index record, **not** an active Local LLM lab. Create a separate research repository when the work is approved to begin; move/cross-link this roadmap there.

## Purpose
Learn how sophisticated real-time multimodal conversational AI systems work by investigating publicly observable interfaces and behaviors, testing falsifiable architectural hypotheses, and implementing limited original reproductions. The objective is *understanding and reproducibility*, **not** extracting proprietary model weights or claiming to have cloned Emma.

## Available resources and constraints
- ASUS ROG Strix G814JI laptop, Intel Core i9-13980HX, NVIDIA RTX 4070 Laptop GPU (8 GB VRAM), Windows.
- Existing Ubuntu VirtualBox VM, Python learning path, Ollama and small Qwen models.
- No additional hardware purchase required or authorized. Avoid disrupting music-production and existing lab configurations.
- No assumption that access to an API reveals proprietary neural architecture.
- No paid API calls or GPU rental without a separate cost/benefit review and explicit approval.

## Research questions
1. Which components of a real-time audiovisual AI system are publicly documented, and which remain unknown?
2. What can input/output timing, interruption behavior, speech overlap, and audiovisual synchronization reveal?
3. What roles might transport, voice activity detection, speech recognition/generation, language modeling, animation, and session orchestration play?
4. Which architectural hypotheses are consistent with observations? What experiments could falsify them?
5. Which capabilities can be reproduced independently using open-source components on an 8 GB GPU?
6. Where are the precise compute, latency, memory, cost, access, and information boundaries?

## Phased roadmap
### Phase 0 — Scope, evidence, and safeguards
- [ ] Reconfirm official documentation, publication dates, terms of service, access permissions, and available public research.
- [ ] Create source register with URLs, access dates, and reliability assessments.
- [ ] Create claims ledger: **verified / observed / inferred / speculative / unknown**.
- [ ] Define privacy, consent, retention, security, and budget boundaries.

**Exit:** Written scope and test plan approved; no unapproved access or charges.

### Phase 1 — Public architecture reconnaissance (read-only)
- [ ] Inventory publicly documented APIs, SDKs, event schemas, session lifecycles, transport protocols, and published technical explanations.
- [ ] Draw a *documented-components* diagram, separate from a *hypothesized-internals* diagram.
- [ ] Identify alternative explanations for observed features.

**Exit:** Source-cited architecture map with uncertainty labels.

### Phase 2 — Authorized black-box measurement
- [ ] Establish repeatable input prompts, recording conditions, clocks, and baseline network measurements.
- [ ] Measure turn timing, interruptions, overlap, recovery behavior, and audio/video synchronization where permitted.
- [ ] Capture only authorized event/log data; redact credentials and personal data.
- [ ] Repeat tests and report distributions, confounders, and sample sizes.

**Exit:** Reproducible methodology, raw or privacy-safe observations, and limitations.

### Phase 3 — Hypothesis testing
- [ ] Compare plausible pipelines (cascaded versus more integrated architectures) without claiming to identify hidden weights or exact internals.
- [ ] Form falsifiable predictions and design experiments to distinguish alternatives.
- [ ] Mark conclusions as supported, inconclusive, or refuted.

**Exit:** Evidence-backed hypothesis matrix and revised architecture diagrams.

### Phase 4 — Small-scale independent reproduction
- [ ] Build isolated proof-of-concept components: audio capture/VAD, streaming event flow, interruption handling, speech response, and optional audiovisual timing.
- [ ] Use openly licensed components and small models; document versions and licenses.
- [ ] Benchmark CPU, GPU VRAM, system RAM, latency, and stability.
- [ ] Avoid implying feature similarity proves identical architecture.

**Exit:** Runnable minimal demo, reproducible benchmark, and hardware resource report.

### Phase 5 — Publication and handoff (Path B)
- [ ] Publish methods, source citations, hypotheses, diagrams, original code, anonymized results, and negative findings.
- [ ] Publish a **Compute & Information Limitations** report separating resource ceilings from unknowable proprietary details.
- [ ] Write reproducibility instructions and a prioritized **Next Experiments for Higher-Compute Researchers** handoff.
- [ ] Consider GPU rental *only* if a specific experiment has measurable benefit, a budget cap, and prior approval.

**Exit:** Research artifact others can reproduce or extend; no requirement to recreate Emma/Nuplex.

## Proposed future repository layout
```text
real-time-multimodal-ai-research/
  README.md
  ROADMAP.md
  docs/
    scope-and-ethics.md
    sources-and-claims.md
    architecture-documented.md
    architecture-hypotheses.md
    compute-limitations.md
  experiments/
    01-public-interface-audit/
    02-behavioral-measurements/
    03-hypothesis-tests/
    04-open-source-prototype/
  results/
    README.md
  handoff/
    next-experiments.md
```

## Working principles
- **Build → Test → Understand → Document → Verify.**
- **Identify → Explain → Recommend → Document.**
- Never present hypotheses as facts, product marketing as independent validation, or API-level access as internal model access.
- Investigate only public information and authorized interfaces. Do not bypass access controls, obtain proprietary weights, extract private data, or violate service restrictions.
- Every significant milestone gets documented before advancing.
- Finish existing commitments before starting this research.

## Resume point
When ready: open this issue, verify source documentation and current permissions, create a **separate** repository, transfer the roadmap, and begin **Phase 0 only**. No research execution has been authorized by this planning issue.


---

**Planning archive note (2026-11-23):** This roadmap is mirrored from [GitHub Issue #1](https://github.com/Execode765/local-llm-infrastructure-lab/issues/1) into `future-research/` for consistent discovery alongside other parked research concepts. The issue remains the original discussion/index record. This archival copy does not activate the project or supersede the proposed separate repository when research is approved.
