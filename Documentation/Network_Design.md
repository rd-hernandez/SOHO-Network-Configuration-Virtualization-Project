# **Network Design** 

# **1. Overview** 

This project involved designing and deploying a virtual Small Office/Home Office (SOHO) network using Proxmox VE, pfSense, and Windows Server. The goal was to create a segmented network environment that provides centralized network services while maintaining security between different network segments. 

Proxmox VE was used as the hypervisor to host the network infrastructure. pfSense was deployed as the virtual router and firewall, while Windows Server provided centralized DHCP and DNS services. 

The network was divided into three VLANs, each assigned a separate subnet to isolate employee devices, servers, and network management. 

# **2. Network Architecture** 

# **Network Topology** 

The network uses a physical connection from the home router through a network switch to the Proxmox server. Proxmox hosts the virtual machines responsible for managing the network. 

pfSense handles network routing and firewall policies, while Windows Server provides DHCP and DNS services to network clients. 

# **Network Diagram:** 

# **Network Components** 

**Component Purpose** Proxmox VE Hypervisor used to host virtual machines pfSense Virtual router and firewall 

Windows Server Centralized DHCP and DNS services vmbr1 VLAN-aware Proxmox network bridge 

VLAN 10 Employee network VLAN 20 Server network VLAN 30 Management network 

# **3. Network Infrastructure** 

# **Proxmox Hypervisor** 

Proxmox VE was deployed as the hypervisor to host the virtual network infrastructure. A VLAN-aware network bridge (vmbr1) was configured to support network segmentation and allow virtual machines to communicate across the configured VLANs. 

VLAN tagging was used to assign virtual network interfaces to their respective network segments. 

# **pfSense Router and Firewall** 

pfSense was deployed as a virtual machine to provide routing and firewall services for the network. 

Its responsibilities include: 

- Managing network traffic between VLANs. 

- Providing default gateways for each subnet. 

- Enforcing firewall rules to restrict unauthorized inter-VLAN communication. 

- Relaying DHCP requests to Windows Server through DHCP Relay. 

Each VLAN was assigned a dedicated pfSense interface with a static IPv4 address serving as its default gateway. 

# **Windows Server** 

Windows Server was deployed on VLAN 30 (Management) to provide centralized DHCP and DNS services. 

# **DHCP:** 

Windows Server manages IP address allocation for the employee network (VLAN 10). A DHCP scope was configured with an address range of 10.0.10.2 – 10.0.10.100. 

Because the DHCP server resides on a different VLAN than the employee clients, DHCP Relay was configured on pfSense to forward DHCP requests between network segments. VLAN 20 and VLAN 30 use static IP addressing rather than DHCP. 

# **DNS:** 

Windows Server also provides DNS services for network clients, allowing devices to resolve hostnames to IP addresses. 

# **4. IP Addressing and Network Segmentation** 

The network uses IPv4 addressing with three separate subnets. Each subnet is associated with a dedicated VLAN and default gateway. 

# **VLAN ID VLAN Name Subnet Default Gateway** 

10 Employees 10.0.10.0/24 10.0.10.1 

20 Servers 10.0.20.0/24 10.0.20.1 

- 30 Management 10.0.30.0/24 10.0.30.1 

Each VLAN serves a different purpose: 

- **VLAN 10 – Employees:** Provides network access to employee devices using DHCP. 

- **VLAN 20 – Servers:** Separates server infrastructure from employee devices. Devices use static IP addresses. 

- **VLAN 30 – Management:** Provides a dedicated network segment for network management and Windows Server services. 

See VLAN Configuration for detailed VLAN settings. 

# **5. Network Traffic Flow** 

Network traffic is managed through pfSense, which acts as the default gateway for each VLAN. 

The following describes how network services operate: 

# **DHCP Address Assignment:** 

1. An employee device connects to VLAN 10. 

2. The device broadcasts a DHCP request. 

3. pfSense DHCP Relay forwards the request to Windows Server on VLAN 30. 

4. Windows Server assigns an available IP address from the configured DHCP scope. 

5. The DHCP response is relayed back to the requesting client. 

# **DNS Resolution:** 

1. A client sends a DNS query to Windows Server. 

2. The query travels through pfSense to VLAN 30. 

3. Windows Server processes the DNS request and returns the corresponding IP address. 

# **Inter-VLAN Communication:** 

Traffic between VLANs passes through pfSense, where firewall rules determine whether communication is permitted or denied. Unauthorized traffic is restricted to maintain network segmentation. 

# **6. Network Security Design** 

Network segmentation was implemented to separate employee devices, servers, and management services into individual network segments. 

pfSense firewall rules were configured to restrict unauthorized communication between VLANs. 

The network follows a default-deny approach to inter-VLAN communication, allowing only necessary traffic required for network services. 

This approach helps: 

- Limit unnecessary communication between network segments. 

- Isolate server infrastructure from employee devices. 

- Restrict access to network management services. 

- Reduce the potential impact of compromised devices. 

See pfSense Firewall Confgurationi for detailed firewall rules. 

# **7. Testing and Verification** 

Network testing was performed to verify the functionality of the network infrastructure and its security configurations. 

The following areas were tested: 

- DHCP address assignment from Windows Server. 

- DNS resolution using Windows Server. 

- Connectivity to configured default gateways. 

- Communication between devices on the same VLAN. 

- Firewall restrictions between VLANs. 

- Network segmentation and routing functionality. 

All connectivity tests were completed successfully. 

See Connectivity Testing for detailed test results. 

# **8. Conclusion** 

The SOHO network was successfully designed and deployed using Proxmox VE, pfSense, and Windows Server. The implementation provides a virtualized network environment with three separate VLANs for employee devices, servers, and network management. pfSense manages routing, firewall policies, and DHCP Relay, while Windows Server provides centralized DHCP and DNS services. Network segmentation and firewall restrictions help improve network organization, control traffic between network segments, and strengthen the overall security of the environment. 

