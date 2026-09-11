<div align="center">

# 🔐 Cybersecurity Lab Environment Setup

**Building an isolated virtual lab for penetration testing and ethical hacking practice**

</div>

<p align="center">
  <img src="https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/VirtualBox-7.x-0070C0?style=flat-square&labelColor=000000" />
  <img src="https://img.shields.io/badge/Kali%20Linux-2026.2-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Linux-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Network-10.0.0.0%2F24-238F89?style=flat-square&labelColor=000000" />
  <img src="https://img.shields.io/badge/Penetration%20Testing-C00000?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Virtualization-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/GitHub-404040?style=flat-square&labelColor=0070C0&logo=github&logoColor=white" />
  <img src="https://img.shields.io/badge/Ethical%20Hacking-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
</p>

---

## 📌 Project Overview

This project focuses on setting up a **virtual cybersecurity and penetration-testing laboratory** using Oracle VM VirtualBox and Kali Linux.

The purpose of the lab is to create a controlled environment where cybersecurity tools, network scanning, reconnaissance, vulnerability assessment, packet analysis, web security testing, and other security-testing activities can be performed safely and repeatedly.

The laboratory is configured on a private virtual network so that additional machines can be added later and used as targets for authorized security testing.

---

## 🎯 Objectives

The main objectives of this project are to:

* Install and configure Oracle VM VirtualBox.
* Import and configure Kali Linux as a virtual machine.
* Create a private **NAT Network** for the cybersecurity lab.
* Configure network connectivity for Kali Linux.
* Assign a consistent static IP address to the Kali VM.
* Configure clipboard sharing and drag-and-drop.
* Configure a shared folder between the host and Kali VM.
* Verify network connectivity and DNS resolution.
* Create a clean VM snapshot for recovery.
* Document the complete setup process.
* Prepare the environment for future cybersecurity projects.

---

## 🛡️ Purpose of the Lab

The lab provides an isolated and controlled environment for cybersecurity learning and authorized security testing.

It can be used for activities such as:

* Network reconnaissance
* Port scanning
* Vulnerability assessment
* Packet analysis
* Web security testing
* Exploitation practice
* Security-tool experimentation

> ⚠️ **Important:** This laboratory must only be used for systems that you own or have explicit permission to test. Do not use the lab or its tools to attack unauthorized systems.

---

## 🏗️ Lab Architecture

The current laboratory consists of a Windows host running Kali Linux inside VirtualBox.

```text
                    ┌─────────────────────────┐
                    │      Windows Host       │
                    │                         │
                    │      VirtualBox 7.x     │
                    └────────────┬────────────┘
                                 │
                                 │ NAT Network
                                 │
                    ┌────────────▼────────────┐
                    │       NatNetwork        │
                    │      10.0.0.0/24        │
                    │                         │
                    │     Gateway: 10.0.0.1   │
                    └────────────┬────────────┘
                                 │
                         ┌───────▼───────┐
                         │   Kali Linux  │
                         │   2026.2      │
                         │               │
                         │ 10.0.0.2/24   │
                         └───────────────┘
```

Additional target machines can be added to the same virtual network in future projects.

---

## ⚙️ Lab Configuration

| **🧩 Component**   | **⚙️ Configuration**     |
| ------------------ | ------------------------ |
| 🖥️ Host OS        | Windows                  |
| 🧰 Hypervisor      | Oracle VM VirtualBox 7.x |
| 🐉 Security OS     | Kali Linux 2026.2        |
| 🧠 Kali RAM        | 2048 MB                  |
| 🌐 Virtual Network | NAT Network              |
| 📛 Network Name    | `NatNetwork`             |
| 📡 Network Address | `10.0.0.0/24`            |
| 🐧 Kali IP Address | `10.0.0.2/24`            |
| 🚪 Default Gateway | `10.0.0.1`               |
| 🌍 DNS Server      | `8.8.8.8`                |
| 📋 Clipboard       | Bidirectional            |
| 🖱️ Drag & Drop    | Bidirectional            |
| 📂 Shared Folder   | `Downloads`              |
| 🔄 Auto-mount      | Enabled                  |
| 🔮 Future VM Range | `10.0.0.3 – 10.0.0.99`   |

---

# 🪜 Lab Setup Procedure

## Step 1. Install VirtualBox

Oracle VM VirtualBox was installed on the Windows host machine and configured as the hypervisor for the cybersecurity laboratory.

---

## Step 2. Create the NAT Network

A dedicated NAT Network was created in VirtualBox.

**Path:**

```text
VirtualBox
   ↓
File
   ↓
Tools
   ↓
Network
   ↓
NAT Networks
```

The following configuration was used:

