# Lab 03 — Command Reference

Commands are grouped by where they run. **Do not run a Windows PowerShell command inside Ubuntu or vice versa.** Outputs are recorded where observed; this is a study reference, not a script to execute all at once.


## Command categories — quick study index

A command may serve more than one purpose. **Always check the environment and risk label before running it.** The detailed examples and observed results are below.

| Category | What it helps you do | Examples | Where to study |
| --- | --- | --- | --- |
| Diagnostics **[READ]** | Inspect the current system without changing its configuration | `hostnamectl`, `lsblk`, `ss -tlnp`, `systemctl status` | Initial setup; sections B, C, H |
| Configuration **[CHANGE]** | Create files or modify settings | `mkdir -p`, `cp -a`, `tee`, `mv`, `chmod` | Sections D, E, F |
| Verification **[TEST]** | Check whether the intended result occurred | `sshd -t`, `sshd -T`, `cmp`, `diff -r`, fresh `ssh` login | Sections D, F, G |
| Troubleshooting **[READ]** | Find why something failed | `journalctl`, `ls -la`, `ssh -v`, `systemctl --failed` | Sections B, H, J |
| Recovery **[RECOVERY]** | Restore service or reverse a change | `systemctl start ssh.socket`, disable the hardening snippet | Sections B, I; Experiment 01 |
| Security **[READ/CHANGE/TEST]** | Manage authentication and access | `ssh-keygen`, `authorized_keys`, `chmod 700`, `PasswordAuthentication no` | Sections C–G |

**Safety note:** `[READ]` means the example is intended for inspection, not that every command with the same name is harmless. `systemctl stop` is disruptive; `systemctl status` is not. Recovery examples in section I were **documented but not executed** in Experiment 02.

## Windows PowerShell — Host diagnostics
```powershell
Get-CimInstance Win32_Processor |
  Select-Object Name, VirtualizationFirmwareEnabled, VMMonitorModeExtensions

systeminfo | findstr /i "hypervisor virtualization"

Get-WindowsOptionalFeature -Online |
  Where-Object { $_.FeatureName -match 'Hyper-V|VirtualMachinePlatform|HypervisorPlatform' } |
  Select-Object FeatureName, State

Get-WindowsOptionalFeature -Online -FeatureName HypervisorPlatform |
  Select-Object FeatureName, State
```
**Purpose:** Inspect CPU virtualization reporting and Windows hypervisor features. The initial CIM flags showed `False`, while Task Manager displayed virtualization enabled and `systeminfo` reported an active hypervisor. Do not infer virtualization is unavailable from those CIM flags alone.

```powershell
& "C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" --version
```
**Observed:** `7.2.22r175245`.

```powershell
Get-Volume -DriveLetter C,E |
  Select-Object DriveLetter, FileSystem,
    @{Name="FreeGB";Expression={[math]::Round($_.SizeRemaining/1GB,2)}},
    @{Name="TotalGB";Expression={[math]::Round($_.Size/1GB,2)}}
```
**Purpose:** Check drive format, available capacity, and total capacity before storing VM files.

## Ubuntu — Identity and hardware
```bash
hostnamectl
hostnamectl --static
free -h
lsblk
```
- `hostnamectl`: OS version, kernel, virtualization vendor, and hostname.
- `hostnamectl --static`: configured hostname only.
- `free -h`: RAM and swap usage in human-readable units.
- `lsblk`: block devices, partitions, logical volumes, and mount points.

**Actual typo encountered:** `hostnamect1` (digit one) is not `hostnamectl` (lowercase L).

## Ubuntu — Network tests
```bash
ip -br addr
ping -c 4 1.1.1.1
```
- `ip -br addr`: concise interface state and IP addresses.
- `ping -c 4`: send four ICMP echo requests; initial test had 0% packet loss. IP-only ping tests reachability, not DNS resolution.

## Ubuntu — SSH service and socket
```bash
sudo systemctl status ssh
systemctl status ssh.socket
systemctl is-active ssh.socket
```
- `ssh.service` may be inactive because the listening socket activates it on demand.
- `ssh.socket` was active and listening on port 22.
- Press `q` to exit a paged `systemctl status` view.

