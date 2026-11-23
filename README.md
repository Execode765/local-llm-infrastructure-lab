# Local LLM Infrastructure Lab

**Author:** Kelli Love | EXEcode765

Hands-on portfolio documenting local LLM deployment, GPU inference testing, Linux server virtualization, remote administration, and infrastructure troubleshooting. The goal is to develop practical skills for systems administration, data center operations, and AI infrastructure.

**Progress snapshot: November 23, 2026** — Labs 01, 02, and 03 completed within their documented scopes. Lab 03 Phase 1 and Phase 2 Experiments 01–02 were completed, verified, and documented. Lab 04 remains planned; Experiment 01 (read-only VM baseline assessment) is the next checkpoint.

## Lab progress

| Lab | Focus | Status | Documentation |
|---|---|---|---|
| 01 | Ollama deployment, GPU verification, storage and benchmarking | Completed | [Lab 01](labs/lab-01-ollama-setup/) |
| 02 | Qwen inference and performance testing | Completed within available hardware limits | [Lab 02](labs/lab-02-qwen-inference/lab-02-qwen-inference.md) |
| 03 | Ubuntu Server VM, SSH administration, controlled failure and recovery, security fundamentals | Completed within documented scope — Phase 1 and Phase 2 Experiments 01–02 verified and documented; additional optional troubleshooting exercises not claimed complete | [Phase 1](labs/lab-03-ubuntu-server/lab-03-phase-1.md) · [Phase 2 — Experiment 01: SSH Recovery](labs/lab-03-ubuntu-server/lab-03-phase-2-experiment-01-ssh-recovery.md) · [Phase 2 — Experiment 02: SSH Hardening](labs/lab-03-ubuntu-server/lab-03-phase-2-experiment-02-ssh-hardening.md) · [Command reference](labs/lab-03-ubuntu-server/lab-03-command-reference.md) |
| 04 | Infrastructure monitoring, dashboard-building, alerting, and optional Windows/Ollama telemetry | Planned — experiments 01–05 outlined; no implementation started | [Lab 04 roadmap and resume checkpoint](labs/lab-04-infrastructure-monitoring/README.md) |

## Latest completed activity: Lab 03 — Phase 2, Experiment 02: SSH Hardening

**Completed and documented:**
- Installed Ubuntu Server 24.04.5 LTS in VirtualBox, configured storage and NAT networking, and established SSH access from Windows PowerShell.
- Captured a clean-install VM snapshot for recovery.
- Performed a controlled SSH connectivity failure-and-recovery exercise, preserving the troubleshooting commands and observed results.

**SSH hardening verified:** Created a pre-hardening snapshot and configuration backups; configured ED25519 key login; added a dedicated early-loading SSH server hardening snippet; passed syntax and effective-policy checks; reloaded the service; confirmed a fresh key-based login and rejection of password-only SSH access; reviewed service logs. Rollback steps are documented but were not executed. See the [technical report](labs/lab-03-ubuntu-server/lab-03-phase-2-experiment-02-ssh-hardening.md) and [plain-English lesson](concepts-explained/05-ssh-hardening-explained.md).

**Next work:** Lab 03 is complete within its documented scope. The next planned milestone is Lab 04 — Infrastructure Monitoring and Dashboard Building, beginning with Experiment 01: read-only VM baseline assessment. No Lab 04 implementation has started.

**Command study guide:** [Lab 03 Command Reference](labs/lab-03-ubuntu-server/lab-03-command-reference.md) now includes a categorized index for diagnostics, configuration, verification, troubleshooting, recovery, and security commands, with Windows/Ubuntu context and safety labels.

## Concepts explained

Beginner-friendly explanations accompany the technical labs. See the [Concepts Explained](concepts-explained/) section for the published lessons and index.

## Technologies

Windows, PowerShell, Ollama, local LLMs, NVIDIA GPU acceleration, Oracle VirtualBox, Ubuntu Server, Linux CLI, SSH, LVM, Git and GitHub.

## Working principles

**Build → Test → Understand → Document → Verify**

**Identify → Explain → Recommend → Document**

Each lab records its objectives, environment, commands, actual observations, troubleshooting, operational implications, and lessons learned. This repository documents hands-on experiments, not claimed production deployments. Planned work is clearly distinguished from completed results.
