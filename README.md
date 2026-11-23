# Local LLM Infrastructure Lab

**Author:** Kelli Love | EXEcode765

Hands-on portfolio documenting local LLM deployment, GPU inference testing, Linux server virtualization, remote administration, and infrastructure troubleshooting. The goal is to develop practical skills for systems administration, data center operations, and AI infrastructure.

**Progress snapshot: November 23, 2026** — Lab 01 and Lab 02 completed; Lab 03 setup, SSH failure/recovery, and SSH hardening technically completed and documented. Lab 04 remains planned.

## Lab progress

| Lab | Focus | Status | Documentation |
|---|---|---|---|
| 01 | Ollama deployment, GPU verification, storage and benchmarking | Completed | [Lab 01](labs/lab-01-ollama-setup/) |
| 02 | Qwen inference and performance testing | Completed within available hardware limits | [Lab 02](labs/lab-02-qwen-inference/lab-02-qwen-inference.md) |
| 03 | Ubuntu Server VM, SSH administration, controlled failure and recovery, security fundamentals | In progress — Phase 1 and Phase 2 Experiments 01–02 completed and documented | [Phase 1](labs/lab-03-ubuntu-server/lab-03-phase-1.md) · [Phase 2 — Experiment 01: SSH Recovery](labs/lab-03-ubuntu-server/lab-03-phase-2-experiment-01-ssh-recovery.md) · [Phase 2 — Experiment 02: SSH Hardening](labs/lab-03-ubuntu-server/lab-03-phase-2-experiment-02-ssh-hardening.md) · [Command reference](labs/lab-03-ubuntu-server/lab-03-command-reference.md) |
| 04 | Infrastructure monitoring and dashboard-building (proposed scope) | Planned — not started | — |

## Latest completed activity: Lab 03 — Phase 2, Experiment 02: SSH Hardening

**Completed and documented:**
- Installed Ubuntu Server 24.04.5 LTS in VirtualBox, configured storage and NAT networking, and established SSH access from Windows PowerShell.
- Captured a clean-install VM snapshot for recovery.
- Performed a controlled SSH connectivity failure-and-recovery exercise, preserving the troubleshooting commands and observed results.

**SSH hardening verified:** Created a pre-hardening snapshot and configuration backups; configured ED25519 key login; added a dedicated early-loading SSH server hardening snippet; passed syntax and effective-policy checks; reloaded the service; confirmed a fresh key-based login and rejection of password-only SSH access; reviewed service logs. Rollback steps are documented but were not executed. See the [technical report](labs/lab-03-ubuntu-server/lab-03-phase-2-experiment-02-ssh-hardening.md) and [plain-English lesson](concepts-explained/05-ssh-hardening-explained.md).\n\n**Next work:** Review documentation and decide the next scoped Lab 03 activity. No additional experiment is claimed complete.

## Concepts explained

Beginner-friendly explanations accompany the technical labs. See the [Concepts Explained](concepts-explained/) section for the published lessons and index.

## Technologies

Windows, PowerShell, Ollama, local LLMs, NVIDIA GPU acceleration, Oracle VirtualBox, Ubuntu Server, Linux CLI, SSH, LVM, Git and GitHub.

## Working principles

**Build → Test → Understand → Document → Verify**

**Identify → Explain → Recommend → Document**

Each lab records its objectives, environment, commands, actual observations, troubleshooting, operational implications, and lessons learned. This repository documents hands-on experiments, not claimed production deployments. Planned work is clearly distinguished from completed results.
