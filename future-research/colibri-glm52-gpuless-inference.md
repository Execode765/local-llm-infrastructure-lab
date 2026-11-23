# Colibri and GLM 5.2 — GPU-less Large-Model Inference (Future Research)

**Status:** Shelved / unverified — not an active lab  
**Captured:** 2026-11-23  
**Origin:** Social-media post shared in a prior ChatGPT conversation (screenshot; original post URL not captured)

## Original claim (not independently verified)

A social-media post attributed to Alex Nexford claims that an Italian developer used a project called **Colibri** to run **GLM 5.2**, described in the post as a **744-billion-parameter** model, on a laptop with **no dedicated graphics card**. The post further claims that Colibri keeps approximately **10 GB of model data in RAM** and streams the remainder from disk, permitting offline chat without per-token API fees.

These are claims from the post, **not demonstrated experimental results**. No repository URL, benchmark, model artifact, reproducible setup, or independent confirmation was established during the original discussion. The exact model name, parameter count, developer attribution, and memory architecture all require verification.

## Questions for a later review

1. Locate the original Colibri source repository and identify the maintainer, license, supported platforms, and implementation.
2. Verify the specific GLM model and parameter count; distinguish total parameters, active parameters (if applicable), and quantized storage size.
3. Determine what “running” means: successful load, token generation, useful interactive speed, or a reproducible benchmark.
4. Examine actual RAM, storage, CPU, and GPU requirements; distinguish disk-backed memory mapping, streaming, caching, and model offloading.
5. Look for independently reproducible measurements of time to first token, tokens per second, storage throughput, and system responsiveness.
6. Assess whether the method has any educational value for the existing local inference infrastructure lab, without assuming it is practical for everyday use.

## Constraints and decision

- **Do not start this experiment now.** Continue the active Local LLM infrastructure labs and other existing commitments.
- **No purchases, cloud credits, or hardware changes** are authorized by this note.
- Use existing hardware only if a later review justifies a safe, bounded experiment.
- Clearly separate verified facts, observed results, marketing claims, and hypotheses.
- Apply **Build → Test → Understand → Document → Verify** before publishing any performance conclusions.

## Revisit trigger

Reopen only when explicitly requested, ideally after current lab milestones are complete. Begin with source verification and a feasibility assessment; do not download large model weights or attempt execution as the first step.

## Source provenance

- Screenshot of a social-media post supplied in the ChatGPT conversation, originally shelved in November 2026.
- No direct link to the original post or Colibri GitHub repository was supplied. Search results and source code must be checked when research resumes.
