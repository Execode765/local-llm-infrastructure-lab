# Why Is an SSH Key Safer Than Just a Password?

*Local LLM Infrastructure Lab — Concepts Explained, Lesson 05*

**Companion experiment:** Lab 03 — Phase 2, Experiment 02: SSH Hardening  
**Status:** Based on completed and verified testing, November 22–23, 2026.

## 1. The Big Question

Imagine you have a computer in another building that you need to manage from home.

You can connect to it over a network, install updates, troubleshoot problems, and perform maintenance without physically touching it.

But there's an important question:

**How do you make sure only authorized people can connect to that computer?**

Most of us are familiar with passwords. We use them to unlock phones, access websites, and sign in to computers.

But passwords aren't the only way to prove who we are.

In this experiment, we explored another method called **SSH key authentication** and learned how to make remote access more secure.

## 2. Think of It Like a Building With a Secure Entrance

Imagine an office building with a keypad on the front door.

Anyone who knows the correct code can potentially enter.

Now imagine replacing that keypad with an electronic access system that recognizes a special credential.

The building has a way to recognize authorized credentials, but it doesn't need to keep a copy of each person's secret credential.

SSH keys work somewhat like this.

There are two parts:

**The public key:** Think of this as information the building's security system uses to recognize an authorized credential. It can be shared without revealing the secret.

**The private key:** Think of this as the secret credential you keep in your possession. You must protect it.

The two keys are mathematically related.

When you connect to a server, your computer uses the private key to prove that it possesses the matching secret.

The server checks that proof using the public key.

Here's the important part:

**Your private key does not have to travel across the network to prove that you have it.**

The server can verify your identity without receiving your private key.

This is more sophisticated than a physical key or access card, but the analogy helps explain why the two parts exist.

## 3. What Makes It Work?

Let's explain the technology without assuming you already know Linux.

### What is SSH?

SSH stands for Secure Shell.

It's a way to securely communicate with and control another computer over a network.

It encrypts the connection so that someone observing the network cannot simply read the commands and information being exchanged.

### What is SSH authentication?

Authentication means proving that you are authorized to sign in.

SSH supports several authentication methods, including passwords and cryptographic keys.

### What is an SSH key pair?

A key pair consists of two related keys:

- A **public key** that the server can use to verify your identity.
- A **private key** that stays with you and is used to produce the proof.

In our experiment, we used a modern key type called ED25519.

You don't need to memorize the name. For now, understand that it's a cryptographic method used to create and verify digital signatures.

### What is a passphrase?

We also protected our private key with a passphrase.

Think of this as an additional lock protecting the private-key file.

The passphrase is different from the Linux account password.

It helps protect the private key if someone obtains a copy of the file, although its effectiveness depends on the passphrase's strength and how securely the computer is managed.

### What is SSH hardening?

Hardening means reducing unnecessary security risks by carefully changing a system's configuration.

For SSH, that might include limiting login methods, restricting privileged access, and disabling features that aren't needed.

Hardening does not mean a system becomes impossible to compromise.

It means reducing specific opportunities for unauthorized access.

## 4. What Did We Actually Build or Test?

We performed this experiment using an Ubuntu Linux server running inside a virtual machine.

Our Windows laptop acted as the remote computer connecting to it.

Before changing anything, we created a recovery snapshot and saved copies of the SSH configuration.

We also kept access to the virtual machine's console in case we accidentally locked ourselves out.

### Step 1: Create our digital keys

We generated an ED25519 public/private key pair on Windows.

We protected the private key with a passphrase and kept it on Windows.

Only the public key was installed on Ubuntu.

We then tested the connection and confirmed that SSH key authentication worked.

### Step 2: Investigate the existing security settings

Before making changes, we examined how Ubuntu was configured.

We discovered that one of its configuration files explicitly allowed password authentication.

We also learned that SSH can read settings from multiple files.

Think of this like a company having a main policy handbook plus several department-specific policy documents.

Reading just one document may not tell you which rule is actually being enforced.

We checked the effective settings before deciding what to change.

### Step 3: Apply our security changes

We created a dedicated configuration file with five settings:

