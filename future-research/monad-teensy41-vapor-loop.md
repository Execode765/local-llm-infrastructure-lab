# MONAD / Teensy 4.1 Groovebox — Future Research

**Status:** Shelved — future research only (not started)  
**Recorded:** 2026-11-23  
**Priority:** After current FM-1 Vapor Loop firmware milestones and Local LLM infrastructure work  
**Purchase authorization:** None; no Teensy, audio shield, or controller purchase planned

## Origin

A Synth Anatomy article highlighted MONAD, a project using a Native Instruments Maschine MK3 controller as the human interface for a sampling/groovebox system. Discussion centered on a reported Teensy-based processing implementation, rather than modifying the Maschine MK3 into a self-contained synthesizer.

Reference: https://synthanatomy.com/2026/11/monad-turns-the-maschine-mk3-midi-controller-into-an-8-part-sampling-groovebox.html

**Verification note:** Confirm the original developer's documentation, exact hardware architecture, source availability, license, and whether MONAD firmware is publicly released before treating it as reusable code. An article or demonstration does not establish an open-source license.

## Research question

Could ideas from MONAD's controller-to-embedded-engine architecture inform a future standalone **Vapor Loop** hardware implementation using a Teensy 4.1, while preserving the existing FM-1 firmware roadmap?

## Possible future investigation

1. Identify the original MONAD project, author, firmware repository, supported Teensy model, build instructions, and license.
2. Map the architecture: Maschine MK3 control input, bidirectional feedback (if any), Teensy firmware, sampling/synthesis engines, storage, and audio output.
3. Determine whether standard USB MIDI is sufficient or whether proprietary Maschine protocols, USB host support, or additional hardware are required.
4. Evaluate reusable **concepts** separately from legally reusable **source code**; respect attribution and licensing.
5. Compare Teensy 4.1 resources and timing with Vapor Loop's MIDI phrase, bass, chord, rhythm, and synchronization engines.
6. Prototype the smallest MIDI-only proof of concept first, before considering audio shield, sampling, or a dedicated enclosure.
7. Record costs and electrical/interface risks before any purchase.

## Guardrails

- **Not an active project**; do not interrupt FM-1 diagnostics, recovery preparation, firmware development, or current infrastructure labs.
- No purchases, flashing, wiring, or experimental controller protocol commands at this stage.
- Do not assume browser JavaScript or FM-1 firmware can run unchanged on Teensy; platform-specific C/C++ porting may be required.
- Distinguish confirmed implementation details from hypotheses and marketing descriptions.
- Follow **Build → Test → Understand → Document → Verify** when activated.

## Future outcome

A documented feasibility decision: adapt compatible MONAD concepts, write an independent Teensy-based Vapor Loop implementation, or defer the idea if hardware, licensing, complexity, or cost do not justify it.
