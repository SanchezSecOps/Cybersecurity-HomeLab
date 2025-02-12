# **CYBERSECURITY-HOME_LAB**
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

### Hardware/Software Allocation 
**Infrastructure allocation**:
    - TP-Link Router 802.11AX WAP, 1x10Gb/s SFP, 5x1Gb/s RJ-45 LAN/WAN  
    - TP-Link Switch(managed/L2) 8x1Gb/s RJ-45 ports supporting advanced security features: ACLs, 802.1q, STP, LAG
    - Bulk STP CAT6 Cabling
    - RJ-45 Connectors
    - Driverless RJ-45 to USB adapters
**Lab Devices**:
    - 802.11AC WAP Router, 3x1Gb/s RJ-45 ports, VPN server, WPA2/WPA3, 2.4Ghz & 5Ghz
    - L2 Managed switch with VLAN and Port Mirroring/SPAN support
    - Sacrificial Clients
      - *Vulnerable Windows client (physical device)*
      - *Vulnerable Linux DesktopVMs*
      - *Vulnerable server VMs*
      - *Honeypot Platform T-Pot*
    - Administration interfaces
      - *Fedora Silverblue VMs across different computers for Remote/LAN access via VPN or ethernet*
    - Virtualization Servers (Repurposed and Upgraded Mini PCs)
      - *Server 1: 14th Gen U9-185H CPU, 64GB DDR5 RAM, 3TB NVME, 2x RJ-45 NICs*
      - *Server 2: i7-1195G7 CPU, 64GB DDR4 RAM, 2TB NVME, 1x RJ-45 NIC*
    - Kali Linux laptop for penetration testing excercises & vulnerability scanning
  **Lab software**:
    - Security Onion SIEM
    - Elastic Fleet/Agents
    - Kibana
    - Proxmox VE type 1
    - Linux, Windows, Mac OSs
    - T-pot Honey Pot Platform
    - Zeek
    - Suricata
  
### Infrastructure Upgrades
Since my home network is "technically" a production environment where other household members are WFH Employees & Business Contractors.
I cannot spontaneously push untested upgrades into our internal network since there is potential for monetary loss.
    - Scheduled network downtime when no business operations are being conducted
    - Used network diagrams to plan, configure, and deploy newly designed network architecture
    - New architecture calls for a segmented internal network to improve security with separate broadcast domains and enforcing them with other security features
    - Untrusted networks will have the below security measures
      - *Security Configuration: WPA2, Captive Portals, 802.1x, ACLs, Complex passwords, UTM Suite, IPS/IDS, VPN w/RADIUS authentication, and SIEM Monitoring*
    - Trusted networks will have the below security measures
      - *Security Configuration: WPA3, Complex passwords, ACLs, etc..* (not great practice to publish all my security measures)

### Architecture Implementation
Following my new network design this is a brief over view of the planned architechture
*once again more detailed and technical documents can be found in (attatchment)*

### Network Segments
802.1q VLANs may be common place in enterprise environments but, having them work nicely with other security features on consumer hardware 
can be a challenge considering how many peices of hardware I had to buy and test before landing on the devices listed above.

|**VLAN ID**| **PURPOSE** | **DEVICES**|
 |----------|-------------|------------|
 | **1**    | *INTERNAL*  | Main Router/Switches, PCs, Phones, etc... |
 | **2**    | *IoT*       | IP Cams, Appliances, etc... |
 | **3**    | *GUEST*     | Any device guest device |
 | **4**    | *LAB*       | Virtualization Servers, Router, L2 Switch |
 | **5**    | *SAN*       | Family File Servers, Media Servers, Archive Storage | 

## **DEPLOYMENT**
Deployment of this new network wasn't without its challenges but, it was mostly due to the fact that I was transitioning, what was a generic star soho network into a secure, and highly specialized enviornment.

### **First Phase: Infrastructure Improvements**
1. Wiring for this project was one of the more difficult aspects since my wireless router can only be in one spot and the new home I had just moved into wasn't outfitted with ethernet.
2. I purchased bulk STP CAT6 cable since interference was a concern since a new home surveillance solution I'm working on would require wiring over my kitchen and garage.
3. Once which rooms needed ethernet were decided on it was time to begin drilling and cutting through walls
4. After installing the new networking hardware and implementing the planned segmentation
5. -
