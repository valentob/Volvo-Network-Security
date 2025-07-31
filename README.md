# Volvo CE iAssist Network Security Simulation (Cisco Packet Tracer)

## Project Overview

This repository contains a Cisco Packet Tracer simulation of the Volvo Construction Equipment (VCE) iAssist network, designed to demonstrate and analyze various cybersecurity measures in an industrial IoT (IIoT) environment. The simulation is based on a comprehensive security report that details the network architecture, key data flows, assets, threat analysis, risk assessment, and recommended security controls for the iAssist functionality.

The primary goal of this project is to provide a practical, hands-on environment to visualize and understand the implementation of network security principles, including network segmentation (VLANs), firewall rules, Network Access Control (NAC), and secure communication protocols, within a simulated operational technology (OT) setting.

## Background (from the Security Report)

The iAssist functionality at Volvo CE involves real-time feedback to machine operators based on sensory data from construction machines (MCHs). This system relies on a robust and secure network infrastructure, leveraging advanced wireless technologies like 5G. The original security report identified critical assets and potential vulnerabilities, emphasizing the need for a layered defense strategy to ensure the confidentiality, integrity, and availability of iAssist data and services.

Key aspects addressed in the simulation reflect the report's recommendations:

*   **Network Segmentation:** Utilizing VLANs to isolate different functional segments (e.g., HMI, Site Controller, Fog/Edge Server, MCH network).
*   **Multi-layered Firewalling:** Implementing two distinct firewalls to protect the perimeter from external threats and to secure the internal machine network.
*   **Secure Communication:** Emphasizing HTTPS for critical services and preparing for encrypted sensor data flows.
*   **Access Control:** Simulating aspects of Network Access Control (NAC) to prevent unauthorized device access.

## Network Topology

The simulated network topology in Cisco Packet Tracer mirrors the architecture described in the security report. It includes:

*   **VCE Back Office:** Representing the cloud solution for long-term analytics and storage.
*   **Internet Router:** Simulating external WAN/ISP connectivity.
*   **First Firewall (Perimeter):** Defending the construction site from general internet threats.
*   **Inside Router:** Facilitating routing within the site network and inter-VLAN communication.
*   **Site Switch:** Centralizing connectivity for internal devices and implementing VLAN segmentation.
*   **HMI (Human-Machine Interface):** For on-site operator interaction.
*   **Site Controller:** For safety signals and control.
*   **Fog/Edge Server:** Hosting the iAssist service, performing local data processing and analytics.
*   **Second Firewall (Internal):** Dedicated to securing the on-site machine environment.
*   **5G_Tower (WRT300N):** Simulating the 5G access point for wireless MCH connectivity.
*   **MCHs (Machines):** Representing the construction equipment transmitting sensory data.





## Setup Instructions

To set up and run this simulation, you will need Cisco Packet Tracer installed on your system. This project is provided as a `.pkt` file, which is the native file format for Cisco Packet Tracer.

### Prerequisites

*   **Cisco Packet Tracer:** Ensure you have Cisco Packet Tracer installed. This simulation was built and tested with a recent version of Packet Tracer (e.g., 7.x or 8.x).

### Getting Started

1.  **Download the `.pkt` file:** Obtain the `volvo_ce_iassist_network.pkt` file from this repository.
2.  **Open in Packet Tracer:** Launch Cisco Packet Tracer and open the downloaded `.pkt` file. The network topology with all devices, connections, and initial configurations should load.

### Key Configuration Details

This section outlines the critical configurations applied to the devices in the Packet Tracer file. These configurations are based on the detailed instructions provided in the security report and were implemented step-by-step.

#### IP Addressing Scheme

The network utilizes a structured IP addressing scheme with `/30` and `/29` subnets for efficient resource allocation and network segmentation.

