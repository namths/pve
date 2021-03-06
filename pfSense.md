## Create bridge network vmbr1

auto vmbr1
iface vmbr1 inet static
        address 10.1.1.0/24
        bridge-ports none
        bridge-stp off
        bridge-fd 0

![Screen Shot 2021-09-20 at 17 25 54](https://user-images.githubusercontent.com/58029441/133987904-15356a41-da2f-4976-ae16-21b1cc245159.png)

## Download file .iso [pfSense](https://www.pfsense.org/download/) and Upload the iso file to PVE

# Create, Install, Configure the VM (pfSense)
![Screen Shot 2021-09-20 at 17 43 03](https://user-images.githubusercontent.com/58029441/133989912-c2d71a62-62bd-4e06-80f5-20d5961ffa9b.png)
![Screen Shot 2021-09-20 at 17 43 20](https://user-images.githubusercontent.com/58029441/133989942-82343f3e-82c1-4e70-a97a-15c8af489bc0.png)
![Screen Shot 2021-09-20 at 17 43 47](https://user-images.githubusercontent.com/58029441/133989991-9a5d475b-d79c-49a2-917e-72fd1e1c097a.png)
![Screen Shot 2021-09-20 at 17 44 04](https://user-images.githubusercontent.com/58029441/133990029-3f085c96-637d-4476-bfdd-a1e660ef1738.png)
![Screen Shot 2021-09-20 at 17 44 17](https://user-images.githubusercontent.com/58029441/133990060-dcdb507d-e8e5-43e2-a358-aff0f23f7b2f.png)
![Screen Shot 2021-09-20 at 17 44 27](https://user-images.githubusercontent.com/58029441/133990074-0d1b17b1-bb01-4469-b63e-9402bf62f4e4.png)
![Screen Shot 2021-09-20 at 17 45 14](https://user-images.githubusercontent.com/58029441/133990171-20fe9ca8-d04c-4641-872a-dbe4605332e6.png)
![Screen Shot 2021-09-20 at 17 49 12](https://user-images.githubusercontent.com/58029441/133990607-59310006-e724-4766-a522-b3d2314f7cec.png)
![Screen Shot 2021-09-20 at 17 50 55](https://user-images.githubusercontent.com/58029441/133990755-3ce0a7c3-1ec4-4730-9d19-1b5ca2e7c628.png)
![Screen Shot 2021-09-20 at 17 51 26](https://user-images.githubusercontent.com/58029441/133990807-a95ae770-c755-486b-b49a-fff2529da586.png)
# Install and reboot, Configure the VM (pfSense)

Enter ānā then press Enter key for VALN set up

![Screen Shot 2021-09-20 at 17 56 36](https://user-images.githubusercontent.com/58029441/133991387-097670e0-5636-4fbb-8a75-36e74ea4cef2.png)

Enter āvtnet1ā then press Enter key for WAN interface, Enter āvtnet0ā then press Enter key for LAN interface

![Screen Shot 2021-09-20 at 17 59 36](https://user-images.githubusercontent.com/58029441/133991737-cb9fafbc-8767-4431-953a-5ebb81a9066b.png)

Enter ā2ā (For Set interface(s) IP address)

![Screen Shot 2021-09-20 at 18 56 47](https://user-images.githubusercontent.com/58029441/133998214-5974b123-ff93-4c25-88a6-3b84b6d647af.png)

Enter ā2ā (For LAN) then press Enter key, since we want to configure LAN interface.

To configure the IP address for LAN (Here we should use an unused IP address from the LAN which PVE host resides in) (e.g. 192.168.1.50)

For subnet bit count, usually we enter ā24ā here

Press Enter then Press Enter again to leave IPv6 settings

Warning: Answer ānā for DHCP server setup for LAN interface (for now) (If not, you might cause trouble in your PVE LAN)

Enter ānā again for revert to HTTP

![Screen Shot 2021-09-20 at 18 57 45](https://user-images.githubusercontent.com/58029441/133998318-39cb2336-f590-4417-bef8-4161396414c8.png)

![Screen Shot 2021-09-20 at 18 59 17](https://user-images.githubusercontent.com/58029441/133998643-f08befc5-d83b-4352-b29d-52757fee7118.png)

Open https://192.168.1.50 (or the address you have configured) from our browser which should be in the same LAN as PVE

Login to pfSense web gui with Account: admin / pfsense

![Screen Shot 2021-09-20 at 19 03 44](https://user-images.githubusercontent.com/58029441/133999036-2c39d948-1f00-4681-81c6-ff33bda720ee.png)

Ignore the pfSense Setup page, Click on āInterfacesā -> Click on āWANā 

![Screen Shot 2021-09-20 at 19 18 38](https://user-images.githubusercontent.com/58029441/134001005-c64b0182-abdf-478c-967d-25d17393795e.png)


Scroll to the bottom of the page -> From āReserved Networksā

Uncheck āBlock private networks and loopback addressesā and āBlock bogon networksā, click on āSaveā button then click on āApply Changesā

![Screen Shot 2021-09-20 at 19 19 10](https://user-images.githubusercontent.com/58029441/134001065-fdc2a1a2-01df-4b20-b7dc-b33efa0f5a40.png)

Click on āSystemā -> āAdvancedā -> āNetworkingā

![Screen Shot 2021-09-20 at 19 20 28](https://user-images.githubusercontent.com/58029441/134001206-82c1ecb3-5a6a-4392-99f8-115ed75bf801.png)

![Screen Shot 2021-09-20 at 19 22 56](https://user-images.githubusercontent.com/58029441/134001536-264088f7-9484-49d6-ac23-daf224fb1e1e.png)

Important!: Scroll down to the bottom of the page (Network Interfaces)
 
Make sure following items are `checked` (Or the NAT/internet wonāt work properly for VMs)

āHardware Checksum Offloadingā

āHardware TCP Segmentation Offloadingā

āHardware Large Receive Offloadingā

Click on āSaveā button and cancel reboot

![Screen Shot 2021-09-20 at 19 23 55](https://user-images.githubusercontent.com/58029441/134001666-333ac8bf-e96f-43c2-ade8-b9146b397764.png)

Click on āFirewallā -> āRulesā -> āWANā

![Screen Shot 2021-09-20 at 19 25 27](https://user-images.githubusercontent.com/58029441/134001832-ab71722e-f060-49e4-9e69-2b9a93585116.png)

![Screen Shot 2021-09-20 at 19 25 53](https://user-images.githubusercontent.com/58029441/134001878-eab16f0f-a4fc-48b4-8095-2f4f9578b4fc.png)

Click on āAddā button to create a rule on WAN

We configure following values for this rule

Action: pass
Interface: WAN
Protocol: TCP
Source: Any (or restrict by IP/subnet)
Destination: WAN Address
Destination port range: HTTPS (Or the custom port)
Description: Allow remote management from anywhere (DANGER!)
Click on āSaveā button and then āApply Changesā button

![Screen Shot 2021-09-20 at 19 28 54](https://user-images.githubusercontent.com/58029441/134002328-3fa636ac-05db-4d11-a660-3ded03aa24c9.png)

![Screen Shot 2021-09-20 at 19 29 34](https://user-images.githubusercontent.com/58029441/134002410-3dd100e2-9fcc-4ad9-bc1a-f340cdafa86e.png)

Now we need to bring back the console or noVNC for pfSense from PVE

Enter ā1ā (Assign Interfaces) then press Enter key

![Screen Shot 2021-09-20 at 19 30 50](https://user-images.githubusercontent.com/58029441/134002607-3b28f5de-c744-497b-892f-1b3829edbdec.png)

Answer no for VALN setup -> Enter āvtnet0ā for WAN -> āvtnet1ā for LAN -> āyā to proceed

![Screen Shot 2021-09-20 at 19 32 33](https://user-images.githubusercontent.com/58029441/134002811-84b36ab5-ccd5-4caf-9cca-07dd3590b203.png)

Configure WAN if itās not configured as DHCP

Enter ā2ā (Set interface(s) IP address)

ā1ā to select WAN

When asked āConfigure IPv4 address WAN interface via DHCP?ā, we answer āyā then press Enter key

Answer ānā for IPv6 unless necessary

Answer ānā for āā¦revert to HTTPā¦ā


Repeat the above step again but this time we select LAN rather than WAN

ā IPv4 address should be ā10.1.1.1ā since we use ā10.1.1.0/24ā network for the NAT (Refer to the screenshot from Assumptions & Preparation) (Modify if necessary)

ā subnet bit count will be 24 (Modify if necessary)

ā Follow the prompt to press Enter key

ā Enable DHCP server on LAN by answering āyā when asked

ā Configure IP address range for the DHCP pool, here we use 10.1.1.100 as start address, 10.1.1.200 as the end address

ā Answer no for āā¦revert to HTTPā¦ā


Bring back PVE web gui, navigate to node name/cluster name -> pfSense VM -> Hardware
ā Double click on āNetwork Device (net1)ā
ā Uncheck āDisconnect
ā Click on āOKā button (In other words, connect the NIC)

![Screen Shot 2021-09-20 at 19 41 03](https://user-images.githubusercontent.com/58029441/134003856-7115d3db-cdb8-49b8-90db-c1df3c70e945.png)

![Screen Shot 2021-09-20 at 19 41 16](https://user-images.githubusercontent.com/58029441/134003879-311a18bf-a658-4f98-acdd-104065df4193.png)

Restart VM pfSense

Now we have a working NAT network for VMs on Proxmox VE

A simple topology graph here:

Internet <-> Router/Modem <-> pfSense VM (On PVE) <-> VM within the NAT network (On PVE)
