# **VLAN Configuration** 

# **1. Overview** 

- Explain the purpose of VLANs in your network. 

- Describe how VLANs provide network segmentation. 

# **2. VLAN Design** 

Include a table describing each VLAN. 

|**VLAN ID **|**VLAN Name**|**Subnet**|**Default Gateway**|
|---|---|---|---|
|10|EMPLOYEES|10.0.10.0/24|10.0.10.1|
|20|SERVERS|10.0.20.0/24|10.0.20.1|
|30|MANAGEMENT|10.0.30.0/24|10.0.30.1|



# **3. VLAN Configuration** 

# **VLAN 10 - EMPLOYEES** 

- VLAN ID: 10 

- Subnet: 10.0.10.0/24 

- Default Gateway: 10.0.10.1 

- DHCP Range: 10.0.10.2 – 10.0.10.100 

- Purpose: VLAN 10 is configured to allow for employees joining and leaving the network by using DHCP 

# **Configuration Details:** 

<u>pfSense -</u> 



<!-- Start of picture text -->
General Configuration<br>Enable @ Enable interface<br>Description Employees<br>IPv4 Configuration Type Static IPv4 bs<br>IPv6 Configuration Type None v<br>MAC Address<br>The MAC address ofa VLAN interface must beset on its parent interface<br>MTU<br>MSS<br>‘Speed and Duplex Default (no preference, typically autoselect v<br>Static IPv4 Configuration<br>IPv4 Address 100901<br><!-- End of picture text -->

Using pfSense, the Employees interface is configured using a static IPv4 address of 10.0.10.1 

# - <u>Proxmox configuration</u> 



<!-- Start of picture text -->
t. Network Device<br>Bridge: vmbr1<br>VLAN Tag 10<br><!-- End of picture text -->

Bridge vmbr1(LAN) is tagged with the VLAN 10 Tag 

# - <u>WinServer DHCP confgurationi</u> 



<!-- Start of picture text -->
Scope [10.0.10.0] Employees Scope Properties ? x<br>General DNS Advanced<br>ee<br>StatIP address: | 10.0 . 10. 2<br>End IP address: wW. 0. 10.100<br>Subnet mask 255 .255. 255. 0 Length: 24<br>‘Lease duration for DHCP cients —$ $<br>© Limited to:<br>Days: Hours: Minutes:<br>2 Pos} [os<br>© Unlimited<br>Description: _[IPv4 Scope for VLAN 10<br><!-- End of picture text -->

Windows Server DHCP scope for EMPLOYEES subnet is configured with a scope of 10.0.10.2 – 10.0.10.100 

# **VLAN 20 - SERVERS** 

- VLAN ID: 20 

- Subnet: 10.0.20.0/24 

- 

- 

   - Default Gateway: 10.0.20.1 

   - DHCP Range: DHCP not configured 

- Purpose: Separate VLAN for servers to safely store and compute data. DHCP disable to keep static IPs on servers 

# **Configuration Details:** 

- <u>pfSense</u> 



<!-- Start of picture text -->
General Configuration<br>Enable — @ Enable interface<br>Description Servers<br>IPv4 Configuration Static IPv4. v<br>Type<br>IPV6 Configuration None v<br>Type<br>MAC Address<br>The MAC address of a VLAN interface must be seton its pa<br>MTU<br>Mss<br>f<br>a value is entered in this field, then MSS clampingfor TCP<br>TCP/IPv4 header size) and minus 60 for IPv6 (TCP/IPv6 he:<br>Speed and Duplex Default (no preference, typically autoselect! v<br>Static IPv4 Configuration<br>IPv4 Address 10.0.20.1<br><!-- End of picture text -->

Using pfSense, the Employees interface is configured using a static IPv4 address of 10.0.20.1 

# - <u>Proxmox configuration</u> 



<!-- Start of picture text -->
Edit: Network Device<br>Bridge: vmbr1<br>VLAN Tag: 20<br><!-- End of picture text -->

Bridge vmbr1(LAN) is tagged with the VLAN 20 Tag 

# **VLAN 30 – MANAGEMENT** 

- VLAN ID: 30 

- Subnet: 10.0.30.0/24 

- Default Gateway: 10.0.30.1 

- DHCP Range: DHCP not configured 

- Purpose: A secure VLAN for management of the network to reside 

# **Configuration Details:** 

- <u>pfSense</u> 



<!-- Start of picture text -->
General Configuration<br>Enable Enable interface<br>Enter a description (name) for the interface here.<br>IPv4 Configuration Static 1Pv4 v<br>Type<br>IPV6 Configuration None v<br>Type<br>MAC Address IOC IOC<br>The MAC address of a VLAN interface must be set on its pai<br>MTU<br>If this field is blank, the adapter’s default MTU will be used. 7<br>Mss<br>if a value is entered in this field, then MSS clamping for TCP<br>(TCP/IPv4 header size) and minus 60 for IPv6 (TCP/IPV6 hei<br>‘Speed and Duplex Default (no preference, typically autoselect) v<br>Explicitly set speed and duplex mode for this interface.<br>WARNING: MUST be set to autoselect (automaticall][ negoti:<br>speed and duplex forced<br>Static IPv4 Configuration<br>IPv4 Address 10.0 30.7<br><!-- End of picture text -->

Using pfSense, the Employees interface is configured using a static IPv4 address of 10.0.10.1 

- <u>Proxmox configuration</u> 



<!-- Start of picture text -->
Edit: Network Device<br>Bridge: vmbr1|<br>VLAN Tag 30<br><!-- End of picture text -->

Bridge vmbr1(LAN) is tagged with the VLAN 20 Tag 

# **6. Verification** 

How VLAN configurations were verified 

- Confirmed clients received the correct IP addresses. 

- Verified default gateway connectivity. 

- Tested communication between devices on the same VLAN. 

- Verified DNS resolution. 

- Tested firewall rules between VLANs. 

See Connectivity Testing for detailed test results. 

# **7. Conclusion** 

Three VLANs were successfully configured using pfSense and Proxmox to separate employee devices, servers, and network management into individual subnets. Each VLAN was assigned its own default gateway, with DHCP configured for the Employees network and static IP addressing used for the Servers and Management networks. Firewall rules were implemented to restrict unauthorized communication between VLANs while allowing necessary network services. These configurations improved network organization, provided greater control over network traffic, and added an additional layer of security through network segmentation. 