| Device / Interface          | IP Address      | Subnet Mask       | Default Gateway   |
| :-------------------------- | :-------------- | :---------------- | :---------------- |
| **VCE Back Office**         | 192.168.5.2     | 255.255.255.252   | 192.168.5.1       |
| **Internet Router**         |                 |                   |                   |
|   - G0/0/0 (to VCE Back Office) | 192.168.5.1     | 255.255.255.252   | N/A               |
|   - G0/0/1 (to First Firewall) | 192.168.0.1     | 255.255.255.252   | N/A               |
| **First Firewall**          |                 |                   |                   |
|   - G1/1 (outside)          | 192.168.0.2     | 255.255.255.252   | N/A               |
|   - G1/2 (inside)           | 192.168.2.2     | 255.255.255.252   | N/A               |
| **Inside Router**           |                 |                   |                   |
|   - G0/0/0 (to First Firewall) | 192.168.2.1     | 255.255.255.252   | N/A               |
|   - G0/0/1 (trunk to Site Switch) | (Subinterfaces) | (Subinterfaces)   | N/A               |
|     - G0/0/1.10 (VLAN 10)   | 192.168.1.1     | 255.255.255.248   | N/A               |
|     - G0/0/1.20 (VLAN 20)   | 192.168.1.1     | 255.255.255.248   | N/A               |
|     - G0/0/1.30 (VLAN 30)   | 192.168.1.1     | 255.255.255.248   | N/A               |
| **Site Switch**             |                 |                   |                   |
|   - F0/1 (HMI)              | N/A             | N/A               | 192.168.1.1       |
|   - F0/2 (Site Controller)  | N/A             | N/A               | 192.168.1.1       |
|   - F0/3 (Fog/Edge Server)  | N/A             | N/A               | 192.168.1.1       |
| **HMI**                     | 192.168.1.5     | 255.255.255.248   | 192.168.1.1       |
| **Site Controller**         | 192.168.1.4     | 255.255.255.248   | 192.168.1.1       |
| **Fog/Edge Server**         | 192.168.1.2     | 255.255.255.248   | 192.168.1.1       |
| **Second Firewall**         |                 |                   |                   |
|   - G1/1 (inside_site)      | 192.168.3.2     | 255.255.255.252   | N/A               |
|   - G1/2 (outside_mch)      | 192.168.3.1     | 255.255.255.252   | N/A               |
| **5G_Tower (WRT300N)**      |                 |                   |                   |
|   - Internet Port           | 192.168.3.1     | 255.255.255.252   | 192.168.3.2       |
|   - LAN (DHCP Pool)         | 192.168.10.1    | 255.255.255.224   | N/A               |
| **MCHs (Wireless)**         | DHCP (192.168.10.x) | DHCP (255.255.255.224) | 192.168.10.1      |

#### VLAN Configuration (Site Switch)

*   **VLAN 10:** Default/Data VLAN (for Fog/Edge Server)
*   **VLAN 20:** HMI VLAN (port F0/1)
*   **VLAN 30:** Site Controller VLAN (port F0/2)
*   **Trunk Ports:** G0/1 (to Inside Router) and G0/2 (to Second Firewall) are configured as 802.1Q trunks to carry all VLAN traffic.

#### Firewall Rules (ASA 5506-X)

*   **First Firewall:**
    *   **NAT:** Configured for dynamic PAT (Port Address Translation) to allow internal networks to access external resources via the firewall's outside interface IP.
    *   **Security Levels:** `outside` interface (G1/1) has security-level 0; `inside` interface (G1/2) has security-level 100.
*   **Second Firewall:**
    *   **NAT:** Configured for dynamic PAT to allow the MCH network to access resources via the firewall's `outside_mch` interface IP.
    *   **Security Levels:** `outside_mch` interface (G1/2) has security-level 0; `inside_site` interface (G1/1) has security-level 100.
    *   **Access Control List (ACL):** An ACL named `MCH_TO_FOG` is applied inbound on the `outside_mch` interface to permit TCP traffic on port 443 (HTTPS) from the MCH network (192.168.10.0/27) to the Fog/Edge Server (192.168.1.2). This simulates allowing only necessary protocols.

#### Wireless Configuration (5G_Tower - WRT300N)

*   **SSID:** `5G_Tower`
*   **Security:** WPA2 Personal with passphrase `VolvoCE_Secure`.
*   **DHCP Server:** Enabled, providing IP addresses in the `192.168.10.0/27` range.

#### Server Configurations

*   **Fog/Edge Server:**
    *   **DNS Service:** Configured to resolve `www.iassist.com` to `192.168.1.2`.
    *   **HTTP Service:** Disabled.
    *   **HTTPS Service:** Enabled (simulating the iAssist web service).

