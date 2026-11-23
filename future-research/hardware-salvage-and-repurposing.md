# Future Research — Hardware Salvage and Repurposing

**Status:** Shelved / Not Started  
**Priority:** After active Local LLM infrastructure labs  
**Purchases:** None authorized; assess needs and costs first  
**Date recorded:** 2026-11-23

## Objective
Evaluate two older gaming desktops and salvaged components for a low-cost, educational home infrastructure / local AI expansion. Focus on learning hardware diagnostics, Linux/server administration, storage health, networking, and GPU inference. Do not assume that repairing or buying an enclosure is cost-effective.

## Known inventory (not yet fully verified)
| Item | Observation | Unknowns |
|---|---|---|
| ASUS ROG G15CS desktop | Powers on but does not boot; CPU cooler/fan attachment and thermal paste need attention | CPU, RAM, motherboard, PSU condition, exact boot fault |
| ASUS NVIDIA GeForce RTX card | Removed from G15CS; PCB marking CG180P2 Rev 1.00 | Exact GPU model, VRAM, power draw, driver/CUDA support |
| Seagate BarraCuda HDD | 1 TB SATA drive | SMART health, existing data, remaining life |
| SSD | Salvaged from desktop | Model, capacity, interface, SMART health, existing data |
| Second gaming desktop | In storage | Entire component inventory and condition |

**Important:** G15CS original factory configurations do not prove the identity of the installed GPU. PCB markings alone are not a confirmed model identifier.

## Questions for future investigation
1. Can the ASUS desktop be safely repaired, and which parts (cooler mount, thermal paste, fan, PSU, etc.) are actually needed?
2. What is the exact RTX GPU model, VRAM, compute capability, and current Ollama/CUDA compatibility?
3. Would a restored desktop running Linux/SSH/Ollama offer more value than an external GPU enclosure? Compare cost, power, performance, and laptop interface compatibility.
4. Are the SSD/HDD healthy? Is there user data that should be preserved before wiping or reusing them?
5. Can the second desktop supply compatible spare parts, or become an independent test server?
6. What is the minimum parts budget, and when is replacement hardware a better choice than repair?

## Proposed future workflow (not authorized to start)
1. **Inventory:** Photograph model/part labels; list CPU, GPU, motherboard, RAM, PSU, drives and connectors.
2. **Safety first:** Do not power the G15CS with an unsecured CPU heatsink/fan. Check fit and cooling before further boot diagnostics.
3. **Non-destructive storage review:** Use suitable SATA connections/adapters, assess SMART data, and preserve/recover desired files before any formatting or write-heavy tests.
4. **Fault isolation:** Troubleshoot POST/boot with known-good cooling and minimal configuration; document observed symptoms, not assumptions.
5. **GPU verification:** Identify GPU with hardware labels or a compatible working PC, then test driver support, VRAM, and a small local inference workload.
6. **Architecture/cost decision:** Compare repaired standalone Linux host vs eGPU enclosure vs storage-only reuse; confirm enclosure interface and power compatibility before spending.
7. **Document and verify:** Record tests, measurements, costs, limitations, and a recommendation.

## Decision gates
- No purchases until hardware identity, compatibility, and cost/benefit are established.
- No assumption that a salvaged GPU will outperform the ASUS laptop RTX 4070 8 GB.
- No formatting or destructive tests on salvaged drives until data recovery needs are checked.
- No change to the active Local LLM lab sequence.

## Guiding principles
**Build → Test → Understand → Document → Verify**  
**Identify → Explain → Recommend → Document**

**Next action:** None. Revisit when the user explicitly chooses to resume this research.
