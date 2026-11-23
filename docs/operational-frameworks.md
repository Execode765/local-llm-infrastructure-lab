# Personal Operational Frameworks — My Operating Firmware

> **Purpose:** Three repeatable frameworks for learning, engineering, and communicating. Think of them as personal operating firmware: when a situation changes, return to the appropriate procedure instead of improvising from scratch.
>
> **Scope:** These principles apply to the Local LLM Infrastructure Lab, Vapor Loop, field support, future infrastructure work, and everyday problem-solving. This is a reference document, **not a new project or lab**.

## 1. Engineering: Build → Test → Understand → Document → Verify

**Use when:** Creating, changing, troubleshooting, or validating a technical system.

| Step | Reminder |
| --- | --- |
| **Build** | Make one deliberate, scoped change or experiment. |
| **Test** | Observe how it behaves under defined conditions. |
| **Understand** | Explain what happened, why, and what remains uncertain. |
| **Document** | Record the configuration, commands, observations, limitations, and decisions. |
| **Verify** | Independently confirm the result and that the documentation matches reality. |

**Example:** Configure a monitoring exporter in the Ubuntu VM, collect a metric, explain its meaning, document the setup, then verify the metric and service status after a restart.

**Guardrail:** Do not call a milestone complete just because a command ran without an error. Record evidence and verify the outcome. Document significant engineering milestones before moving forward.

## 2. Operational Communication: Identify → Explain → Recommend → Document

**Use when:** Reporting an incident, describing a risk, escalating a problem, or advising someone on a technical decision.

| Step | Reminder |
| --- | --- |
| **Identify** | State the observed issue and distinguish facts from assumptions. |
| **Explain** | Describe the operational impact and relevant uncertainty in plain English. |
| **Recommend** | Propose a proportionate next action, including trade-offs or escalation. |
| **Document** | Preserve the evidence, decision, owner, and follow-up. |

**Example:** A branch PC has unexpectedly large IIS logs. Identify the disk usage, explain the storage/availability risk, recommend preserving evidence and investigating log growth, then document the drive swap and escalation. Do **not** claim the root cause is confirmed without evidence.

**Guardrail:** Communicate operational risk rather than panic. Separate observation, hypothesis, and recommendation.

## 3. Independent Learning: Look It Up → Understand → Apply → Recognize

**Use when:** Encountering an unfamiliar command, AI model, specification, platform, or technical concept.

| Step | Reminder |
| --- | --- |
| **Look It Up** | Consult a trusted reference, official documentation, or the personal cheat sheet. |
| **Understand** | Explain the concept in your own words; identify what you still don't know. |
| **Apply** | Use the idea in a small, safe example or connect it to a known system. |
| **Recognize** | Notice the same pattern the next time it appears; revisit the reference as needed. |

**Example:** See `Q4_K_M` on a model page; look up quantization, explain its memory/quality trade-offs, compare it with an existing Ollama model, and recognize the label when browsing again.

**Guardrail:** Don't confuse recognition with mastery. Ask questions when the reference is insufficient; independence does not mean avoiding help.

## Which firmware do I load?

| Situation | Framework |
| --- | --- |
| I'm building or troubleshooting something | **Engineering** |
| I need to explain an issue or recommend action | **Operational Communication** |
| I'm exploring something unfamiliar | **Independent Learning** |

The frameworks can be used together. For example: **learn** what a metric means, **engineer** a reliable monitoring test, then **communicate** what the results mean.

## Operating rules

- Keep the three frameworks stable; refine only when real experience shows a gap.
- Prefer evidence over assumptions, clarity over jargon, and completion over project sprawl.
- Preserve meaningful results in the appropriate existing repository.
- Use AI as an assistant to learning and verification, **not a substitute for independent understanding**.
- No new purchases, installations, or projects are implied by this document.

**Core reminder:** *Choose the framework. Follow the steps. Check the result.*

_Last documented: November 2026._
