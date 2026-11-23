# Lab 03 — Phase 2, Experiment 02: SSH Hardening

**Date:** November 22–23, 2026 (Ubuntu VM logs in UTC)  
**Result:** Technical implementation and positive/negative authentication tests completed; documentation recorded afterward.  
**Environment:** Ubuntu Server 24.04.5 LTS, Oracle VirtualBox NAT; Windows PowerShell 5.1 OpenSSH client; Windows `127.0.0.1:2222` forwarded to guest TCP `22`.  
**Recovery checkpoint:** `Lab03-Phase2-PreHardening`.

> Isolated educational VM. Keep a working SSH session and independent VirtualBox console available before changing authentication. Never publish private keys, passphrases, or credential contents.

## Objective

Reduce SSH password-based access while preserving verified ED25519 key login. Validate syntax, configuration precedence, effective policy, live authentication behavior, and recovery options.

## Baseline and safeguards

- `ssh.service` running and `ssh.socket` active; initial `sshd -T` reported `passwordauthentication yes`, `pubkeyauthentication yes`, `kbdinteractiveauthentication no`, and `permitrootlogin without-password`.
- Backed up `/etc/ssh/sshd_config` and `/etc/ssh/sshd_config.d/` under root-owned `/root/ssh-hardening-backup/`, with permissions restricted to root. Verified copied main file with `cmp` and snippet directory with `diff -r`; both returned exit status 0.
- Created VirtualBox snapshot `Lab03-Phase2-PreHardening` before editing.
- Generated a passphrase-protected ED25519 key pair on Windows (`~/.ssh/lab03_ed25519`); installed only its **public** key into Ubuntu `/home/labadmin/.ssh/authorized_keys`. Confirmed key login with verbose client output: `Authenticated ... using "publickey"`.
- Kept the private key on Windows and maintained VirtualBox console access.

## Configuration investigation

The main **server** file is `/etc/ssh/sshd_config` (not the client file `/etc/ssh/ssh_config`). It includes:

```text
Include /etc/ssh/sshd_config.d/*.conf
```

The included `50-cloud-init.conf` contained:

```text
PasswordAuthentication yes
```

OpenSSH generally uses the first obtained value for a directive. A dedicated `01-lab-hardening.conf` is loaded before `50-cloud-init.conf`, avoiding edits to the cloud-init-managed snippet.

## Change procedure (executed)

Created a staging file not matched by the `*.conf` include:

```bash
sudo tee /etc/ssh/sshd_config.d/01-lab-hardening.conf.pending > /dev/null <<'EOF'
PubkeyAuthentication yes
PasswordAuthentication no
KbdInteractiveAuthentication no
PermitRootLogin no
X11Forwarding no
EOF
```

Read the staged file back, then checked its standalone syntax:

```bash
sudo sshd -t -f /etc/ssh/sshd_config.d/01-lab-hardening.conf.pending
```

The command succeeded. Renamed it to a loaded `.conf` file:

```bash
sudo mv -n /etc/ssh/sshd_config.d/01-lab-hardening.conf.pending /etc/ssh/sshd_config.d/01-lab-hardening.conf
sudo sshd -t
```

The combined syntax check succeeded. Inspected the effective policy for the observed NAT client context:

```bash
sudo sshd -T -C user=labadmin,host=localhost,addr=10.0.2.2
```

Relevant observed values:

```text
usepam yes
permitrootlogin no
pubkeyauthentication yes
passwordauthentication no
kbdinteractiveauthentication no
x11forwarding no
```

Applied the configuration and checked the service:

```bash
sudo systemctl reload ssh
sudo systemctl is-active ssh
```

Observed `active` with no reload error.

## Positive test — key authentication allowed

From a **new Windows PowerShell tab**, with the original SSH session left open:

