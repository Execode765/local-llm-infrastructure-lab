# 8 GB VRAM AI Coding Agent Challenge — Future Research

**Status:** Planned / shelved — not started  
**Captured:** 2026-11-23 (proposal developed in an earlier conversation)  
**Repository:** `Execode765/local-llm-infrastructure-lab`  
**Future lab number:** To be determined  
**Priority:** After current Local LLM infrastructure and Vapor Loop milestones

## Research question

How effectively can a locally hosted AI coding agent constrained to **8 GB of dedicated GPU VRAM** complete real-world software engineering tasks compared with a frontier cloud-based AI coding agent?

The experiment is **not** designed to prove that a small local model outperforms a frontier model. It aims to establish what is achievable, repeatable, and efficient with affordable consumer hardware, and where limitations emerge.

## Existing equipment and initial candidate

- ASUS ROG Strix G814JI laptop
- Intel Core i9-13980HX CPU
- NVIDIA RTX 4070 Laptop GPU with **8 GB dedicated VRAM**
- Existing Windows / Ollama local-inference environment
- **Qwen3.5 4B** as an *initial candidate*, subject to future verification of model availability, fit, and agent compatibility
- A compatible coding-agent interface, **potentially Codex CLI**, subject to compatibility testing when the lab begins

No additional GPU, purchased hardware, rented compute, or cloud infrastructure is required or authorized. A frontier-cloud coding agent would serve as a **comparison baseline**, subject to future access, cost, and configuration decisions; this note does not authorize paid API use.

## Proposed experimental method (not executed)

Use **isolated copies of a fixed Vapor Loop codebase snapshot** and present comparable local and cloud agents with:

1. **Code comprehension:** Explain relevant JavaScript, WebAssembly (where present), MIDI, and audio-processing code accurately.
2. **Bug diagnosis and repair:** Detect and correct deliberately introduced, documented defects.
3. **Feature development:** Implement a small, precisely scoped improvement with defined acceptance tests.
4. **Hardware efficiency:** Observe GPU utilization, peak dedicated VRAM, CPU and system RAM use, offloading behavior, inference latency, and throughput.

Confirm actual components and test coverage in the selected source snapshot rather than assume every technology is represented.

## Evaluation metrics

- Correct task completion and acceptance criteria met
- Automated tests passed and regressions introduced
- Wall-clock time per task and across repeated trials
- Input/output token counts and measured throughput, where available
- Peak dedicated GPU VRAM usage and GPU utilization
- CPU load, system RAM, and CPU/RAM offloading
- Number and type of human interventions
- Code clarity, maintainability, and review findings
- Local versus cloud **incremental operating costs**, with assumptions disclosed (including any subscriptions, API fees, energy use estimates, and hardware already owned)

Distinguish model inference time from total coding-agent execution time. Report unavailable metrics explicitly instead of estimating silently.

## Experimental controls

- Identical, version-pinned source-code snapshot and isolated working copies
- Identical task descriptions and objective acceptance criteria
- Same coding-agent software, tooling permissions, and test harness **where practical**; document differences when not
- Repeat trials to assess consistency rather than rely on one successful demonstration
- Record exact model identifiers, quantization, context settings, Ollama version, agent version, hardware baseline, and relevant configurations
- Keep production Vapor Loop source and branches untouched; review generated code and execute only within bounded test copies
- Document failures, context limits, partial completions, retries, and unexpected results without selecting only successful runs

## Prerequisites and feasibility gates

Before beginning:

1. Make further documented progress on active Local LLM infrastructure labs.
2. Reach appropriate Vapor Loop engineering and documentation milestones.
3. Freeze suitable source snapshots and create objective task fixtures and tests.
4. Verify actual compatibility between the selected local model, Ollama endpoint, and intended coding-agent interface. **Codex CLI compatibility is a question to test, not an established fact.**
5. Define a fair frontier-cloud baseline and explicit spending limits before any paid access.
6. Establish safe isolation, measurement tools, and repeatable reporting procedures.

## Decision / no-work boundary

**Preserve this as future research only. Do not start implementation, install tools, run benchmarks, modify Vapor Loop, buy equipment, or change current priorities.** The 8 GB dedicated-VRAM constraint is fundamental and must not be relaxed simply to improve results. Local CPU/RAM offloading may be measured, but it must not be misrepresented as GPU-only execution.

The existing active projects take precedence. The future lab number remains **TBD**.

## Revisit trigger

Reopen only on explicit request after the relevant Local LLM and Vapor Loop milestones. Start with a compatibility/feasibility review and a small objective pilot design, not an installation or a large code-generation run.

## Guiding principles

**Build → Test → Understand → Document → Verify**  
**Identify → Explain → Recommend → Document**

> We're not benchmarking against what money can buy. We're investigating what knowledge can accomplish with the hardware we already own.

## Source provenance

- Research proposal and constraints supplied in the ChatGPT conversation titled **Future Local LLM Lab: 8 GB VRAM AI Coding Agent Challenge**.
- No benchmark runs, agent integrations, measured hardware performance, or cloud comparisons have been completed for this future experiment.
