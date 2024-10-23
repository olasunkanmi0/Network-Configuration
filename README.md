# Network-Configuration
Network Configuration with Spanning Tree, VTP and Etherchannel.
overview: 
In this configuration, I set up a network environment involving three switches. The first switch acts as the VTP Server, the second switch operates as a VTP Client, and the third switch is set to VTP Transparent Mode. The configuration also includes Spanning Tree Protocol (STP) for loop prevention and EtherChannel for port aggregation to enhance bandwidth between switches.

Switches Involved:
Switch 1 (VTP Server)
Switch 2 (VTP Client)
Switch 3 (VTP Transparent Mode)
Step 1: VTP Configuration
Switch 1 (VTP Server):

Configure the switch as a VTP server and set the VTP domain name.
Set the VTP version (optional).
Set a password for secure VTP operation. 
Switch(config)# vtp mode server
Switch(config)# vtp domain NetworkLab
Switch(config)# vtp version 2
Switch(config)# vtp password secureVTP
Switch 2 (VTP Client):

Configure the switch as a VTP client.
Set the VTP domain name (same as server).
Set the VTP password (same as server).
Switch(config)# vtp mode client
Switch(config)# vtp domain NetworkLab
Switch(config)# vtp password secureVTP
Switch 3 (VTP Transparent Mode):

Configure the switch to transparent mode.
Set the VTP domain name.
Set the VTP password.
Switch(config)# vtp mode transparent
Switch(config)# vtp domain NetworkLab
Switch(config)# vtp password secureVTP

Step 2: VLAN Configuration on the VTP Server
On Switch 1 (VTP Server), create the necessary VLANs for the network.

Switch(config)# vlan 10
Switch(config-vlan)# name HR
Switch(config)# vlan 20
Switch(config-vlan)# name IT
Switch(config)# vlan 30
Switch(config-vlan)# name Finance

Verify that the VTP client (Switch 2) receives the VLANs from the server:
Switch# show vlan brief

Step 3: Spanning Tree Protocol (STP) Configuration
Enable Rapid Spanning Tree Protocol (RSTP) on all switches for faster convergence.
Switch(config)# spanning-tree mode rapid-pvst

On Switch 1 (VTP Server), set it as the primary root bridge for VLANs 10, 20, and 30.
Switch(config)# spanning-tree vlan 10,20,30 root primary

On Switch 2 (VTP Client), set it as the secondary root bridge for redundancy.
Switch(config)# spanning-tree vlan 10,20,30 root secondary

For Switch 3 (VTP Transparent), ensure STP is running but not affecting the root election.
Step 4: EtherChannel Configuration
On Switch 1 and Switch 2:

Configure EtherChannel on both Switch 1 and Switch 2 to bundle multiple interfaces into a single logical link for better bandwidth.
Switch(config)# interface range gigabitEthernet 1/1 - 2
Switch(config-if-range)# channel-group 1 mode active
Switch(config-if-range)# exit

Verify EtherChannel status:
Switch# show etherchannel summary

On both switches, ensure STP treats EtherChannel as a single link:
Switch(config)# spanning-tree portfast trunk

Step 5: Verification and Testing
VTP Status Check:

Ensure the VTP server, client, and transparent switch are working correctly.
Switch# show vtp status

VLAN Propagation:

Check if the VLANs created on the server are propagated to the client.
Switch# show vlan brief

Spanning Tree Status:

Verify STP root status and ensure there's no loop.
Switch# show spanning-tree

EtherChannel Verification:

Ensure that EtherChannel is functioning properly and that the interfaces are bundled.
Switch# show etherchannel summary



Conclusion:
This setup ensures a loop-free, redundant network with VLANs being efficiently propagated via VTP, and enhanced bandwidth through EtherChannel. Configuring STP ensures the network remains stable, preventing loops and enhancing fault tolerance.