#### Router Configurations

*   **Static Routes:** Configured on both the Internet Router and Inside Router to ensure full reachability across the network segments and through the firewalls.





## How to Test the Simulation

Once the Packet Tracer file is loaded, you can perform various tests to verify the network functionality and security measures.

### 1. Verify Basic Connectivity (Ping Tests)

Use the `ping` command from the `Command Prompt` on end devices (HMI, Site Controller, VCE Back Office) to test connectivity.

*   **From HMI (Desktop -> Command Prompt):**
    *   `ping 192.168.1.2` (Ping Fog/Edge Server - should be successful)
    *   `ping 192.168.1.4` (Ping Site Controller - should be successful)
    *   `ping 192.168.10.2` (Ping MCH1 - should be successful, demonstrating communication through the Second Firewall)
    *   `ping 192.168.0.1` (Ping Internet Router - should be successful, demonstrating communication through the First Firewall)

*   **From MCH1 (Config -> Wireless0 -> Command Prompt - *if available, otherwise assume connectivity if IP is assigned*):**
    *   `ping 192.168.1.2` (Ping Fog/Edge Server - should be successful, demonstrating MCH to Fog/Edge communication through the Second Firewall)

*   **From VCE Back Office (Desktop -> Command Prompt):**
    *   `ping 192.168.0.1` (Ping Internet Router - should be successful)

### 2. Test iAssist Service (HTTPS)

Verify that the iAssist web service on the Fog/Edge Server is accessible via HTTPS and DNS.

*   **From HMI (Desktop -> Web Browser):**
    *   In the URL bar, type `https://www.iassist.com` and press Enter.
    *   You should see the default Packet Tracer web server page, confirming successful DNS resolution and HTTPS connectivity.

### 3. Observe Wireless Connectivity

*   Visually confirm the dotted lines connecting the `MCH` devices to the `5G_Tower` (WRT300N) in the logical workspace.
*   On the `MCH` devices, go to `Config` tab -> `Wireless0` and verify that they have obtained IP addresses in the `192.168.10.x` range via DHCP.

### 4. Test Firewall Functionality

*   **First Firewall:** Attempt to initiate connections from the `Internet Router` side to internal networks that are not explicitly permitted. You should observe that these connections are blocked by the firewall.
*   **Second Firewall:** Attempt to initiate connections from the `Site Switch` side (e.g., from HMI) to the MCH network (192.168.10.x) on ports other than 443 (if you implemented stricter ACLs). These should be blocked.

### 5. Test VLAN Segmentation

*   On the `Site Switch`, use the `show vlan brief` command in the CLI to verify that the HMI, Site Controller, and Fog/Edge Server ports are correctly assigned to their respective VLANs (VLAN 20, VLAN 30, and VLAN 10/default).
*   Attempt to ping devices within the same VLAN (e.g., HMI to another device in VLAN 20, if you added one) and then across VLANs (e.g., HMI to Site Controller). Inter-VLAN communication should be successful due to the Inside Router performing routing.





## Limitations of the Simulation

While this Packet Tracer simulation provides a valuable representation of the Volvo CE iAssist network and its security measures, it's important to note the inherent limitations of network simulation software:

*   **Packet Tracer Simplifications:** Cisco Packet Tracer simplifies many real-world networking and security functionalities. For instance, the ASA firewall model in Packet Tracer does not support the full range of commands and features found in a physical ASA device. Similarly, 802.1X NAC implementation is highly simplified.
*   **No Real-World Traffic:** The simulation does not generate actual sensor data or iAssist application traffic. Connectivity tests are based on ICMP (ping) and basic application layer protocols (HTTP/HTTPS).
*   **Performance:** Packet Tracer does not accurately simulate network performance metrics like latency, throughput, or packet loss under heavy load.
*   **Advanced Security Features:** Complex security features such as Intrusion Detection/Prevention Systems (IDS/IPS), advanced malware protection, or detailed logging and monitoring are not fully functional or are represented conceptually.

For a more in-depth analysis or real-world testing, physical lab environments or more advanced network emulation/simulation platforms would be required.

## Acknowledgments

This Packet Tracer simulation is done for the "Network Security – DVA 498" course by **Haris Adrović** and **Valento Bardhoshi**.

---
