# Why Is an SSH Key Safer Than Just a Password?

**Companion:** [Lab 03 — Phase 2, Experiment 02: SSH Hardening](../labs/lab-03-ubuntu-server/lab-03-phase-2-experiment-02-ssh-hardening.md)  
**Status:** Based on completed, verified lab testing (November 22–23, 2026).

## 1. The Big Question

If you manage a Linux computer remotely, how do you let the right person in without leaving ordinary password-based SSH login available?

## 2. Think of It Like…

Imagine a building where the front door used to accept a memorized code. Someone who guesses or steals that code might enter. We changed the remote entrance so that it checks for a matching cryptographic key instead.

The **private key** stays with the administrator; the server stores the matching **public key**. The private key isn't sent to the server during login. In our lab we also protected the private key with a passphrase, which is different from the Linux account password.

This analogy has limits: key security still depends on protecting the private key and the computer storing it.

## 3. What Makes It Work?

- **SSH** (Secure Shell) provides encrypted remote administration.
- **sshd** is the Linux server process accepting incoming SSH connections. **ssh** is the client.
- **Public-key authentication** uses a key pair to prove possession of the private key.
- **Password authentication** lets a remote user attempt to authenticate with an account password. We disabled that SSH method.
- **Keyboard-interactive authentication** is another possible interactive login mechanism; we disabled it too.
- **PAM** can still handle account and session checks even when SSH password authentication is off.
- **Configuration precedence** matters: the main server file includes additional `.conf` files, and the first value obtained for most SSH directives takes effect.

A `#` at the beginning of a line means it's a comment, not an active setting. A setting visible in a file may also be superseded by an included file. The `sshd -T` command shows effective server settings for a given context.

## 4. What Did We Actually Build or Test?

We used Ubuntu Server in VirtualBox, connecting from Windows PowerShell through forwarded port 2222 to the VM's SSH port 22.

First, we saved configuration backups and a VM snapshot called `Lab03-Phase2-PreHardening`. Then we generated an ED25519 key pair on Windows and verified a working key-based login.

We discovered that `50-cloud-init.conf` explicitly allowed SSH passwords. Rather than editing that file, we created `01-lab-hardening.conf` with these controls:

```text
PubkeyAuthentication yes
PasswordAuthentication no
KbdInteractiveAuthentication no
PermitRootLogin no
X11Forwarding no
```

We staged the file first, checked syntax, activated it, verified the combined configuration and effective settings, then reloaded SSH.

**What the tests showed:**

| Test | Observed result |
| --- | --- |
| Combined syntax check | Passed |
| Effective password authentication | `no` |
| Effective public-key authentication | `yes` |
| SSH service after reload | `active` |
| New key-based login | Successful |
| Password-only client attempt | `Permission denied (publickey).` |
| Server log | Recorded accepted public key and later pre-authentication reset |

We did **not** need to restore the VM snapshot or execute the rollback procedure.

## 5. Why Would Anyone Want This?

Administrators need remote access to servers, but every enabled login method increases what must be secured. Requiring SSH keys can reduce exposure to password guessing against SSH.

It does **not** mean a server is automatically secure. A stolen private key, weak account permissions, outdated software, or exposed services can still create risk. Our VM also reported pending security updates, which we did not install during this focused experiment.

## 6. What Did We Learn?

The most valuable lesson was not simply typing `PasswordAuthentication no`.

We learned to distinguish the SSH client configuration from the server configuration, discover included settings, preserve a recovery path, test a change before applying it, and check both an allowed login and a rejected login.

We also learned practical command-line troubleshooting. Misspelling a directory and confusing `ls -ld` with `ls -la` were recoverable mistakes, not reasons to skip verification.

**Operational approach:** Identify the risk → explain what it affects → recommend a controlled change → document the outcome.

## 7. Three Things to Remember

1. **Keys are credentials too.** Protect the private key; only the public key belongs on the server.
2. **A configuration file is not the whole truth.** Included files and effective settings matter.
3. **Test both directions.** A permitted login should work, and a disallowed login should fail.

## 8. Explore the Actual Experiment

Read the [technical SSH hardening report](../labs/lab-03-ubuntu-server/lab-03-phase-2-experiment-02-ssh-hardening.md) for the commands, evidence, limitations, and untested rollback procedure.

## 9. AI-Assisted Educational Documentation

This explanation was drafted with AI assistance from the user's actual home-lab observations. The commands and tests were performed by the human lab operator; the report distinguishes observed outcomes from planned or untested procedures.

**Build → Test → Understand → Document → Verify**
