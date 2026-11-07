# Local LLM Infrastructure Lab

**Author:** Kelli Love | EXEcode765

Hands-on portfolio documenting local LLM deployment, GPU inference testing, Linux server virtualization, and infrastructure troubleshooting. The goal is to build practical skills for systems administration and AI infrastructure operations.

## Lab progress

| Lab | Focus | Status | Documentation |
|---|---|---|---|
| 01 | Ollama deployment, GPU verification, storage and benchmarking | Completed | [Lab 01](labs/lab-01-ollama-setup/) |
| 02 | Qwen inference and performance testing | Completed within available hardware limits | [Lab 02](lab-02-qwen-inference.md) |
| 03 | Ubuntu Server VM, remote administration, failure and recovery | **Phase 1 complete**; fault injection pending | [Phase 1 report](labs/lab-03-ubuntu-server/lab-03-phase-1.md) · [Command reference](labs/lab-03-ubuntu-server/lab-03-command-reference.md) |
| 04 | Physical GPU/server troubleshooting on salvaged hardware | Planned | — |

## Lab 03 milestone (2026-11-07)

Installed Ubuntu Server 24.04.5 LTS in VirtualBox 7.2.22, configured LVM and NAT networking, established SSH access from Windows PowerShell, verified package health, and captured the powered-off recovery snapshot `Lab03-Baseline-Clean-Install`.

Next: controlled failure and recovery experiments, with results and commands documented in the Lab 03 folder.

## Technologies
Windows, PowerShell, Ollama, local LLMs, NVIDIA GPU acceleration, Oracle VirtualBox, Ubuntu Server, Linux CLI, SSH, LVM, Git and GitHub.

## Documentation approach
Each lab records its objectives, environment, commands, observed results, troubleshooting steps, and lessons learned. This repository documents actual experiments, not claimed production deployments.
