# 🔐 Cybersecurity Lab Environment Setup
Building an isolated virtual lab for penetration testing and ethical hacking practice.

---

## 📌 Project Overview
This project focuses on setting up a virtual cybersecurity and penetration-testing laboratory using VirtualBox and Kali Linux.

The purpose of the lab is to create a controlled environment where cybersecurity tools, network scanning, reconnaissance, vulnerability assessment, and other security-testing activities can be performed safely and repeatedly.

The lab is configured on a private virtual network so that additional machines can be added later and used as targets for authorized security testing.

---

## 🎯 Objectives
The main objectives of this project are to:
* Install and configure VirtualBox.
* Install/import Kali Linux as a virtual machine.
* Create a private NAT Network for the cybersecurity lab.
* Configure network connectivity for Kali Linux.
* Assign a consistent IP address to the Kali VM.
* Verify network connectivity and DNS resolution.
* Take a clean VM snapshot for recovery.
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

> **⚠️ Important:** This laboratory must only be used for systems that you own or have explicit permission to test. Do not use the lab or its tools to attack unauthorized systems.

---

## 🏗️ Lab Architecture
The lab is built using Oracle VirtualBox running a custom NAT Network (`10.0.0.0/24`). Additional target machines (e.g., Windows 10/11, Server 2016, Android) can be added to the same virtual network in future projects.

---

## ⚙️ Lab Configuration

| 🧩 Component | ⚙️ Configuration |
| :--- | :--- |
| **🖥️ Host OS** | Windows 10 / 11 |
| **🧠 Host RAM** | 8 GB or more |
| **⚡ Processor** | Core i3 / i5 / i7 |
| **🧰 Hypervisor** | VirtualBox v7.x |
| **🐉 Security OS** | Kali Linux 2026.x |
| **🧠 Kali RAM** | 2048 MB |
| **🌐 Virtual Network** | NAT Network (`NatNetwork`) |
| **📡 Network Address** | `10.0.0.0/24` |
| **🐧 Kali IP Address** | `10.0.0.2/24` |
| **🚪 Default Gateway** | `10.0.0.1` |
| **🌍 DNS Server** | `8.8.8.8` |
| **🔮 Future VM Range** | `10.0.0.3` – `10.0.0.99` |

---

## 🪜 Lab Setup Procedure

### Step 1. Install 7-Zip
7-Zip was installed to extract the Kali Linux virtual-machine package.
* **Tool:** 7-Zip (`https://7-zip.org/download.html`)

### Step 2. Install VirtualBox
VirtualBox was installed as the host hypervisor.
* **Tool:** Oracle VirtualBox (`https://virtualbox.org/wiki/Downloads`)

### Step 3. Create the NAT Network
A dedicated NAT Network was created in VirtualBox under **Tools** > **Network** > **NAT Networks**.
* **Network Name:** `NatNetwork`
* **IPv4 Prefix:** `10.0.0.0/24`
* **DHCP:** Enabled
* **IPv6:** Disabled

<img width="1266" height="1021" alt="image" src="https://github.com/user-attachments/assets/9dd429a2-e3d5-4f53-a897-77ab86f797dd" />

> A NAT Network was selected because multiple virtual machines connected to the same NAT Network can communicate with one another while also having outbound network connectivity.

### Step 4. Import Kali Linux
The Kali Linux virtual machine was imported into VirtualBox.
* **Adapter 1 Attached to:** `NAT Network`
* **Network Name:** `NatNetwork`
* **Adapter Type:** Intel PRO/1000 MT Desktop
* **Promiscuous Mode:** `Allow All`
* **Allocated RAM:** 2048 MB
* **Shared Folders:** Configured host `/downloads` folder mapping with Full Access.
* **Clipboard / Drag & Drop:** Enabled Bidirectional.

### Step 5. Configure the Kali Linux Network
The network connection inside Kali Linux was configured with a static IPv4 address:
* **IP Address:** `10.0.0.2`
* **Subnet Mask:** `255.255.255.0` (`/24`)
* **Gateway:** `10.0.0.1`
* **DNS:** `8.8.8.8`

<img width="1262" height="1021" alt="image" src="https://github.com/user-attachments/assets/4cfc878c-4716-49be-aaf9-1c0b94cdaeef" />
<img width="1271" height="1029" alt="image" src="https://github.com/user-attachments/assets/be45252a-f0b4-466a-85c7-4e0ac35ce9bf" />

### Step 6. Create a Clean VM Snapshot
After completing the initial setup, a VirtualBox snapshot was created:
* **Snapshot Name:** `Clean Kali - Network Setup`

<img width="1268" height="1021" alt="image" src="https://github.com/user-attachments/assets/c0d27d36-6aa0-4822-bc3d-20d83f903d87" />

---

## 🔎 Lab Verification

| ✅ Test | 🧾 Command | 🎯 Expected Result |
| :--- | :--- | :--- |
| **🌐 Check IP address** | `ip a` | Correct Kali IP (`10.0.0.2`) displayed |
| **📡 Test gateway** | `ping 10.0.0.1` | Successful ICMP replies |
| **🌍 Test Internet connectivity** | `ping 8.8.8.8` | Successful ICMP replies |
| **🔎 Test DNS resolution** | `nslookup networkwalks.com` | Domain resolves successfully |
| **🧰 Verify Nmap** | `nmap --version` | Nmap version information displayed |
| **🔄 Verify snapshot** | Restore snapshot and run `ip a` | Baseline configuration restored |

---

## 🐞 Problems Encountered & Solutions

### Problem 1. Internet Connectivity After Static IP Configuration
After manually configuring IPv4 settings, Internet connectivity failed due to NetworkManager duplicate address detection (DAD) timeouts on Kali Linux 2026.x releases.

**Solution:**
Ran the following terminal commands to disable DAD timeouts and restart the network connection:
```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
