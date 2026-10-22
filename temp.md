# Wazuh SIEM — Centralized Security Monitoring System

## Overview
This project implements a centralized **Security Information and Event Management (SIEM)** solution using **Wazuh** deployed on **Ubuntu Server 24.04 LTS** within **Oracle VirtualBox**. The system is designed to collect, analyze, and visualize security events from endpoints for real-time threat detection, compliance monitoring, and Security Operations Center (SOC) operations.

**Deployment Model:** All-in-One (Single Node)

**Core Components:**
- **Wazuh Manager:** Central component for log collection, analysis, and rule-based detection.
- **Wazuh Indexer:** OpenSearch-based engine for log storage, indexing, and search.
- **Wazuh Dashboard:** Web-based UI for alert visualization, investigation, and reporting.

**Environment Details:**
- **Host OS:** Windows 11
- **Hypervisor:** Oracle VirtualBox 7.x
- **Guest OS:** Ubuntu Server 24.04 LTS
- **VM Name:** wazuh-server
- **Resources:** 4 GB RAM, 2 vCPUs, 50 GB Storage
- **Network Mode:** NAT with Port Forwarding
- **Administrative User:** wazuh

---

## Table of Contents
1. [Virtual Machine Provisioning](#1-virtual-machine-provisioning)
2. [Network Configuration](#2-network-configuration-bridged-to-nat-migration)
3. [Port Forwarding](#3-port-forwarding)
4. [Clipboard Limitation & SSH Solution](#4-clipboard-limitation-and-ssh-implementation)
5. [Wazuh Installation](#5-wazuh-installation)
6. [Dashboard Access & Verification](#6-dashboard-access--verification)
7. [Troubleshooting Summary](#7-troubleshooting-summary)
8. [Future Work](#8-future-work)

---

## 1. Virtual Machine Provisioning

1. Created a new Virtual Machine in Oracle VirtualBox with name `wazuh-server`.
2. Attached Ubuntu Server 24.04 LTS ISO as installation media.
3. Allocated system resources: 4 GB RAM, 2 CPU Cores, 50 GB VDI Disk.
4. Completed Ubuntu Server installation with user `wazuh` and hostname `wazuh-server`.
5. Verified successful boot and login as `wazuh@wazuh-server`.

---

## 2. Network Configuration: Bridged to NAT Migration

### Problem Identified
Initially, the VM was configured with **Bridged Adapter** mode and received IP `192.168.x.x` directly from the physical router. Internet speed inside the VM was significantly slower than on the host machine, causing `apt` downloads and Wazuh installer downloads to stall.

### Root Cause Analysis
Bridged mode places the VM directly on the physical LAN, making it dependent on router DHCP and DNS. In this environment, it resulted in packet loss and slow throughput due to router-level filtering and DNS latency.

### Solution Implemented: NAT Mode
Migrated the VM network adapter to **NAT** mode to route traffic through the host's network stack, achieving host-level download speed.

### Procedure
1. Power off the VM:
   ```bash
   sudo poweroff































   Navigate to VirtualBox > Settings > Network > Adapter 1Set Attached to: NATSet Adapter Type: Intel PRO/1000 MT Desktop (selected for driver stability)Power on the VM and verify network configuration:bash   ip aResult:javascript   IPv4 address for enp0s3: 10.0.2.15NAT IP 10.0.2.15 confirmed successful migration.3. Port ForwardingConceptIn NAT mode, the guest VM is isolated and not directly reachable from the host network. Port Forwarding creates a static mapping between a host port and a guest port, enabling external access to internal services.javascriptHost (https://localhost:443) -> Host Port 443 -> Guest Port 443 -> Wazuh Dashboard
Host (ssh -p 2222) -> Host Port 2222 -> Guest Port 22 -> SSH ServerConfigured RulesNameProtocolHost IPHost PortGuest IPGuest PortPurposeWazuhTCP127.0.0.144310.0.2.15443Dashboard HTTPS AccessSSHTCP127.0.0.1222210.0.2.1522SSH Remote AccessConfiguration Path: VirtualBox > VM Settings > Network > Advanced > Port ForwardingBenefitsDashboard accessible via https://localhost without Bridged mode.Only required ports are exposed, maintaining VM isolation and security.Removes dependency on router DHCP and physical network changes.4. Clipboard Limitation and SSH ImplementationIssue EncounteredBidirectional Clipboard was enabled in VirtualBox Settings, but copy-paste from Host (Windows) to Guest (Ubuntu Server) did not function.Root CauseUbuntu Server is a CLI-only operating system with no graphical desktop environment. VirtualBox Guest Additions and Clipboard sharing are designed for GUI-based guest OSes and are non-functional on a headless server terminal.Resolution: SSH-Based ManagementStep 1 — Install OpenSSH Server in VM:bashsudo apt update
sudo apt install openssh-server -y
sudo systemctl enable --now ssh
sudo systemctl status sshExpected Output: Active: active (running) — Server listening on 0.0.0.0 port 22Step 2 — Enable SSH Port Forwarding:
Host Port 2222 forwarded to Guest Port 22 as defined in Section 3.Step 3 — Connect from Windows Host via CMD / PowerShell:bashssh -p 2222 wazuh@127.0.0.1On first connection, confirm host authenticity by typing yes.Result:Full terminal access to Ubuntu Server from Windows CMD.Native copy-paste support (Ctrl+C / Ctrl+V) enabled.All subsequent installation commands executed via SSH for efficiency and reliability.5. Wazuh InstallationAll installation steps were performed via SSH connection from Windows CMD after NAT migration.Step 1 — System Preparationbashsudo apt update
sudo apt install curl -yNote: Full apt upgrade was intentionally skipped to prevent large bandwidth consumption and to preserve system stability during installation.Step 2 — Network Speed Verificationbashcurl -o /dev/null -s -w "Speed: %{speed_download}\n" https://packages.wazuh.com/4.12/wazuh-install.shStep 3 — Download Wazuh Installerbashcurl -sO https://packages.wazuh.com/4.12/wazuh-install.shStep 4 — Execute All-in-One Installationbashsudo bash wazuh-install.sh -aInstallation duration: Approximately 15-25 minutes depending on network throughput.Step 5 — Retrieve Admin Credentialsbashsudo tar -O -xvf wazuh-install-files.tar wazuh-install-files/wazuh-passwords.txtSave the admin password securely — required for dashboard login.6. Dashboard Access & VerificationAfter installation completes, verify the Wazuh stack status:bashsudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboardAccess URL:javascripthttps://localhostDefault Credentials:Username: adminPassword: <retrieved from wazuh-passwords.txt>Accept the self-signed certificate warning in the browser to proceed.7. Troubleshooting SummaryIssueCauseSolutionSlow internet in VMBridged mode + router DNS latencySwitched to NAT modeClipboard not workingUbuntu Server has no GUIImplemented SSH accessDashboard not reachableNAT isolationPort Forwarding 443 -> 443SSH connection refusedPort Forwarding rule missingAdded 2222 -> 22 ruleVM not getting IPAdapter type mismatchChanged to Intel PRO/1000 MT8. Future WorkAgent Deployment: Install Wazuh Agent on Windows 11 host endpoint. Forward ports 1514 (TCP/UDP) and 1515 (TCP) for agent enrollment.Endpoint Monitoring: Configure Sysmon and File Integrity Monitoring (FIM) for Windows.Detection Engineering: Create custom rules for brute-force attacks, malware execution, and unauthorized logins.Compliance & Reporting: Enable MITRE ATT&CK mapping, vulnerability detection, and automated compliance reports.Scalability: Evaluate distributed deployment with separate Manager, Indexer, and Dashboard nodes for production.Project StatusPhase 1 — Infrastructure & Installation: ✅ CompletedPhase 2 — Agent Integration: ⏳ In ProgressPhase 3 — Detection & Use Cases: ⬜ PendingAuthor: Wazuh SIEM DeploymentDate: October 2026License: For Academic / Final Year Project Usejavascript
Ye final file hai. Isme:
- Heading + Overview
- VM setup se leke CMD SSH tak ke saare steps
- NAT, Port Forwarding, Clipboard issue ka professional explanation
- Installation commands
- Troubleshooting table aur Future Work

Ab isko copy karo, Notepad me `README.md` ke naam se save karo, aur apni GitHub repo ya FYP document me use kar lo.