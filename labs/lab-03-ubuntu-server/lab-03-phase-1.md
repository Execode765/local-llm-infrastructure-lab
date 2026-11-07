# Lab 03 — Ubuntu Server Virtualization and Recovery (Phase 1)

**Date:** 2026-11-07  
**Status:** Phase 1 complete — baseline snapshot captured; failure/recovery experiments not yet started.

## Objective
Build an isolated Ubuntu Server virtual machine on a Windows laptop, establish headless remote administration, verify the baseline, and preserve a known-good recovery point before deliberately testing infrastructure failures.

## Host and virtualization environment
- Host: ASUS ROG Strix G814JI, Windows, Intel Core i9-13980HX, RTX 4070 Laptop GPU, approximately 64 GB system RAM.
- CPU configuration during this lab: 4 P-cores / 0 E-cores (reduced in ASUS settings for Studio One stability). Windows reported 8 logical processors.
- Virtualization: Task Manager showed **Enabled**; Windows reported a running hypervisor and virtualization-based security. `VirtualMachinePlatform` and `HypervisorPlatform` were enabled.
- Oracle VirtualBox **7.2.22 r175245**, installed in default `C:\Program Files\Oracle\VirtualBox` location after an initial external-drive installation was changed.
- External SSD E: used for ISO and VM files, NTFS, about 465.76 GB total / 456.43 GB free at pre-install check. Host C: about 3,700.32 GB total / 2,788.96 GB free at that check.
- ISO: `E:\ISO Images\ubuntu-24.04.5-live-server-amd64.iso`.
- VM: `Local-LLM-Lab03-Ubuntu`; folder `E:\Virtual Machines\Local-LLM-Lab03-Ubuntu`.

## VM configuration
| Setting | Value |
|---|---|
| Guest OS | Ubuntu Server 24.04.5 LTS, x86_64 |
| Virtual CPUs | 2 |
| RAM | 4096 MB |
| Virtual disk | 40 GB, dynamically allocated VDI |
| Boot | Legacy BIOS (EFI disabled) |
| Networking | VirtualBox NAT |
| Ubuntu account | `labadmin` |
| Correct hostname | `lab03-ubuntu` |
| SSH | OpenSSH installed; systemd socket activation |
| Windows → VM SSH | Host `127.0.0.1:2222` → guest `10.0.2.15:22` |

**Security:** Credentials, passwords, private keys, and authentication secrets are intentionally omitted. Do not commit these to GitHub.

## Installation and troubleshooting notes
1. VirtualBox initially misidentified the Ubuntu 24.04.5 ISO as Ubuntu 24.10; the ISO filename and later `hostnamectl` output confirmed the installed guest is **Ubuntu 24.04.5 LTS**.
2. Unattended installation was disabled to use the interactive Ubuntu Server installer.
3. Storage was provisioned with LVM. The virtual disk is 40 GB; `/boot` is 2 GB; the root logical volume is about 19 GB, with roughly 19 GB unallocated within the volume group for later expansion exercises.
4. The installer initially stalled at `Failed unmounting cdrom.mount` and requested removal of installation media. After powering off, the ISO was detached from the virtual optical drive; the guest subsequently booted from its virtual disk.
5. The VM console was inconvenient for copying commands; using Windows PowerShell over SSH provided a normal copy/paste workflow.
6. Hostname was initially entered as `lab03-ubantu`, corrected with `hostnamectl`; `/etc/hosts` was checked and displayed `127.0.1.1 lab03-ubuntu`.
7. The package upgrade installed 28 available updates (no removals reported); a nonfatal AppArmor post-install script warning was observed during the upgrade. Later `dpkg --audit` produced no output.
8. SSH `ssh.service` showed `inactive (dead)`, but `ssh.socket` was **active (listening)**. This is expected for socket activation; the Windows SSH login subsequently succeeded.
9. Ubuntu advertised a newer OS release, but **no distribution upgrade was performed**; the lab remains on 24.04 LTS.

## Verification evidence
| Check | Observation |
|---|---|
| `hostnamectl` | Ubuntu 24.04.5 LTS, kernel `6.8.0-149-generic`, Oracle virtualization |
| `free -h` | ~3.8 GiB RAM, ~428 MiB used at initial check; ~3.8 GiB swap |
| `lsblk` | `sda` 40G; `sda1` 1M; `sda2` 2G mounted `/boot`; `sda3` 38G; `ubuntu--vg-ubuntu--lv` 19G mounted `/` |
| `ip -br addr` | `enp0s3 UP 10.0.2.15/24` |
| `ping -c 4 1.1.1.1` | 4/4 replies, 0% packet loss, average RTT ~22.724 ms |
| `systemctl status ssh.socket` | Active (listening), TCP port 22 |
| Windows `ssh -p 2222 labadmin@127.0.0.1` | Successful interactive Ubuntu shell |
| `sudo dpkg --audit` | No output (no audited package problems) |
| `systemctl is-active ssh.socket` | `active` |
| Snapshot | `Lab03-Baseline-Clean-Install`, confirmed in VirtualBox Manager while powered off |

## Recovery checkpoint
**Snapshot:** `Lab03-Baseline-Clean-Install`  
**Captured:** 2026-11-07, after installation, package updates, hostname correction, and SSH verification.

A VirtualBox snapshot is a rollback checkpoint, **not** an independent backup. The VM and snapshot files reside on the external E: SSD; that drive must remain connected and healthy.

## Next phase — Not yet executed
- Controlled SSH failure, console diagnosis, and recovery.
- Network configuration failure/recovery.
- Linux service and log troubleshooting.
- Optional LVM expansion and additional fault scenarios after safeguarding the baseline.

See [Lab 03 command reference](lab-03-command-reference.md) for the actual commands used and their purposes.