## Windows PowerShell — SSH connection
Configure VirtualBox **Adapter 1 → NAT → Port Forwarding** first:
- Protocol TCP
- Host IP `127.0.0.1`
- Host port `2222`
- Guest IP `10.0.2.15`
- Guest port `22`

```powershell
ssh -p 2222 labadmin@127.0.0.1
```
**Observed:** Successful login to Ubuntu 24.04.5 LTS. For first connection, compare the displayed host-key fingerprint to the fingerprint from the VM console before accepting. Do not publish passwords or private keys.

On Ubuntu, inspect the ED25519 host key fingerprint with:
```bash
sudo ssh-keygen -lf /etc/ssh/ssh_host_ed25519_key.pub
```

## Ubuntu — Hostname correction
```bash
sudo hostnamectl set-hostname lab03-ubuntu
cat /etc/hosts
hostnamectl --static
```
**Observed:** `/etc/hosts` contained `127.0.1.1 lab03-ubuntu`, and the static hostname was `lab03-ubuntu`. An already-open shell may still show the old hostname in its prompt until reconnection.

## Ubuntu — Package maintenance
```bash
sudo apt update
sudo apt upgrade
```
- `apt update` refreshes package indexes.
- `apt upgrade` upgrades installed packages; review the transaction before confirming.
- This is **not** the same as `do-release-upgrade` (which was not run).

```bash
if [ -f /var/run/reboot-required ]; then
    echo "Reboot required"
else
    echo "No reboot required"
fi
```
**Observed:** No reboot required after updates.

```bash
sudo dpkg --audit
```
**Observed:** No output; no incomplete package configuration reported.

## Ubuntu — Safe shutdown before snapshot
```bash
sudo shutdown -h now
```
**Purpose:** Halt the guest cleanly; SSH disconnects. Wait until VirtualBox shows **Powered Off**, then take the snapshot through the VirtualBox Snapshots view.

**Snapshot name:** `Lab03-Baseline-Clean-Install`.

## Learning notes
- `sudo` runs a command with elevated privileges; Ubuntu may request the account password.
- Linux commands are case-sensitive and character-sensitive.
- An SSH connection is a practical way to administer a headless Linux VM and copy/paste long commands.
- A snapshot is not a standalone backup.
- Future intentional failures should be isolated to this VM and followed by a documented recovery.

---

## Lab 03 Phase 2 — Command study guide (Experiments 01 and 02)

**Scope:** Experiment 01 intentionally interrupted and restored SSH; Experiment 02 configured and verified SSH key-only login. These are **individual reference commands, not one script**. Use a VirtualBox console for disruptive changes. Commands marked **[CHANGE]** modify state; **[DISRUPTIVE]** may lock out remote access; **[READ]** only inspect; **[TEST]** verify behavior. Some recovery examples are documented procedures **not executed** in Experiment 02.

### A. How to read the shell