```text
Network Name: NatNetwork
IPv4 Prefix:  10.0.0.0/24
DHCP:         Enabled
```

A **NAT Network** was selected because multiple virtual machines connected to the same NAT Network can communicate with one another while also having outbound network connectivity.

This allows future attacker and target VMs to communicate within the isolated laboratory environment.

---

## Step 3. Import Kali Linux & Configure the Adapter

Kali Linux 2026.2 was imported into VirtualBox and configured to use the previously created NAT Network.

The VM network adapter was configured as follows:

```text
Adapter 1
Attached to: NAT Network
Network:     NatNetwork
```

The Kali VM was allocated:

```text
RAM: 2048 MB
```

Additional VirtualBox integration settings were configured:

```text
Clipboard:    Bidirectional
Drag & Drop:  Bidirectional
Shared Folder: Downloads
Auto-mount:    Enabled
```

These settings make it easier to transfer files and tools between the Windows host and Kali Linux VM.

---

## Step 4. Configure the Kali Linux Network

The Kali Linux network was configured with a consistent static IPv4 address using NetworkManager.

The following configuration was used:

```text
IP Address: 10.0.0.2/24
Subnet Mask: 255.255.255.0
Gateway: 10.0.0.1
DNS: 8.8.8.8
```

The configuration was applied using:

```bash
sudo nmcli connection modify "Wired connection 1" \
  ipv4.method manual \
  ipv4.addresses 10.0.0.2/24 \
  ipv4.gateway 10.0.0.1 \
  ipv4.dns 8.8.8.8
```

The network connection was then restarted:

```bash
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
```

The IP configuration was verified using:

```bash
ip a
```

A consistent IP address makes it easier to document the laboratory and reference the Kali machine during future cybersecurity exercises.

---

## Step 5. Verify Network Connectivity

Network connectivity was verified in multiple stages.

### Check IP Configuration

```bash
ip a
```

Expected:

```text
10.0.0.2/24
```

### Check Default Route

```bash
ip route
```

Expected:

```text
default via 10.0.0.1
```

### Test IP Connectivity

```bash
ping 8.8.8.8
```

This verifies connectivity without relying on DNS resolution.

### Test DNS Resolution

```bash
ping google.com
```

Successful replies confirm that DNS resolution is functioning.

---

## Step 6. Create a Clean VM Snapshot

After successfully completing the initial configuration, a VirtualBox snapshot was created.

Example snapshot name:

```text
WK1-PM1-Complete
```

The snapshot represents the clean baseline of the laboratory.

If a future cybersecurity exercise changes or damages the VM configuration, the machine can be restored to this known-good state.

---

# 🔎 Lab Verification

| **✅ Test**                    | **🧾 Command**            | **🎯 Expected Result**          |
| ----------------------------- | ------------------------- | ------------------------------- |
| 🌐 Check IP address           | `ip a`                    | `10.0.0.2/24` displayed         |
| 🚪 Check default route        | `ip route`                | `default via 10.0.0.1`          |
| 📡 Test gateway               | `ping 10.0.0.1`           | Successful replies              |
| 🌍 Test Internet connectivity | `ping 8.8.8.8`            | Successful replies              |
| 🔎 Test DNS resolution        | `ping google.com`         | Domain resolves                 |
| 🧰 Verify Nmap                | `nmap --version`          | Nmap version displayed          |
| 🔄 Verify snapshot            | Restore snapshot → `ip a` | Baseline configuration restored |

### Example Results

```text
IP Address : 10.0.0.2/24
Gateway    : 10.0.0.1
DNS        : 8.8.8.8
Network    : 10.0.0.0/24
```

---

# 🐞 Problems Encountered & Solutions

Documenting problems and troubleshooting steps is an important part of the project because it demonstrates the process used to identify and resolve configuration issues.

---

## Problem 1. Network Adapter Kept Disabling Itself

### Symptoms

The **Enable Network Adapter** setting appeared to revert to disabled whenever the VM was started, even after being enabled in VirtualBox.

### Cause

The setting was being modified while the VM was not completely powered off.

### Solution

The VM was completely powered off and verified as **Powered Off** in VirtualBox Manager.

The network adapter was then enabled again through:

```text
VirtualBox
   ↓
Settings
   ↓
Network
   ↓
Enable Network Adapter
```

The setting was verified before starting the VM.

### Lesson

When troubleshooting VirtualBox hardware or network settings, make sure the VM is completely powered off before modifying important configuration options.

---

## Problem 2. `ping google.com` Failed with DNS Resolution Error

### Symptoms

After configuring the static IP address, DNS resolution failed when running:

```bash
ping google.com
```

The system returned:

```text
Temporary failure in name resolution
```

### Diagnosis

