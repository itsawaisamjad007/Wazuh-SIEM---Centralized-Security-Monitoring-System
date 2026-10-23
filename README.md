# Wazuh SIEM - Centralized Security Monitoring System

## Overview
This project documents my first hands-on experience setting up a Wazuh SIEM lab from scratch. It covers the complete setup process, the errors I faced during installation, and how I troubleshooted and resolved them practically with patience.

This is a beginner-friendly lab, built to understand how centralized log monitoring, detection, and visualization actually work in a real SOC environment.

## Disclaimer
All activities were performed in my own isolated virtual lab for ethical and educational purposes only. No real systems were targeted.

## Lab Architecture

| Component | Role |
| :--- | :--- |
| **Windows 10** | Host OS |
| **Oracle VirtualBox** | Virtualization Platform |
| **Kali Linux VM** | Attacker Machine |
| **Ubuntu Server 24.04 LTS VM** | Victim / Monitored Server |
| **Wazuh Server VM** | Wazuh Manager, Indexer & Dashboard |

**Network:** All VMs were configured on **NAT Network** in VirtualBox to allow internal communication while keeping the lab isolated.

![](./screenshots/My%20Lab.png)


## Step-by-Step Setup Process

### 1. Setting Up the Virtual Machines
First, I installed Oracle VirtualBox on my Windows 10 host. I had already practiced installing Kali Linux manually using an ISO file multiple times, so this time I imported the official Kali OVA file to save time.

I then downloaded **Ubuntu Server 24.04 LTS** from its official website and installed it as a victim server. Finally, I downloaded the **Wazuh Server OVA** and imported it into VirtualBox.

### 2. Initial Attempt: Installing Wazuh on Ubuntu Server
Before using the OVA, I tried installing Wazuh directly on Ubuntu Server multiple times. The installation completed successfully:

![](./screenshots/01%20Wazuh%20install%20complete.png)

However, when I tried to access Wazuh in the browser, it kept showing "incorrect username or password" even though the credentials were correct. After several failed attempts, I decided to switch to the official Wazuh OVA file, which resolved the issue.

### 3. Installing Wazuh Agent on Ubuntu Server
After the Wazuh Server was up, I installed the Wazuh Agent on the Ubuntu Server to forward its logs to the manager.

![](./screenshots/06%20wazuh%20agent%20install%20in%20ubuntu%20server.png)

### 4. Configuring Wazuh Agent
I then configured the agent to connect to the Wazuh Manager's IP and enrolled it successfully.

![](./screenshots/07%20setting%20wazuh%20agent%20in%20ubuntu%20server.png)

## Troubleshooting Phase

### Issue 1: Wazuh-Manager Error
After installation, I repeatedly faced the `wazuh-manager` service error.

![](./screenshots/02wazuh-manager%20error.png)

### Issue 2: API Error in Dashboard
Once the manager error was fixed, I faced an API connection error in the Wazuh Dashboard on the browser.

![](./screenshots/03%20Api%20error%20in%20wazuh%20dashboard%20in%20browser.png)

## How I fixed it:

I restarted all core services one by one and increased the resources for the Wazuh VM:

* systemctl restart wazuh-indexer
* systemctl restart wazuh-manager
* systemctl restart wazuh-dashboard

# Core Concepts Covered in This Project

## 1. SIEM & Wazuh Architecture
- **SIEM Concept:** Centralized collection, analysis, and alerting of logs from multiple sources.
- **Wazuh Manager:** The central component that collects logs, applies rules, and performs detection.
- **Wazuh Indexer:** An OpenSearch-based engine that stores, indexes, and searches logs.
- **Wazuh Dashboard:** The web interface for visualizing alerts, investigating incidents, and reporting.
- **Wazuh Agent:** A lightweight software installed on Ubuntu Server to forward logs to the Manager.

## 2. Virtualization & Lab Environment
- **Oracle VirtualBox:** Running multiple virtual machines on a Windows 10 host.
- **OVA vs ISO Installation:** Learned the difference between manual installation via ISO and importing a pre-built machine via OVA file.
- **Resource Management:** Understood how RAM and storage allocation directly affect Wazuh services. Low RAM caused Indexer and Dashboard failures.

## 3. Networking in VirtualBox
- **NAT Network:** Configured all VMs on a single NAT Network to allow internal communication while keeping the lab isolated.
- **NAT vs Bridged vs Host-Only:** Learned the differences between all network modes and why NAT Network is the safest choice for a lab.
- **IP Addressing & Connectivity:** Configured IPs for Wazuh Server, Ubuntu Agent, and Kali, and verified connectivity using ping.

## 4. Linux & Service Management
- **Ubuntu Server 24.04 LTS:** Managed a CLI-based server without a GUI.
- **Systemd Services:** Managed Wazuh services using `systemctl status`, `restart`, and `enable`.
- **Service Troubleshooting:** Diagnosed services through logs, restarted them systematically, and verified their status.

## 5. Troubleshooting & Problem Solving
- **Wazuh Manager Error:** Investigated why the Manager service failed to start.
- **API Error:** Resolved Dashboard API connectivity issues with the Manager and Indexer.
- **Login Issue:** Faced authentication failure despite correct credentials and solved it by switching to the official OVA.
- **Persistence in Lab Work:** Completed the lab after multiple failures by systematically testing each solution.