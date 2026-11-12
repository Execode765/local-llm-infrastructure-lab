# How Can You Run a Computer Inside Another Computer?

*Local LLM Infrastructure Lab — Concepts Explained, Lesson 03*

## 1. The Big Question

Imagine turning on your laptop, opening an application, and starting an entirely separate computer inside it.

This second computer has its own operating system, memory, storage, network connection, and even its own name.

You can install software on it, connect to it remotely, restart it, and troubleshoot problems without needing to purchase another physical computer.

Sounds strange, doesn't it?

But that's exactly what virtualization makes possible.

In this experiment, we used a Windows gaming laptop to create and operate a virtual computer running Ubuntu Server, a Linux-based operating system commonly used for servers.

## 2. Think of It Like an Apartment Building

Imagine a large building with several apartments.

The building provides the shared infrastructure: electricity, plumbing, heating, and physical space.

But each apartment functions as a separate living environment.

Different people can occupy different apartments, arrange their belongings, and manage their daily activities independently.

Virtualization works somewhat similarly.

Your physical computer is the building.

The virtual machines are the apartments.

Each virtual machine can have its own operating system and software while sharing the physical computer's underlying hardware.

The software that manages these virtual computers is called a *hypervisor*.

Of course, virtual machines aren't completely independent. If the physical computer loses power or its hardware fails, the virtual machines running on it can be affected too.

But virtualization allows one physical computer to support multiple separate computing environments.

## 3. What Makes Virtualization Possible?

Several components work together.

**The host computer:** This is the physical machine providing the actual processing power, memory, and storage. In our experiment, the host was a Windows gaming laptop.

**The hypervisor:** This is the technology that creates and manages virtual machines. We used Oracle VirtualBox, which works alongside the virtualization capabilities available on our Windows system.

**The guest operating system:** This is the operating system installed inside the virtual machine. Our guest was Ubuntu Server 24.04.5 LTS.

**Virtual hardware:** The virtual machine is assigned simulated or virtualized components, such as processors, memory, storage drives, and network adapters. These are backed by resources from the physical computer.

**Virtual networking:** The guest can communicate over a network using virtual network adapters. Depending on the configuration, it may communicate with the host, other computers, or the internet.

One important detail: creating a virtual computer doesn't create additional physical computing power.

If your laptop has a limited amount of memory or processing capacity, its virtual machines must share those resources.

Virtualization makes hardware more flexible. It doesn't make hardware limitations disappear.

## 4. What Did We Actually Build?

For Lab 03, we created a virtual Ubuntu Server environment using Oracle VirtualBox.

Our physical computer was an ASUS ROG Strix gaming laptop running Windows.

We configured the virtual machine with:

- Two virtual processors
- 4 GB of assigned memory
- A 40 GB virtual storage disk
- Ubuntu Server 24.04.5 LTS
- A virtual network connection

We named the virtual machine `Local-LLM-Lab03-Ubuntu`.

After installing Ubuntu, we performed system updates, checked its storage configuration, and verified that it could communicate with the internet.

But one of the most interesting parts came next.

### We connected to the virtual computer remotely

Instead of controlling Ubuntu through its VirtualBox window, we used a technology called SSH.

SSH stands for *Secure Shell*.

Think of SSH as a secure doorway that allows an authorized person to open a command-line session on another computer.

From Windows PowerShell, we connected to our Ubuntu virtual machine using:

```powershell
ssh -p 2222 labadmin@127.0.0.1
```

That command opened a session on our Ubuntu server.

We could then enter Linux commands from the Windows terminal, even though Ubuntu was running as a separate virtual computer.

The address `127.0.0.1` refers to the local computer itself. VirtualBox forwarded the connection from port 2222 on Windows to the SSH service inside Ubuntu.

This is similar in principle to how IT professionals administer remote servers, although production environments often use different network configurations and security controls.

### We verified that the system was working

Our checks confirmed that:

- Ubuntu was installed and running.
- The virtual network adapter had an IP address.
- A test of internet connectivity received four successful replies.
- The SSH listening socket was active.
- We could successfully connect from Windows.
- A package audit reported no incomplete package configurations.

We also encountered and resolved several installation and configuration issues, including a hostname typo and installation media that needed to be disconnected before the virtual machine could boot normally.

