# What Happens When SSH Stops Working?

*Local LLM Infrastructure Lab — Concepts Explained, Lesson 04*

## 1. The Big Question

Imagine you're responsible for maintaining a computer in another building.

You can't physically see it, but you can connect to it from your own computer, check its health, install updates, and troubleshoot problems.

Then one day, your connection stops working.

Does that mean the computer has crashed? Has the network failed? Or is something preventing you from connecting?

In our latest infrastructure experiment, we deliberately created this situation to learn how remote computer administration works and what happens when that access becomes unavailable.

## 2. Think of It Like a Building With a Locked Door

Imagine an office building with a secure entrance.

The building has electricity, employees, computers, and everything needed to operate.

But there's a problem: the electronic door-access system stops working.

Suddenly, authorized visitors can't get inside.

Does that mean the entire building has stopped operating?

Not necessarily.

The building may be functioning normally. The problem is with the system that allows people to enter.

SSH works somewhat like that secure entrance.

SSH stands for *Secure Shell*. It's a technology that allows authorized users to securely access and manage another computer over a network.

If SSH stops working, administrators may lose their normal remote access even while the computer itself continues operating.

That's exactly the type of failure we investigated.

## 3. How Does SSH Actually Work?

SSH involves two main participants.

**The client:** The computer initiating the connection. In our experiment, this was our Windows laptop using PowerShell.

**The server:** The computer receiving the connection. In our experiment, this was our virtual Ubuntu Linux server.

When an SSH connection is established, the two systems negotiate a secure, encrypted communication session.

The server proves its identity using a cryptographic host key, and the user must authenticate using an accepted method, such as a password or SSH key.

Once authenticated, the administrator can issue commands remotely.

This means someone can manage a computer without physically sitting in front of it.

SSH is widely used in Linux administration, data centers, cloud computing, and other IT environments.

## 4. What Did We Actually Test?

Our Ubuntu Server virtual machine was already installed and configured.

We had previously verified that we could connect to it from Windows using SSH.

For our next experiment, we deliberately disabled the SSH components responsible for accepting incoming connections.

We weren't trying to damage the virtual computer.

We wanted to observe what happens when a critical remote-access service becomes unavailable.

### What happened?

After disabling SSH, our Windows computer could no longer establish its usual remote session.

The connection failed because Ubuntu was no longer listening for incoming SSH connections on its normal port.

In everyday language, the computer was still there, but the doorway we normally used to reach it was closed.

This distinction matters.

A failed remote connection does not automatically mean the entire server is offline.

### How did we diagnose it?

We examined the SSH service and its listening socket.

A *service* is a program or system component that performs a particular job.

A *socket* is a software endpoint that allows network communication. In our Ubuntu configuration, the SSH socket could listen for connections and activate the SSH service when needed.

Our investigation confirmed that the SSH socket was inactive.

This gave us evidence of the actual problem instead of relying on assumptions.

### How did we restore access?

Using the virtual machine's local console, we restarted the SSH socket.

We then checked that the system was listening for SSH connections again.

Finally, we returned to Windows PowerShell and successfully connected to Ubuntu.

Our verification confirmed that:

- The SSH socket was active.
- Ubuntu was listening for connections on port 22.
- A new remote login succeeded.
- The system reported no failed units in the final service-health check.

We restored access without rebooting the entire virtual machine or restoring its snapshot.

## 5. Why Would Anyone Want to Understand This?

Imagine working as a systems administrator responsible for hundreds of servers.

One morning, you discover that you can't remotely connect to a particular machine.

Your first instinct might be to restart the server.

But what if the server is still running an important application?

An unnecessary restart could interrupt services that were otherwise functioning normally.

Understanding SSH helps administrators approach the situation more carefully.

Instead of immediately assuming the worst, they can investigate whether the problem involves the network, the SSH service, authentication, or another component.

This is an important difference between guessing and troubleshooting.

It's also why having an alternative way to access a server can be valuable when its normal remote-access method fails.

In our experiment, the virtual machine console provided that alternative access.

## 6. What Did We Learn?

Our experiment demonstrated that losing remote access is not necessarily the same as losing the entire computer.

We learned that SSH depends on specific software components being available and correctly configured.

We also learned that a service failure can sometimes be corrected without restarting the operating system.

Most importantly, we practiced a structured troubleshooting process.

**Identify:** Determine what isn't working.

**Explain:** Understand why the failure is happening.

**Recommend:** Choose a corrective action based on the evidence.

**Document:** Record the problem, the corrective action, and the verification results.

This process applies far beyond SSH.

It can be used when troubleshooting network connections, applications, databases, and many other infrastructure services.

## 7. Three Things to Remember

1. **A failed SSH connection doesn't automatically mean the server is offline.** The problem may be limited to its remote-access service.

2. **Troubleshooting should begin with evidence, not assumptions.** Identifying the failed component helps avoid unnecessary changes.

3. **Restoring a service isn't the end of the job.** Administrators should verify that the original functionality has returned and document what happened.

---

## Explore the Actual Experiment

This lesson accompanies [Lab 03 Phase 2, Experiment 01 — SSH Failure and Recovery](../labs/lab-03-ubuntu-server/lab-03-phase-2-experiment-01-ssh-recovery.md).

The technical experiment documents the commands used to disable SSH, the connection failures observed, the diagnostic checks, and the steps taken to restore and verify remote access.

Unlike this beginner-friendly explanation, the technical report provides the operational details needed to understand and reproduce the controlled experiment.

**Current scope:** This lesson covers the completed SSH failure-and-recovery experiment. SSH security hardening is reserved for a future experiment.

## AI-Assisted Educational Documentation

This guide was developed with assistance from ChatGPT by OpenAI, based on the repository owner's documented hands-on infrastructure experiments.

The repository owner performed the SSH failure, diagnosis, recovery, and verification. AI assistance was used to explain the underlying concepts and prepare accessible educational documentation.

The guide distinguishes completed work from planned experiments.

**Project philosophy: Build → Test → Understand → Document.**