| Element | Meaning |
| --- | --- |
| \`sudo\` | Run as administrator/root; may request your Ubuntu account password |
| \`~\` | Current user's home directory; for \`labadmin\`, usually \`/home/labadmin\` |
| \`/\` | Linux directory separator; \`/etc/ssh/\` contains SSH configuration |
| \`-l\` / \`-a\` / \`-d\` / \`-h\` | Common \`ls\` flags: long listing / include hidden / show directory itself / human-readable sizes |
| \`|\` | Send one command's output into the next command |
| \`>\` / \`>>\` | Overwrite / append redirected output (shell handles redirection) |
| \`&&\` | Run the next command only if the previous command succeeded |
| \`$?\` | Exit status of the most recent command **in that same shell**; \`0\` generally means success |
| \`#\` | Comment in shell scripts and most SSH configuration files |
| \`*.conf\` | Filename pattern matching files ending in \`.conf\` |
| \`Ctrl+C\` | Interrupt a running command; \`q\` quits many pagers |
| \`-t\` / \`-T\` with \`sshd\` | Test configuration syntax / display effective settings |
| \`-i\` / \`-p\` with \`ssh\` | Choose a private key / specify the client destination port |

**Remember:** Options depend on the command. \`-t\` in \`sshd\` is not necessarily the same as \`-t\` in another program. Read \`man <command>\` or \`<command> --help\` when unsure.

### B. SSH service failure and recovery — Ubuntu

Run in the **VirtualBox guest console**, with an independent recovery path available.

\`\`\`bash
# [READ] Current service/socket state
systemctl status ssh.service
systemctl status ssh.socket
systemctl is-active ssh.socket
systemctl --failed

# [READ] TCP listeners (22 is the guest port, not Windows host port 2222)
sudo ss -tlnp | grep ':22'

# [DISRUPTIVE] Experiment 01 only: deliberately stop SSH access
sudo systemctl stop ssh.socket
sudo systemctl stop ssh.service

# [CHANGE] Restore socket-based access from VM console
sudo systemctl start ssh.socket

# [READ] Verify listener and socket after recovery
systemctl is-active ssh.socket
sudo ss -tlnp | grep ':22'
systemctl --failed
\`\`\`

**Observed in Experiment 01:** Socket became \`inactive\`, listener disappeared, a new Windows SSH connection failed, and starting \`ssh.socket\` restored new logins without reboot. **Do not stop SSH remotely when it is your only access path.**

\`\`\`powershell
# [TEST] Windows PowerShell: new connection, with 5-second connect timeout
ssh -o ConnectTimeout=5 -p 2222 labadmin@127.0.0.1
\`\`\`

### C. SSH configuration discovery — Ubuntu

\`\`\`bash
# [READ] Main SERVER configuration, not /etc/ssh/ssh_config (client file)
sudo cat /etc/ssh/sshd_config

# [READ] Included configuration snippets
sudo ls -la /etc/ssh/sshd_config.d/
sudo cat /etc/ssh/sshd_config.d/50-cloud-init.conf

# [READ] Syntax and effective policy
sudo sshd -t
sudo sshd -T
sudo sshd -T -C user=labadmin,host=localhost,addr=10.0.2.2

# [READ] Show lines containing settings; optional learning aid
sudo grep -nEi '^[[:space:]]*[^#[:space:]].*(Include|Match|PermitRootLogin|PasswordAuthentication|PubkeyAuthentication|KbdInteractiveAuthentication|UsePAM)' /etc/ssh/sshd_config
\`\`\`

**Observed:** Main server file includes \`/etc/ssh/sshd_config.d/*.conf\`. The \`50-cloud-init.conf\` snippet contained \`PasswordAuthentication yes\`. Before hardening, effective policy allowed password authentication. **Configuration order matters:** for most directives OpenSSH uses the first value obtained. A filename starting \`01-\` sorts before \`50-\`.

**Important correction:** \`/etc/ssh/ssh_config\` is the **client** configuration, while \`/etc/ssh/sshd_config\` is the **server** configuration. A missing path may simply be a typo or the wrong filename.

### D. Backup, listing and comparison — Ubuntu

\`\`\`bash
# [READ] Inspect the backup directory itself versus its contents
sudo ls -ld /root/ssh-hardening-backup
sudo ls -la /root/ssh-hardening-backup

# [CHANGE] Example of backing up the current SSH files (before editing)
sudo mkdir -p /root/ssh-hardening-backup
sudo chmod 700 /root/ssh-hardening-backup
sudo cp -a /etc/ssh/sshd_config /root/ssh-hardening-backup/
sudo cp -a /etc/ssh/sshd_config.d /root/ssh-hardening-backup/

# [READ] Compare file and directory copies
sudo cmp /etc/ssh/sshd_config /root/ssh-hardening-backup/sshd_config
echo $?
sudo diff -r /etc/ssh/sshd_config.d /root/ssh-hardening-backup/sshd_config.d
echo $?
\`\`\`

**Observed:** File and directory comparisons returned exit status \`0\` when the copies matched. The original exercise also included correcting a mistyped backup-directory name using \`mv\` and restricting its permissions.

| Command | Plain English |
| --- | --- |
| \`mkdir -p\` | Create directory, including missing parent directories |
| \`chmod 700\` | Owner has full access; group and others have none |
| \`cp -a\` | Copy while preserving attributes where possible |
| \`mv -n\` | Move/rename without overwriting an existing destination |
| \`cmp\` | Compare two files byte by byte; usually silent when identical |
| \`diff -r\` | Compare directories recursively |
| \`ls -ld folder\` | Show the folder's own metadata |
| \`ls -la folder\` | Show entries inside the folder, including hidden entries |

**Study tip:** Run \`echo $?\` **immediately after** the command you are checking. In Windows PowerShell, \`$?\` has different semantics, and \`$LASTEXITCODE\` is often used for native program exit codes.

### E. Generate and install an ED25519 login key

**Windows PowerShell — [CHANGE]:** The following is a reusable example. A key pair was generated during Experiment 02. Protect the private file and its passphrase.

\`\`\`powershell
ssh-keygen -t ed25519 -f "$env:USERPROFILE\.ssh\lab03_ed25519" -C "lab03-ssh-hardening"
Get-ChildItem "$env:USERPROFILE\.ssh"
\`\`\`

**Public versus private:** \`lab03_ed25519\` is the private key and **must stay private**. \`lab03_ed25519.pub\` is the public key, safe to install in the Ubuntu account's \`authorized_keys\`. \`known_hosts\` stores previously accepted **server host keys**, not your user login key.

**Windows PowerShell — [CHANGE]:** The command used to append the public key to Ubuntu:

\`\`\`powershell
Get-Content "$env:USERPROFILE\.ssh\lab03_ed25519.pub" | ssh -p 2222 labadmin@127.0.0.1 'umask 077; mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys'
\`\`\`

The command prompts for the existing Ubuntu password during initial setup. **Never pipe the private key.** If rerun, \`>>\` may add a duplicate public-key line; inspect first.

**Ubuntu — [READ]:**

\`\`\`bash
ls -ld ~/.ssh
ls -l ~/.ssh/authorized_keys
\`\`\`

**Observed:** \`authorized_keys\` existed under \`/home/labadmin/.ssh/\`. A key login was verified before disabling password authentication.

**Windows PowerShell — [TEST]:**

\`\`\`powershell
ssh -v -i "$env:USERPROFILE\.ssh\lab03_ed25519" -o IdentitiesOnly=yes -o PreferredAuthentications=publickey -o PasswordAuthentication=no -o KbdInteractiveAuthentication=no -p 2222 labadmin@127.0.0.1
\`\`\`

**Observed:** Verbose SSH output included \`Server accepts key\` and \`Authenticated ... using "publickey"\`. The passphrase prompt unlocks the **local private key**; it is not a server account-password prompt.

### F. Stage and apply SSH hardening — Ubuntu

**[CHANGE]** This is the experiment's configuration, not a general one-size-fits-all production policy. The staging filename ending \`.pending\` is deliberately not loaded by \`*.conf\`.

\`\`\`bash
sudo tee /etc/ssh/sshd_config.d/01-lab-hardening.conf.pending > /dev/null <<'EOF'
PubkeyAuthentication yes
PasswordAuthentication no
KbdInteractiveAuthentication no
PermitRootLogin no
X11Forwarding no
EOF

# [READ] Verify staged contents and standalone syntax
sudo cat /etc/ssh/sshd_config.d/01-lab-hardening.conf.pending
sudo sshd -t -f /etc/ssh/sshd_config.d/01-lab-hardening.conf.pending

# [CHANGE] Activate the snippet only after validating
sudo mv -n /etc/ssh/sshd_config.d/01-lab-hardening.conf.pending /etc/ssh/sshd_config.d/01-lab-hardening.conf

# [READ] Validate combined configuration and effective policy
sudo sshd -t
sudo sshd -T -C user=labadmin,host=localhost,addr=10.0.2.2

# [CHANGE] Apply without closing the existing SSH session
sudo systemctl reload ssh

# [READ] Verify running service
sudo systemctl is-active ssh
\`\`\`

**Observed effective values:** \`pubkeyauthentication yes\`, \`passwordauthentication no\`, \`kbdinteractiveauthentication no\`, \`permitrootlogin no\`, \`x11forwarding no\`, \`usepam yes\`. Combined syntax passed and service remained \`active\`. **\`sshd -t -f\` on a snippet tests its syntax in isolation, not the combined policy; always run the combined check too.**

### G. Test allowed and denied logins — Windows PowerShell

**[TEST] Positive test:** Open a **new** PowerShell tab; leave the old working SSH session open.

\`\`\`powershell
ssh -i "$env:USERPROFILE\.ssh\lab03_ed25519" -o IdentitiesOnly=yes -o PreferredAuthentications=publickey -o PasswordAuthentication=no -o KbdInteractiveAuthentication=no -p 2222 labadmin@127.0.0.1
\`\`\`

**Observed:** New session opened using the ED25519 key.

**[TEST] Negative test:** Force the client to attempt password-only login without prompting:

\`\`\`powershell
ssh -o PubkeyAuthentication=no -o PreferredAuthentications=password -o KbdInteractiveAuthentication=no -o BatchMode=yes -p 2222 labadmin@127.0.0.1
\`\`\`

**Observed:** \`Permission denied (publickey).\` That is the **expected** result for this test, not a failure of the hardening work.

### H. Logs and troubleshooting — Ubuntu

\`\`\`bash
# [READ] Recent SSH service logs
sudo journalctl -u ssh --since "10 minutes ago" --no-pager

# [READ] Alternative: most recent 50 service entries
sudo journalctl -u ssh -n 50 --no-pager

# [READ] Check configuration and service
sudo sshd -t
sudo systemctl is-active ssh
sudo systemctl status ssh
\`\`\`

**Observed:** Reload at 00:46:41–42 UTC, accepted public-key login at 00:47:16 UTC, and a pre-authentication connection reset at 00:49:08 UTC. Logs corroborate what happened; a single \`preauth\` message alone does not prove a particular authentication policy.

### I. Recovery reference — NOT executed in Experiment 02

**[CHANGE / RECOVERY]** Use the VirtualBox console, not the only SSH session. To disable the experiment's snippet and restore the prior configuration precedence:

\`\`\`bash
sudo mv /etc/ssh/sshd_config.d/01-lab-hardening.conf /etc/ssh/sshd_config.d/01-lab-hardening.conf.disabled
sudo sshd -t
sudo systemctl reload ssh
sudo sshd -T -C user=labadmin,host=localhost,addr=10.0.2.2
\`\`\`

This can restore password authentication through \`50-cloud-init.conf\`. It is a **rollback**, not the preferred final security state. The VM snapshot and root-owned backups are additional recovery options. **No rollback was necessary or performed.**

### J. Common mistakes from this lab

| Symptom | Likely lesson / next check |
| --- | --- |
| \`No such file or directory\` | Confirm spelling, absolute path and filename with \`ls\`; the backup directory was mistyped during this lab |
| \`cmp: invalid --ignore-initial value\` | Check spacing, path and command boundaries; run \`cmp\` and \`echo $?\` separately |
| \`grep: /etc/ssh/sshd_config: No such file or directory\` | Confirm \`sshd_config\` (server) versus \`ssh_config\` (client), and avoid long manually typed patterns |
| \`ls -ld\` only shows one folder | Use \`ls -la folder/\` to list its contents |
| \`Permission denied (publickey)\` in the negative test | **Expected:** password authentication was disabled |
| Existing SSH session still works after a change | It does **not** prove a fresh login will work; open a second session |
| \`ssh -v\` shows missing \`known_hosts2\` | Often an optional-file lookup; inspect the final authentication outcome |
| Login closes unexpectedly | Inspect \`ssh -v\`, \`journalctl\`, and key/authorized_keys permissions before changing security policy |

### K. Quick memory drills

1. **Where am I running this?** Windows PowerShell or Ubuntu?
2. **Am I reading or changing state?** Look for \`status\`, \`cat\`, \`-T\` versus \`stop\`, \`mv\`, \`tee\`, \`reload\`.
3. **Which port?** Windows NAT host \`2222\` versus Ubuntu guest \`22\`.
4. **Which key?** Private key stays on Windows; public key goes into \`authorized_keys\`; server host fingerprints go into \`known_hosts\`.
5. **How do I verify?** Syntax check → effective policy → service status → **new** positive login → negative login → logs.
6. **How do I recover?** Keep VM console and snapshot available before any potentially disruptive change.

### L. Further study (not completed in this lab)

Learn reusable shell scripts, PowerShell functions, SSH client profiles (\`~/.ssh/config\`), key management, log filtering, and safe automation **after** you can explain the individual commands. This reference intentionally does not claim those topics were tested.

**Source experiments:** [Experiment 01 — SSH Recovery](lab-03-phase-2-experiment-01-ssh-recovery.md) · [Experiment 02 — SSH Hardening](lab-03-phase-2-experiment-02-ssh-hardening.md)

*AI-assisted reference compiled from human-executed lab commands, screenshots, and verified experiment reports. Examples, observations, and unexecuted recovery steps are labeled separately.*