Basic IP connectivity was tested first:

```bash
ping 8.8.8.8
```

The routing table was then checked:

```bash
ip route
```

This helped determine whether the problem was related to basic connectivity, routing, or DNS resolution.

---

## Problem 3. `ping 8.8.8.8` Returned `Destination Host Unreachable`

### Symptoms

The Kali VM had a static IP address and default route configured, but:

```bash
ping 8.8.8.8
```

returned:

```text
Destination Host Unreachable
```

### Root Cause

The NAT Network's IPv4 Prefix had accidentally been configured as:

```text
10.0.2.0/24
```

instead of:

```text
10.0.0.0/24
```

This created a subnet mismatch between the VirtualBox NAT Network and the static network configuration on Kali Linux.

The Kali VM was configured to use:

```text
IP Address: 10.0.0.2/24
Gateway:    10.0.0.1
```

while the VirtualBox NAT Network was using a different subnet.

### Fix

The NAT Network configuration was corrected through:

```text
VirtualBox
   ↓
File
   ↓
Tools
   ↓
Network
   ↓
NAT Networks
   ↓
NatNetwork
```

The IPv4 Prefix was changed to:

```text
10.0.0.0/24
```

The Kali network connection was then restarted:

```bash
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
```

Connectivity was tested again:

```bash
ping 8.8.8.8
ping google.com
```

Both tests then succeeded.

### Lesson

A single subnet mismatch can prevent network communication even when the VM's IP address, gateway, and DNS configuration appear correct.

Always verify both:

```text
VirtualBox NAT Network
        ↓
Kali IP Configuration
        ↓
Default Gateway
        ↓
DNS
```

---

# 💡 What I Learned

Through this project, I learned how to create and configure a virtual environment for cybersecurity practice.

The main concepts I learned include:

## 1. NAT vs NAT Network

A standard NAT configuration and a NAT Network serve different purposes.

A NAT Network allows multiple virtual machines connected to the same virtual network to communicate with one another while providing outbound connectivity.

This makes it useful for building a multi-machine cybersecurity laboratory containing attacker and target virtual machines.

---

## 2. Virtual Machine Networking

I learned how VirtualBox virtual network adapters connect virtual machines to different network configurations and how the selected network mode affects communication between virtual machines.

---

## 3. Static IP Configuration

I learned how to configure and verify:

* IPv4 addresses
* Subnet masks
* Default gateways
* DNS servers
* Network routes

using Kali Linux and NetworkManager.

---

## 4. VM Snapshots

I learned that a clean snapshot should be created before performing risky or experimental activities.

This provides a known-good recovery point that can be used when experimenting with cybersecurity tools and configurations.

---

## 5. Systematic Troubleshooting

I learned to troubleshoot network connectivity step-by-step rather than changing multiple settings at once.

The troubleshooting process followed:

```text
ip a
  ↓
ip route
  ↓
ping 8.8.8.8
  ↓
ping google.com
```

This helped isolate configuration, routing, connectivity, and DNS-related issues.

---

## 6. Documentation

I learned that documenting commands, configurations, screenshots, problems, and solutions is an important part of a professional cybersecurity project.

Good documentation makes a lab easier to reproduce, troubleshoot, and extend in future projects.

---

# 🔐 Security & Ethical Use

This laboratory is intended strictly for **educational purposes and authorized security testing**.

All penetration-testing, vulnerability-assessment, scanning, and exploitation activities should only be performed against:

* Systems owned by you
* Intentionally vulnerable laboratory machines
* Systems for which you have explicit authorization to test

> ⚠️ **Never use these techniques against unauthorized systems, networks, websites, or devices.**

---

# 🔗 Tools & Resources

* **7-Zip:** https://7-zip.org/
* **Oracle VM VirtualBox:** https://virtualbox.org/wiki/Downloads
* **Kali Linux:** https://kali.org/get-kali/

---

# 👤 Author

**Bhabani Priyadarshini Panda**

Cybersecurity Intern — Batch B083
Networkwalks

**LinkedIn:** https://linkedin.com/in/bp69

---

## 📌 Project Information

| **Field**        | **Details**                          |
| ---------------- | ------------------------------------ |
| **Program Name** | Cybersecurity at Networkwalks        |
| **Batch**        | B083                                 |
| **Week**         | 01                                   |
| **Project**      | Cybersecurity & Pentesting Lab Setup |
| **Platform**     | VirtualBox + Kali Linux              |
| **Network**      | `10.0.0.0/24`                        |
| **Security OS**  | Kali Linux 2026.2                    |
| **Repository**   | GitHub                               |

---

<div align="center">

### 🔐 Learn • Practice • Secure

**Cybersecurity Lab — Week 01**

</div>
