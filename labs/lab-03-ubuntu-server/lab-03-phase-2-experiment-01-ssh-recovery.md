# Lab 03 — Phase 2, Experiment 01: SSH Service Failure and Recovery

**Date:** November 20–21, 2026  
**Result:** Completed — SSH remote access restored without VM reboot or snapshot rollback.  
**Environment:** Ubuntu Server 24.04.5 LTS in Oracle VirtualBox; NAT port forwarding Windows `127.0.0.1:2222` → guest TCP `22`.  
**Recovery checkpoint:** `Lab03-Phase2-PreFailure`.

> Educational, isolated VM exercise. Commands that stop SSH can disconnect remote administration. Ensure independent console access before trying them on another machine. No passwords, keys, or other secrets are recorded here.

## Objective

Simulate an SSH outage, collect evidence on both the client and guest, restore service from the VM console, and verify that new remote logins work. Follow **Identify → Explain → Recommend → Document** and **Build → Test → Understand → Document → Verify**.

## Baseline (observed)

| Check | Evidence |
| --- | --- |
| `systemctl is-active ssh.socket` | `active` |
| `sudo ss -tlnp \| grep ':22'` | Port 22 listening |
| `systemctl --failed` | `0 loaded units listed.` |
| `uptime` | Up 3 minutes; load averages 0.09, 0.11, 0.05 |
| Remote SSH | Working before test |

## Controlled failure

**Executed in VirtualBox guest console, not in the SSH session:**

```bash
sudo systemctl stop ssh.socket
sudo systemctl stop ssh.service
systemctl is-active ssh.socket
sudo ss -tlnp | grep ':22'
```

Observed: socket returned `inactive`; the port-listener check returned no output. Stopping both units matters because this Ubuntu installation uses **systemd socket activation**. A previously established SSH session may persist even when new connections fail.

**New connection attempted from Windows PowerShell:**

```powershell
ssh -o ConnectTimeout=5 -p 2222 labadmin@127.0.0.1
```

Observed error:

```text
kex_exchange_identification: read: Unknown error
banner exchange: Connection to 127.0.0.1 port 2222: Unknown error
```

This is evidence of failed SSH negotiation, not by itself a unique diagnosis. Together with the inactive guest socket and missing listener, it supports the intentionally stopped SSH endpoint as the cause of this lab outage.

## Targeted recovery

**From the VirtualBox guest console:**

```bash
sudo systemctl start ssh.socket
systemctl is-active ssh.socket
sudo ss -tlnp | grep ':22'
```

**From Windows PowerShell, open a new session:**

```powershell
ssh -o ConnectTimeout=5 -p 2222 labadmin@127.0.0.1
```

**After login, inside Ubuntu:**

```bash
systemctl --failed
systemctl is-active ssh.socket
sudo ss -tlnp | grep ':22'
```

### Recovery evidence (observed)

- New SSH connection succeeded; Ubuntu login prompt appeared.
- `systemctl --failed` reported `0 loaded units listed.`
- `systemctl is-active ssh.socket` returned `active`.
- `ss` showed TCP listeners on `0.0.0.0:22` and `[::]:22`, associated with `sshd` and `systemd`.
- No guest reboot or snapshot rollback was needed.

## Operational interpretation

**Identify:** Remote SSH handshake failed, while the guest remained accessible through the hypervisor console.  
**Explain:** In this deliberately induced scenario, stopping the socket and service removed the SSH listener. The guest OS itself was not shown to have failed.  
**Recommend:** Confirm service/socket state and port binding before changing firewall, credentials, or rebooting. Restore the intended socket using an independent console.  
**Document:** Preserve commands, symptoms, observed output, and post-recovery evidence.

## Reusable SSH quick reference

| Purpose | Command |
| --- | --- |
| Connect using default SSH port | `ssh username@host` |
| Connect using custom client port | `ssh -p 2222 username@host` |
| Limit connection timeout | `ssh -o ConnectTimeout=5 -p 2222 username@host` |
| Inspect socket status | `systemctl status ssh.socket` |
| Check socket active state | `systemctl is-active ssh.socket` |
| Inspect SSH service | `systemctl status ssh.service` |
| Find listening SSH port | `sudo ss -tlnp \| grep ':22'` |
| Check failed systemd units | `systemctl --failed` |
| Review SSH logs | `sudo journalctl -u ssh.socket -u ssh.service --no-pager -n 50` |
| Restore socket (this VM) | `sudo systemctl start ssh.socket` |

**Port distinction:** The Windows client uses port **2222** because VirtualBox forwards it to guest port **22**. A different server or device may use port 22 directly, another forwarded port, key-based authentication, or a different service manager. Verify the target's configuration before applying commands. `systemctl` is appropriate for systemd-based Linux hosts, not universally for all devices.

**Scope:** These stop/start commands change current runtime state; they do not permanently disable or enable SSH on boot. Do not run disruptive SSH commands over the only available remote access path.

## Lessons learned

1. A remote service outage is not necessarily a host outage.
2. An SSH handshake error alone does not identify the root cause; corroborate with guest-side evidence.
3. Socket activation means both `ssh.socket` and `ssh.service` can matter.
4. Use a console or other out-of-band recovery route before deliberately stopping remote access.
5. Restore the smallest failed component and verify with a **new** client connection.

**Experiment status:** Complete. Further Lab 03 Phase 2 experiments are not claimed as completed.