These are examples of the small but important troubleshooting tasks that come with administering computer systems.

## 5. Why Would Anyone Want to Do This?

Virtualization has many practical applications.

**Learning without buying more computers:** Students and technicians can practice Linux, networking, and server administration using hardware they already own.

**Testing software:** Developers can experiment in a separate environment without directly installing every application on their main operating system.

**Running different operating systems:** A Windows computer can host a Linux virtual machine, allowing users to work with both systems.

**Data center efficiency:** Organizations can operate multiple virtual servers on powerful physical machines instead of dedicating an entire physical computer to every workload.

**Cloud computing:** Virtualization is one of the foundational technologies used by many cloud platforms to provide computing resources to customers.

**Infrastructure troubleshooting:** Administrators can practice diagnosing network failures, service problems, and operating-system issues in controlled environments.

However, virtual machines have limitations.

They still depend on the host's hardware, and running multiple machines consumes memory, storage, and processing capacity.

Virtual machines also provide isolation, but they are not an absolute security boundary. Software vulnerabilities and configuration mistakes can still create risks.

And although virtualization is widely used in production environments, operating a virtual machine on a personal laptop is not the same as managing a production data center.

It is a practical way to learn many of the underlying concepts.

## 6. What About Snapshots?

Before ending Phase 1, we created a VirtualBox snapshot named:

`Lab03-Baseline-Clean-Install`

Think of a snapshot as saving a checkpoint in a video game.

If you later make changes that cause problems, you may be able to return the virtual machine to its earlier state.

This is particularly useful when learning how to troubleshoot.

For example, we can eventually introduce a controlled configuration problem, investigate it, and practice restoring normal operation.

But there's an important distinction.

**A snapshot is not the same as an independent backup.**

Our snapshot depends on the virtual machine files stored on our external SSD.

If that storage device fails, both the virtual machine and its snapshot could be lost.

Snapshots are useful recovery checkpoints, but they don't replace a proper backup strategy.

### What comes next?

Our initial Ubuntu installation, remote access, verification, and baseline snapshot are complete.

The next phase will involve deliberately introducing selected problems and practicing diagnosis and recovery.

Those failure-and-recovery experiments have not yet been performed.

## 7. What Did We Learn?

Our experiment demonstrated that we could operate a separate Linux server environment on an existing Windows laptop.

We learned that virtual machines have their own operating systems and configurations while sharing physical hardware resources.

We also learned that administering a server doesn't always require sitting in front of its screen.

Using SSH, we could manage Ubuntu through a command-line connection from Windows.

Finally, we established a known-good checkpoint before attempting more advanced troubleshooting exercises.

The larger lesson is that virtualization makes it possible to learn and experiment with infrastructure without owning an entire room full of servers.

It's one of the bridges between everyday computer use and professional systems administration.

## 8. Three Things to Remember

1. **A virtual machine behaves like a separate computer, but it shares the resources of a physical host.**
2. **Virtualization makes it possible to learn, test, and operate multiple computing environments without purchasing separate hardware for each one.**
3. **Technologies like Linux, SSH, and virtual networking are foundational skills for server administration, data centers, and cloud infrastructure.**

---

## Explore the Actual Experiment

This article is the beginner-friendly companion to our Lab 03 Phase 1 technical documentation.

- [Lab 03 — Ubuntu Server Virtualization and Recovery (Phase 1)](../labs/lab-03-ubuntu-server/lab-03-phase-1.md)
- [Lab 03 — Command Reference](../labs/lab-03-ubuntu-server/lab-03-command-reference.md)

The technical documents contain the actual installation settings, system commands, troubleshooting observations, verification evidence, and snapshot details.

**Current lab status:** Phase 1 complete. Controlled failure-and-recovery experiments remain pending.

## AI-Assisted Educational Documentation

This guide was developed with assistance from ChatGPT by OpenAI, using the repository owner's documented hands-on experiments as the primary source.

The repository owner performed the virtual machine installation, configuration, troubleshooting, SSH verification, and snapshot creation. AI assistance was used to provide technical guidance, interpret observations, and prepare beginner-friendly educational documentation.

The guide distinguishes completed work from future experiments and does not represent the lab as a production deployment.

**Project philosophy: Build → Test → Understand → Document.**