```powershell
ssh -i "$env:USERPROFILE\.ssh\lab03_ed25519" -o IdentitiesOnly=yes -o PreferredAuthentications=publickey -o PasswordAuthentication=no -o KbdInteractiveAuthentication=no -p 2222 labadmin@127.0.0.1
```

**Observed:** A new `labadmin@lab03-ubuntu:~$` session opened. Server log recorded `Accepted publickey for labadmin` and a PAM session opened at 00:47:16 UTC on November 23.

## Negative test — password-only access denied

From another Windows PowerShell tab:

```powershell
ssh -o PubkeyAuthentication=no -o PreferredAuthentications=password -o KbdInteractiveAuthentication=no -o BatchMode=yes -p 2222 labadmin@127.0.0.1
```

**Observed:**

```text
labadmin@127.0.0.1: Permission denied (publickey).
```

This test intentionally disabled the client's key method. The server offered only public-key authentication; password-only access was unavailable. The log later recorded `Connection reset by authenticating user labadmin ... [preauth]` at 00:49:08 UTC. That log entry is consistent with the unsuccessful attempt but **does not alone prove** the password restriction; the effective policy and client-side result provide the stronger evidence.

## Service-side evidence

```bash
sudo journalctl -u ssh --since "10 minutes ago" --no-pager
```

- 00:46:41–42 UTC: `ssh.service` reloaded; daemon reported listeners on IPv4 and IPv6 port 22.
- 00:47:16 UTC: ED25519 public-key login accepted; PAM session opened.
- 00:49:08 UTC: connection reset during pre-authentication.

## Recovery and rollback plan (documented, not exercised)

**If a new key login fails:** Do not close an existing working SSH session. Use the VirtualBox guest console to inspect `sudo sshd -t`, `sudo sshd -T`, `systemctl status ssh`, and `sudo journalctl -u ssh -n 50 --no-pager`.

**To remove this experiment's hardening snippet from the guest console:**

```bash
sudo mv /etc/ssh/sshd_config.d/01-lab-hardening.conf /etc/ssh/sshd_config.d/01-lab-hardening.conf.disabled
sudo sshd -t
sudo systemctl reload ssh
```

Then test a fresh SSH connection and inspect `sudo sshd -T`. The original `50-cloud-init.conf` may then restore password authentication; this is a **rollback to the previous policy**, not a preferred secure end state. The pre-hardening VirtualBox snapshot and root-owned configuration backups provide additional recovery paths. **No rollback was performed during this experiment.**

## Troubleshooting and operational lessons

- A mistyped directory name caused `No such file or directory`; inspected `/root` and renamed it with `mv` before proceeding.
- `ls -ld` describes a directory itself; `ls -la` lists its contents.
- Entering a filename and shell operators incorrectly can produce misleading `cmp` errors; separating comparison and `echo $?` made verification easier.
- The SSH client config (`ssh_config`, `Host *`) is not the server config (`sshd_config`). Read the correct file.
- A commented setting is not necessarily the effective setting. Inspect included snippets and use `sshd -T` before changing access policy.
- A successful authorized login and a rejected disallowed login test different sides of the security control.
- During final login, Ubuntu reported 42 available package updates, including 16 standard security updates. Package upgrades were **not performed**; maintenance remains separate from this SSH experiment.

## Scope and limitations

The experiment verified key-based access and password-only rejection for the tested `labadmin` connection through VirtualBox NAT. It did not test every possible account, authentication mechanism, or exposure scenario. Direct root login and X11 forwarding were verified through effective settings, not separate live negative tests. This is a home-lab demonstration, not a claim of production compliance.

## Conclusion

**Technical experiment completed and verified.** The running SSH server accepted a fresh ED25519-key login after reload and rejected the password-only client attempt, while remaining active. Recovery was prepared but not needed.

**Build → Test → Understand → Document → Verify**  
**Identify → Explain → Recommend → Document**

*AI-assisted documentation based on human-executed commands and observed outputs. No credentials or private-key contents are included.*
