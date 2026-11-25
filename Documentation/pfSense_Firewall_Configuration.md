# **pfSense Firewall Configuration** 

# **1. Overview** 

pfSense runs as a virtual machine within Proxmox and provides routing and firewall functionality for the SOHO network. It manages network traffic between VLANs and applies firewall rules to control access to network resources. 

This document focuses on the pfSense interface assignments, firewall rules, DHCP Relay configuration, and traffic management. 

# **2. Interface Configuration** 

The pfSense virtual machine uses two virtual network interfaces. The WAN interface connects to the external network, while the LAN interface serves as the parent interface for the three VLANs. 

|**Interface**|**Network Po i**|**rt Configuration**|
|---|---|---|
|WAN|vtnet0|DHCP|
|LAN|vtnet1|Parent interface for VLANs|
|Employees|VLAN 10|vtnet1|
|Servers|VLAN 20|vtnet1|
|Management|VLAN 30|vtnet1|



The WAN interface obtains its IP configuration automatically through DHCP. The three VLAN interfaces are assigned to the LAN network interface and are managed separately within pfSense. 

# **3. Firewall Rules** 

Firewall rules control traffic entering and leaving each VLAN interface. Rules are configured to restrict communication between network segments while allowing required services. 

# **Employees** 

- Allow DNS traffic to the Management subnet using TCP/UDP port 53. 

- Block other traffic destined for the Management subnet. 

- Block traffic destined for the Servers subnet. 

- Allow remaining IPv4 traffic. 

# **Servers** 

- Allow DNS traffic to the Management subnet using TCP/UDP port 53. 

- Block other traffic destined for the Management subnet. 

- Block traffic destined for the Employees subnet. 

- Allow remaining IPv4 traffic. 

# **Management** 

- Block traffic destined for the Servers subnet. 

- Block traffic destined for the Employees subnet. 

- Allow remaining IPv4 traffic. 

The rules are evaluated in order, allowing specific traffic before applying broader restrictions. This provides control over inter-VLAN communication without preventing necessary network services. 

# **4. DHCP Relay Configuration** 

DHCP Relay is enabled on pfSense to forward DHCP requests from the Employees VLAN to the Windows Server DHCP service. 

The following configuration is used: 

|**Setting**|**Configuration**|
|---|---|
|DHCP Relay|Enabled|
|Downstream Interface|Employees|
|Upstream Server|10.0.30.5|



DHCP Server Windows Server 

Server VLAN Management (VLAN 30) 

This configuration allows devices on VLAN 10 to obtain their IP addresses from the Windows Server located on VLAN 30. 

# **5. Configuration Verification** 

The pfSense configuration was verified to ensure that interface assignments, firewall rules, and DHCP Relay were functioning as intended. 

Verification included: 

- Confirming the WAN and VLAN interface assignments. 

- Checking firewall rules for each VLAN. 

- Confirming DHCP Relay was enabled and pointed to the Windows Server. 

- Testing permitted and restricted network communication. 

The configuration worked as intended during testing. 

# **6. Conclusion** 

pfSense was configured to provide centralized routing and firewall management for the SOHO network. Firewall rules restrict unauthorized communication between VLANs while allowing necessary services, including DNS, to operate. DHCP Relay allows the Windows Server to provide IP addressing to clients on a separate VLAN. 

These configurations provide greater control over network traffic and help maintain separation between network segments. 

