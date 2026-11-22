# Local LLM Infrastructure Lab

**Author:** Kelli Love | EXEcode765

Hands-on portfolio documenting local LLM deployment, GPU inference testing, Linux server virtualization, remote administration, and infrastructure troubleshooting. The goal is to develop practical skills for systems administration, data center operations, and AI infrastructure.

**Progress snapshot: November 21, 2026** — Lab 01 and Lab 02 completed; Lab 03 setup and SSH failure/recovery work completed; SSH hardening is **next, not yet performed**.

## Lab progress

| Lab | Focus | Status | Documentation |
|---|---|---|---|
| 01 | Ollama deployment, GPU verification, storage and benchmarking | Completed | [Lab 01](labs/lab-01-ollama-setup/) |
| 02 | Qwen inference and performance testing | Completed within available hardware limits | [Lab 02](labs/lab-02-qwen-inference/lab-02-qwen-inference.md) |
| 03 | Ubuntu Server VM, SSH administration, controlled failure and recovery, security fundamentals | In progress — initial setup and SSH recovery documented; SSH hardening next | [Phase 1](labs/lab-03-ubuntu-server/lab-03-phase-1.md) · [Phase 2 — SSH Recovery](labs/lab-03-ubuntu-server/lab-03-phase-2-experiment-01-ssh-recovery.md) · [Command reference](labs/lab-03-ubuntu-server/lab-03-command-reference.md) |
| 04 | Infrastructure monitoring and dashboard-building (proposed scope) | Planned — not started | — |

## Current focus: Lab 03 — SSH hardening (Experiment 03)

**Completed and documented:**
- Installed Ubuntu Server 24.04.5 LTS in VirtualBox, configured storage and NAT networking, and established SSH access from Windows PowerShell.
- Captured a clean-install VM snapshot for recovery.
- Performed a controlled SSH connectivity failure-and-recovery exercise, preserving the troubleshooting commands and observed results.

**Next experiment — not started:** Establish the current SSH security baseline, configure and test key-based authentication, evaluate secure SSH settings, validate access and recovery safeguards, and document observed results. No SSH-hardening outcomes are claimed yet.

## Concepts explained

Beginner-friendly explanations accompany the technical labs. See the [Concepts Explained](concepts-explained/) section for the published lessons and index.

## Technologies

Windows, PowerShell, Ollama, local LLMs, NVIDIA GPU acceleration, Oracle VirtualBox, Ubuntu Server, Linux CLI, SSH, LVM, Git and GitHub.

## Working principles

**Build → Test → Understand → Document → Verify**

**Identify → Explain → Recommend → Document**

Each lab records its objectives, environment, commands, actual observations, troubleshooting, operational implications, and lessons learned. This repository documents hands-on experiments, not claimed production deployments. Planned work is clearly distinguished from completed results.
