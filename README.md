# **CYBERSECURITY-HOME-LAB**
Describing and documenting the process of deploying a HomeLab for security research and training. 
This is for anyone looking to get into the world of InfoSec/Cybersecurity

## **OVERVIEW**

This project outlines the entirety of building and deploying a lab environment within my home network while keeping it isolated and without being overly dependent on 3rd party virtualization. I am prioritizing the use of physical hardware to better understand the behavior, maintenance, and problems with on-premise equipment in a simulated enterprise network. Furthermore, the prioritization of physical hardware helps me become more aware of hardware considerations and limitations when deploying new services, applications, hardware compenents, etc... Virtualization will be a big part of this lab but it will be locally hosted on specially procured hardware making me responsible for updating and securing the machines and services on them.

## **NETWORK ARCHITECTURE & INFRASTRUCTURE**
*Detailed change request documents and network diagrams can be found in* (ATTACHMENT)

**Before launching my home lab I had to consider how this lab's traffic would flow and can my existing infrastructure support it**
- Considering the nature of the lab I'd like to have it be in as little contact with my existing home network. To achieve that isolation I'll need more advanced features from my networking devices in the internal (Home) network. These devices must support VLANS, 802.1q tagging, Port Forwarding, Firewall ACL Rules, stronger security protocols like WPA3, and Multiple broadcast domains.
- My existing infrastructure was an ISP-provided modem and Soho router with basic features. The ISP keeps the admin interface for the router pretty locked down and significantly limits control over the network. A new router will need to be purchased along with a layer 2 switch for further isolation and ACL configuration.
- Consumer hardware will allow for more control than the locked down ISP router/modem but still lack advanced networking features. Enterprise hardware will solve all our networking needs but is not a cost-effective solution. Business-class solutions in this case will give us the advanced networking features we need while being more cost-effective
- TP-Link has business-class networking solutions and will provide the necessary upgrades in my current network infrastructure and allow centralized control of the network

### Hardware & Software Allocation 

**Infrastructure allocation**
    
    - TP-Link Router 802.11AX WAP, 1x10Gb/s SFP, 5x1Gb/s RJ-45 LAN/WAN  
    - TP-Link Switch(managed/L2) 8x1Gb/s RJ-45 ports supporting advanced security features: ACLs, 802.1q, STP, LAG
    - CAT6 550MHz FTP shielded cabling

**Lab Devices**
    
    - 802.11AC WAP Router, 3x1Gb/s RJ-45 ports, VPN server, WPA2/WPA3, 2.4Ghz & 5Ghz
    - L2 Managed switch with VLAN and Port Mirroring/SPAN support
    - Sacrificial Clients
    *Vulnerable Windows client (physical device)*
    *Vulnerable Linux DesktopVMs*
    *Vulnerable server VMs*
    - Administration interfaces
    *Fedora VMs across different computers for Remote/LAN access via VPN or ethernet*
    - Virtualization Servers (Repurposed and Upgraded Mini PCs)
    *Server 1: 14th Gen U9-185H CPU, 64GB DDR5 RAM, 3TB NVME, 2x RJ-45 NICs*
    *Server 2: i7-1195G7 CPU, 64GB DDR4 RAM, 2TB NVME, 1x RJ-45 NIC*
    - Kali Linux laptop for penetration testing exercises & vulnerability scanning
  
  **Lab software**:
    
    - Security Onion SIEM
    - Elastic Fleet/Agents
    - Kibana
    - Proxmox VE type 1
    - Linux, Windows, Mac OSs
    - T-pot
    - Zeek
    - Suricata
  
### Infrastructure Upgrades
Since my home network is "technically" a production environment where other household members are WFH Employees & Business Contractors.
I cannot spontaneously push untested upgrades into our internal network since there is potential for monetary loss.
    - Scheduled network downtime when no business operations are being conducted
    - Used network diagrams to plan, configure, and deploy newly designed network architecture
    - New architecture calls for a segmented internal network to improve security with separate broadcast domains and enforce them with other security features
    - Untrusted networks will have the below security measures
      - *Security Configuration: WPA2, Captive Portals, 802.1x, ACLs, Complex passwords, UTM Suite, IPS/IDS, VPN w/RADIUS authentication, and SIEM Monitoring*
    - Trusted networks will have the below security measures
      - *Security Configuration: WPA3, Complex passwords, ACLs, etc..* (not great practice to publish all my security measures)

### Architecture planning
Following my new network design this is a brief overview of the planned architecture
*once again more detailed and technical documents can be found in (attatchment)*

### Network Segments
802.1q VLANs may be commonplace in enterprise environments but, having them work properly with other security features on non-enterprise hardware 
can be tedious considering (in my case) the entire network needed to be redesigned with brand-new hardware

|**VLAN ID**| **PURPOSE** | **DEVICES**|
 |----------|-------------|------------|
 | **1**    | *INTERNAL*  | Main Router/Switches, PCs, Phones, etc... |
 | **2**    | *IoT*       | IP Cams, Appliances, etc... |
 | **3**    | *GUEST*     | Any device guest device |
 | **4**    | *LAB*       | Virtualization Servers, Router, L2 Switch |
 | **5**    | *HONEY NET* | T-pot VMs |
 | **6**    | *SAN*       | Family File Servers, Media Servers, Archive Storage | 

