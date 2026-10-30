# 🔐 Cybersecurity Lab Environment Setup
Building an isolated virtual lab for penetration testing and ethical hacking practice[cite: 16, 17]

---

## 📌 Project Overview
This project focuses on setting up a virtual cybersecurity and penetration-testing laboratory using VirtualBox and Kali Linux[cite: 16, 17].

The purpose of the lab is to create a controlled environment where cybersecurity tools, network scanning, reconnaissance, vulnerability assessment, and other security-testing activities can be performed safely and repeatedly[cite: 16, 17].

The lab is configured on a private virtual network so that additional machines can be added later and used as targets for authorized security testing[cite: 16, 17, 26].

---

## 🎯 Objectives
The main objectives of this project are to:
* Install and configure VirtualBox[cite: 25, 27].
* Install/import Kali Linux as a virtual machine[cite: 25, 27].
* Create a private NAT Network for the cybersecurity lab[cite: 25, 27, 28].
* Configure network connectivity for Kali Linux[cite: 25, 27, 30].
* Assign a consistent IP address to the Kali VM.
* Verify network connectivity and DNS resolution[cite: 27, 30].
* Take a clean VM snapshot for recovery.
* Document the complete setup process[cite: 8, 17].
* Prepare the environment for future cybersecurity projects.

---

## 🛡️ Purpose of the Lab
The lab provides an isolated and controlled environment for cybersecurity learning and authorized security testing[cite: 2, 16, 17].

It can be used for activities such as:
* Network reconnaissance[cite: 16]
* Port scanning[cite: 16]
* Vulnerability assessment[cite: 16]
* Packet analysis[cite: 16]
* Web security testing[cite: 16]
* Exploitation practice[cite: 16]
* Security-tool experimentation[cite: 16]

> **⚠️ Important:** This laboratory must only be used for systems that you own or have explicit permission to test. Do not use the lab or its tools to attack unauthorized systems.

---

## 🏗️ Lab Architecture
The lab is built using Oracle VirtualBox running a custom NAT Network (`10.0.0.0/24`)[cite: 25, 26, 28]. Additional target machines (e.g., Windows 10/11, Server 2016, Android) can be added to the same virtual network in future projects[cite: 26, 27].

---

## ⚙️ Lab Configuration

| 🧩 Component | ⚙️ Configuration |
| :--- | :--- |
| **🖥️ Host OS** | Windows 10 / 11[cite: 26] |
| **🧠 Host RAM** | 8 GB or more[cite: 26] |
| **⚡ Processor** | Core i3 / i5 / i7[cite: 26] |
| **🧰 Hypervisor** | VirtualBox v7.x[cite: 25, 27] |
| **🐉 Security OS** | Kali Linux 2026.x[cite: 25, 27, 29] |
| **🧠 Kali RAM** | 2048 MB |
| **🌐 Virtual Network** | NAT Network (`NatNetwork`)[cite: 25, 28] |
| **📡 Network Address** | `10.0.0.0/24`[cite: 25, 28] |
| **🐧 Kali IP Address** | `10.0.0.2/24`[cite: 25, 26, 30] |
| **🚪 Default Gateway** | `10.0.0.1`[cite: 30] |
| **🌍 DNS Server** | `8.8.8.8`[cite: 30] |
| **🔮 Future VM Range** | `10.0.0.3` – `10.0.0.99`[cite: 26] |

---

## 🪜 Lab Setup Procedure

### Step 1. Install 7-Zip
7-Zip was installed to extract the Kali Linux virtual-machine package.
* **Tool:** 7-Zip (`https://7-zip.org/download.html`)[cite: 27]

### Step 2. Install VirtualBox
VirtualBox was installed as the host hypervisor[cite: 25, 27].
* **Tool:** Oracle VirtualBox (`https://virtualbox.org/wiki/Downloads`)[cite: 27]

### Step 3. Create the NAT Network
A dedicated NAT Network was created in VirtualBox under **Tools** > **Network** > **NAT Networks**[cite: 28].
* **Network Name:** `NatNetwork`[cite: 28]
* **IPv4 Prefix:** `10.0.0.0/24`[cite: 25, 28]
* **DHCP:** Enabled[cite: 28]
* **IPv6:** Disabled[cite: 28]

> A NAT Network was selected because multiple virtual machines connected to the same NAT Network can communicate with one another while also having outbound network connectivity.

### Step 4. Import Kali Linux
The Kali Linux virtual machine was imported into VirtualBox[cite: 25, 27, 29].
* **Adapter 1 Attached to:** `NAT Network`[cite: 25, 29]
* **Network Name:** `NatNetwork`[cite: 29]
* **Adapter Type:** Intel PRO/1000 MT Desktop[cite: 29]
* **Promiscuous Mode:** `Allow All`[cite: 29]
* **Allocated RAM:** 2048 MB
* **Shared Folders:** Configured host `/downloads` folder mapping with Full Access[cite: 25, 32].
* **Clipboard / Drag & Drop:** Enabled Bidirectional[cite: 25].

### Step 5. Configure the Kali Linux Network
The network connection inside Kali Linux was configured with a static IPv4 address[cite: 25, 27, 30]:
* **IP Address:** `10.0.0.2`[cite: 25, 30]
* **Subnet Mask:** `255.255.255.0` (`/24`)[cite: 25, 30]
* **Gateway:** `10.0.0.1`[cite: 30]
* **DNS:** `8.8.8.8`[cite: 30]

### Step 6. Create a Clean VM Snapshot
After completing the initial setup, a VirtualBox snapshot was created:
* **Snapshot Name:** `Clean Kali - Network Setup`[cite: 27, 33]

---

## 🔎 Lab Verification

| ✅ Test | 🧾 Command | 🎯 Expected Result |
| :--- | :--- | :--- |
| **🌐 Check IP address** | `ip a` | Correct Kali IP (`10.0.0.2`) displayed[cite: 25, 30] |
| **📡 Test gateway** | `ping 10.0.0.1` | Successful ICMP replies[cite: 27, 30] |
| **🌍 Test Internet connectivity** | `ping 8.8.8.8` | Successful ICMP replies[cite: 25, 30] |
| **🔎 Test DNS resolution** | `nslookup networkwalks.com` | Domain resolves successfully[cite: 25, 30] |
| **🧰 Verify Nmap** | `nmap --version` | Nmap version information displayed[cite: 16] |
| **🔄 Verify snapshot** | Restore snapshot and run `ip a` | Baseline configuration restored[cite: 27, 33] |

---

## 🐞 Problems Encountered & Solutions

### Problem 1. Internet Connectivity After Static IP Configuration
After manually configuring IPv4 settings, Internet connectivity failed due to NetworkManager duplicate address detection (DAD) timeouts on Kali Linux 2026.x releases[cite: 23, 31, 32].

**Solution:**
Ran the following terminal commands to disable DAD timeouts and restart the network connection[cite: 23, 31, 32]:
```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
