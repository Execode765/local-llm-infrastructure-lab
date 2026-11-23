# ANBERNIC RG40XX V — Portable Synthesizer Research

**Status:** Shelved — research only; no experiments started  
**Category:** Embedded Linux / Digital Audio / MIDI / Synthesizer Development  
**Hardware:** ANBERNIC RG40XX V (owned; configuration and condition not yet verified)  
**Recorded:** 2026-11-23  
**Project boundary:** This is a future research concept, **not** a Local LLM Infrastructure Lab experiment and **not** an active Vapor Loop development milestone.

## Purpose

Investigate the RG40XX V as a small standalone digital musical instrument platform. Possible outcomes include an original synthesizer, sample-based instrument, sequencer, and/or a portable adaptation of Vapor Loop. Reuse infrastructure skills (Linux, SSH, troubleshooting, storage, recovery, automation, documentation) without conflating this audio project with the active infrastructure labs.

## Starting references

- [GrainBrick by ETRNLIAM](https://github.com/ETRNLIAM/GrainBrick) — an existing granular-synthesis handheld project; review its current documentation, supported devices, installation method, licensing, and release before testing. Prior discussion identified the RG40XX V with KNULLI as a reported tested combination; **independently reconfirm before treating as verified**.
- [KNULLI](https://knulli.org/) — alternative firmware/distribution to evaluate for hardware support and application deployment.
- [Vapor Loop](https://github.com/Execode765/vapor-loop) — source project for potential reuse of sequencing, MIDI, rhythm and companion logic; port feasibility untested.

## Proposed development path (not started)

### Phase 1 — Inventory and safety baseline
- Confirm actual model, hardware revision, stock OS, microSD arrangement, available storage, firmware version, USB ports, audio output, and controls.
- Preserve factory media where feasible; document backups and a verified recovery path before any firmware experiments.
- Distinguish removable-card OS images from on-device firmware and bootloader changes.

### Phase 2 — KNULLI / GrainBrick feasibility
- Confirm supported KNULLI image and documented installation instructions for the exact model.
- Use a separate quality microSD card; avoid modifying the original system card.
- Test GrainBrick only if its licensing, release, dependencies, and installation instructions permit; record whether it launches, produces audio, and responds to controls.
- Capture actual observations rather than claiming compatibility from similar hardware alone.

### Phase 3 — Development access and platform characterization
- Evaluate supported SSH access, file transfer, native build vs cross-compilation, Python/SDL2 and suitable audio libraries.
- Determine real USB host and MIDI capabilities, drivers, permissions, and connectivity options.
- Measure audio latency, CPU load, memory usage, storage performance, and stability with reproducible tests.

### Phase 4 — Minimal original synthesizer
- Build a tiny proof of concept: oscillator or sample playback, envelope, button mapping, audio output, and preset save/load.
- Add features only after verifying timing, audio quality, and stable operation.
- Document build steps, dependencies, and performance constraints.

### Phase 5 — Vapor Loop Portable feasibility
- Audit the current Vapor Loop codebase for reusable MIDI, sequencing, clock, groove, and companion logic.
- Compare a browser-based runtime against a native UI/audio implementation; do not assume the device has a suitable browser.
- Prototype a limited feature set before attempting a full port; define MIDI and audio acceptance tests.

### Phase 6 — Multi-instrument launcher and storage
- Explore separate applications (synth, drum engine, sample mangler, sequencer), shared presets, and a simple launcher.
- Prefer multiple apps under one stable OS. Storing multiple OS images on a card **does not** itself provide multiboot or safe firmware switching.
- Consider alternative OS images or multiboot only after confirming hardware-specific support and recovery.

### Phase 7 — Verification and publication
- Record commands, measurements, failures, rollback procedures, design decisions, and lessons learned.
- Publish active code and detailed engineering work in a dedicated project repository (or Vapor Loop repository when appropriate), not inside this future-research backlog.
- Maintain clear boundaries between proposed features, tested results, and unsupported assumptions.

## Risks and open questions

- Exact RG40XX V software compatibility and current GrainBrick/KNULLI support
- USB MIDI host support and practical controller connectivity
- Audio latency, sound quality, and processor/RAM limits for real-time DSP
- Browser runtime availability and JavaScript portability
- Licensing restrictions on reusing third-party code
- Boot/recovery behavior, microSD reliability, and rollback
- Whether a native engine is more practical than porting the existing browser application

## Activation criteria

**Do not start now.** Revisit only when explicitly prioritized after existing commitments. Before activating: verify current support documentation, define a narrow first experiment, preserve the factory system, and identify a recovery path. No additional hardware purchase is authorized by this research note.

## Working principles

**Build → Test → Understand → Document → Verify**  
**Identify → Explain → Recommend → Document**

**Research library policy:** One centralized `future-research/` library can hold ideas from multiple disciplines. Active projects keep their own repositories and engineering records. An idea being documented does not make it an obligation or active lab.