## **DEPLOYMENT**
Deployment of this new network wasn't without its challenges but, it was mostly due to the fact that I was transitioning, what was a generic star soho network into a secure, and highly specialized environment.

### **First Phase: Infrastructure Upgrade**
1. There is no ethernet wiring throughout my home and the wiring that supports the current network was in another room so, in a somewhat crude manner I simply cut a hole through the wall, and added low-voltage mounting plates to run an ethernet cable where the lab will be located.
2. After the wiring was in place and the maintenance window was open I began powering up my new networking devices Main Router and Switch while leaving the current set up active.
3. Following the plan I previously designed, I began segmenting the network and establishing IP ranges for each segmentation
4. Once the new router had the proper network configurations I powered down my old router, reset my modem, and connected the new networking devices
5. From there I went around my home connecting all devices to their designated segment and ensuring they were within their proper IP range.

### **Second Phase: Security Testing**
1. Once WAN connectivity was established my next step was to implement and test ACLs to ensure isolation *see (Attatchment) for more details*
2. Testing the network's isolation from other segments began with pinging devices to ensure those devices/networks were unreachable even with ICMP.
3. Next I conducted a double tagging VLAN attack on my switches to ensure there were no misconfigurations with trunking and tagging.
4. Finally I attempted to crack and brute force network segments utilizing the WPA2 protocol to verify the PSK complexity was sufficient.

### **Third Phase: Lab Deployment**
*To support the lab I envisioned with a wide variety of tools and services, in my attempt to replicate an enterprise network. I was going to require powerful and versatile computing resources.*
1. Two PCs needed an upgrade since their stock components weren't sufficient for my purposes. Each was upgraded to 64GB RAM totaling 128GB of memory between my two virtualization servers.
2. I wiped the internal storage devices on both PCs using AOMEI Partition assistant as a security measure since they were acquired on the 2nd hand market.
3. Internal 802.11 devices were removed to limit my attack surface from external threats since configurations on this network segment at times will be purposefully vulnerable for experimentation
4. Using a spare NVMe SSD & M.2 enclosure I flashed a Proxmox VE ISO onto it to create the installation media for my type 1 hypervisor servers.
5. Proper installation of Proxmox VE requires simple hardware configuration in the UEFI settings *see (attachment) for more*
6. Next I needed to deploy the lab's dedicated networking infrastructure: 1xRouter, 1xSwitch, 7xCAT6 Patch cables.
7. Wiring goes as follows: INTERNAL NETWORK> LAB WAP/ROUTER> LAB SWITCH> VIRTUALIZATION SERVERS & WIRED/WIRELESS CLIENTS
8. Now that the lab is running with properly configured infrastructure and security measures Tools, Services, Apps, and workstations can be deployed from PVE servers

### **Fourth Phase: End-point and Security tools**
*content in this section is in regards to the network segment dedicated to the cybersecurity lab environment*
1. First I read the documentation for the tools I had decided on to get more insight into how they work and to better troubleshoot any issues that may come
2. One of the main tools I wanted in this lab was a SIEM and Security Onion (Standalone) seemed like the most complete and scalable solution. Their documentation was very clear so deployment was pretty smooth.
3. Security Onion VM: 2xNICs 1(Management)/2(Monitoring) in promiscuous mode, 24GB RAM, 8 CORE CPU, 250GB STORAGE W/ SSD EMULATION. *(NOTE)S.O requires 2xNICs one of which needs to physically connect to a SPAN*
4. Workstation VMs: UBUNTU, MINT, FEDORA
5. Physical Clients: MacBook Air(ADMIN), Fedora 41(ANALYST), WINDOWS10 MACHINE(VICTIM CLIENT), KALI(ATTACKER).
6. OPNsense VM: 2xNICs Management/Monitoring, 16GB RAM, 8 CORE CPU, 250GB STORAGE
7. This OPNsense VM will be configured as a UTM (unified threat management) suite consisting of: VPN, FIREWALL, WEB PROXY, IDS/IPS, VLAN SUPPORT
8. Lastly T-Pot, A honeypot hosting platform for exposing vulnerable machines to the WAN in order to analyze attacker telemetry.
9. T-POT VM: 16GB RAM, 8 CORE CPU, 500GB STORAGE

# **CONCLUSION**

While this project was resource-intensive, There are many ways to deploy your own home lab and gain technical knowledge about security tools. 
You can do all this and more with cloud service providers or repurpose and upgrade old/inexpensive hardware to suit your needs. My approach
was meant to prioritize versatility and integrity while remaining cost-effective. I am satisfied with the result of my lab and have had a lot of fun
putting it to use since its completion. If you're just starting out in cybersecurity, I believe building your own lab environment can be an indispensable 
learning tool for practical knowledge outside of a textbook!

# ABOUT ME
i like computer

Linkedin: https://www.linkedin.com/in/h-julian-sanchez-92a10a24a/

Contact me: julian@darkhorizonit.tech


