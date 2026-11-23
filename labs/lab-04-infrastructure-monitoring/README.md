# Lab 04 — Infrastructure Monitoring & Dashboard Building

**Status:** Planned — not started  
**Plan approved:** 2026-11-22  
**Next action:** Experiment 01 — read-only baseline assessment of the existing Ubuntu VM  
**Last verified checkpoint:** Roadmap created; no Lab 04 commands run, services installed, dashboard built, or measurements collected.

## Purpose

Learn to observe, measure, troubleshoot, and explain the health of existing infrastructure with established tools rather than develop a monitoring product. Distinguish a running service from a healthy system, degraded performance, and impending resource exhaustion.

## Environment and constraints

- Available: ASUS ROG Strix Windows laptop (Ollama and VirtualBox host), existing Ubuntu Server 24.04.5 LTS VirtualBox VM with hardened SSH.
- Spare desktops are **not operational** and are **not dependencies**.
- No hardware purchases, paid cloud services, unnecessary host load, unapproved changes to SSH configuration, or public dashboard exposure.
- Evaluate CPU, RAM, disk, and network capacity **before** choosing where each monitoring component runs.
- Commands and observations must be independently verified; do not present proposed steps as tested.

## Progress tracker

| Order | Experiment | Status | Evidence / notes |
|---|---|---|---|
| 01 | [Baseline Assessment & Monitoring Fundamentals](experiment-01-baseline/README.md) | Not started | No measurements collected |
| 02 | [Metrics Collection](experiment-02-metrics-collection/README.md) | Not started | Node Exporter/Prometheus not deployed |
| 03 | [Dashboard Construction](experiment-03-dashboard-construction/README.md) | Not started | Grafana not deployed |
| 04 | [Alerts & Troubleshooting](experiment-04-alerts-troubleshooting/README.md) | Not started | No controlled incidents or alerts tested |
| 05 | [Windows & Local LLM Monitoring](experiment-05-windows-ollama/README.md) | Not started; conditional on resources | No exporter or AI telemetry integration verified |

## Proposed architecture (not deployed)

Ubuntu VM → Node Exporter → Prometheus → Grafana dashboard.

Node Exporter exposes host metrics; Prometheus scrapes and stores time-series data; Grafana queries and displays them. Deployment location of Prometheus and Grafana is **to be decided after Experiment 01**. Other collectors may be evaluated for Windows or Ollama in Experiment 05. Ollama API inference timings are not automatically Prometheus metrics; integration must be tested rather than assumed.

## Resume here

**Current checkpoint: Begin Experiment 01.** Start the existing VM and SSH from Windows PowerShell using the established Lab 03 connection procedure, subject to confirming the current key-based authentication setup. Inside Ubuntu, use read-only commands:

```bash
hostnamectl
lscpu
free -h
df -hT
uptime
ip -br addr
```

Record dated actual outputs (redact identifying network details if needed), idle baseline, relevant host/VM allocations, and any limitations. **Do not install monitoring software yet.** Evaluate available resources and choose component placement before moving to Experiment 02.

After every work session, update: (1) experiment status, (2) commands executed and observed outcomes, (3) unresolved issues, (4) next exact action, (5) date of last verified checkpoint. Keep this file as the authoritative navigation and resumption index.

## Completion and documentation policy

For each experiment record objective, prerequisites, safety/rollback considerations, procedure/command reference, observations, verification criteria, troubleshooting, operational interpretation, and lessons learned. Supporting screenshots/configurations are added only when produced. Plain-English lessons may be added to `concepts-explained/` after technical verification.

**Working principles:** Build → Test → Understand → Document → Verify; Identify → Explain → Recommend → Document.

*Planning documentation only. No Lab 04 implementation or experiment completion is claimed.*
