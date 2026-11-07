# Lab 03 — Command Reference

Commands are grouped by where they run. **Do not run a Windows PowerShell command inside Ubuntu or vice versa.** Outputs are recorded where observed; this is a study reference, not a script to execute all at once.

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