| Setting | Plain-English meaning |
|---|---|
| `PubkeyAuthentication yes` | Allow users to authenticate using SSH keys |
| `PasswordAuthentication no` | Disable ordinary SSH password authentication |
| `KbdInteractiveAuthentication no` | Disable another interactive authentication method |
| `PermitRootLogin no` | Disallow direct SSH login as the highly privileged root account |
| `X11Forwarding no` | Disable an unnecessary remote graphical application feature |

We checked the configuration for syntax errors before activating it.

Then we applied the changes without rebooting the entire server.

### Step 4: Test what was allowed

We opened a new connection from Windows using our private key.

**Result: Successful.**

Ubuntu accepted the key-based login.

The server logs also confirmed that public-key authentication had succeeded.

### Step 5: Test what was supposed to be blocked

Next, we deliberately attempted a connection that could not use our SSH key and requested password authentication instead.

**Result: Rejected.**

The connection returned:

`Permission denied (publickey).`

This was an expected result.

It demonstrated that the server was no longer offering ordinary password-based SSH authentication for that tested connection.

### Step 6: Verify the server was still healthy

We confirmed that the SSH service remained active and that the intended authentication settings were in effect.

We did not need to restore our snapshot or perform a rollback.

Our recovery procedures were prepared, but they were not exercised during this experiment.

## 5. Why Would Anyone Want This?

Imagine an organization operating hundreds of Linux servers.

Those servers might host websites, databases, internal applications, or AI services.

Administrators need to access them remotely, but attackers may also try to gain access.

Passwords can be guessed, reused, stolen, or exposed through phishing.

Properly managed SSH keys can reduce certain password-related risks, particularly automated attempts to guess SSH account passwords.

However, SSH keys introduce responsibilities of their own.

If someone steals an unprotected private key, compromises the administrator's computer, or gains access through another weakness, the server can still be at risk.

**SSH key authentication is an important security control, not a complete security strategy.**

Other measures, including software updates, access restrictions, monitoring, and careful credential management, remain important.

## 6. What Did We Learn?

One of the most important lessons was that improving security requires more than changing a setting.

We learned to investigate the current configuration, preserve a recovery path, make controlled changes, and verify the results.

We also discovered that a successful login only proves that an allowed method works.

It doesn't prove that an unwanted method has been blocked.

That's why we performed two separate tests.

One confirmed that key authentication worked.

The other confirmed that password-only authentication was rejected.

We also learned the importance of checking the settings the server actually uses rather than assuming that a single configuration file tells the whole story.

Our operational approach remained:

**Identify → Explain → Recommend → Document.**

We identified the authentication risk, explained the existing configuration, implemented a controlled change, and documented the evidence.

## 7. Three Things to Remember

**1. Your private key is a secret.**

The public key can be shared with the server. The private key must be protected.

**2. Security settings must be verified.**

Changing a configuration file doesn't automatically prove that the server is enforcing the intended policy.

**3. Good security testing checks what works and what fails.**

We verified that authorized key-based access succeeded and password-only access was rejected.

Both results mattered.

---

## 8. Explore the Actual Experiment

This beginner-friendly explanation is based on our completed technical experiment:

[Lab 03 — Phase 2, Experiment 02: SSH Hardening](../labs/lab-03-ubuntu-server/lab-03-phase-2-experiment-02-ssh-hardening.md)

The technical report contains the actual commands, configuration changes, observed results, troubleshooting notes, and recovery procedures.

**Scope of verification:** We confirmed key-based login and password-only rejection for the tested Ubuntu account and connection through VirtualBox networking.

Direct root login and graphical forwarding restrictions were confirmed through effective configuration settings, not separate live attack simulations.

We did not test every possible account, authentication method, or security threat. We also did not perform the documented rollback procedure or install the pending operating-system updates during this experiment.

This was an isolated educational lab, not a production security certification.

## 9. AI-Assisted Educational Documentation

This educational guide was developed with assistance from ChatGPT by OpenAI, using the repository owner's documented hands-on experiment.

The infrastructure configuration, troubleshooting, authentication tests, and verification were performed by the human lab operator.

AI assistance was used to organize the explanation, clarify technical concepts, and make the material accessible to readers without an IT background.

The guide distinguishes verified observations from general technical explanations and procedures that were not tested.

**Build → Test → Understand → Document → Verify.**
