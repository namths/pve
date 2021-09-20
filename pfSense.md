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

Enter “n” then press Enter key for VALN set up

![Screen Shot 2021-09-20 at 17 56 36](https://user-images.githubusercontent.com/58029441/133991387-097670e0-5636-4fbb-8a75-36e74ea4cef2.png)

Enter “vtnet1” then press Enter key for WAN interface, Enter “vtnet0” then press Enter key for LAN interface

![Screen Shot 2021-09-20 at 17 59 36](https://user-images.githubusercontent.com/58029441/133991737-cb9fafbc-8767-4431-953a-5ebb81a9066b.png)

Enter “2” (For Set interface(s) IP address)

![Screen Shot 2021-09-20 at 18 56 47](https://user-images.githubusercontent.com/58029441/133998214-5974b123-ff93-4c25-88a6-3b84b6d647af.png)

Enter “2” (For LAN) then press Enter key, since we want to configure LAN interface.

To configure the IP address for LAN (Here we should use an unused IP address from the LAN which PVE host resides in) (e.g. 192.168.1.50)

For subnet bit count, usually we enter “24” here

Press Enter then Press Enter again to leave IPv6 settings

Warning: Answer “n” for DHCP server setup for LAN interface (for now) (If not, you might cause trouble in your PVE LAN)

Enter “n” again for revert to HTTP

![Screen Shot 2021-09-20 at 18 57 45](https://user-images.githubusercontent.com/58029441/133998318-39cb2336-f590-4417-bef8-4161396414c8.png)

![Screen Shot 2021-09-20 at 18 59 17](https://user-images.githubusercontent.com/58029441/133998643-f08befc5-d83b-4352-b29d-52757fee7118.png)

Open https://192.168.1.50 (or the address you have configured) from our browser which should be in the same LAN as PVE

Login to pfSense web gui with Account: admin / pfsense

![Screen Shot 2021-09-20 at 19 03 44](https://user-images.githubusercontent.com/58029441/133999036-2c39d948-1f00-4681-81c6-ff33bda720ee.png)

Ignore the pfSense Setup page, Click on “Interfaces” -> Click on “WAN” 

![Screen Shot 2021-09-20 at 19 18 38](https://user-images.githubusercontent.com/58029441/134001005-c64b0182-abdf-478c-967d-25d17393795e.png)


Scroll to the bottom of the page -> From “Reserved Networks”

Uncheck “Block private networks and loopback addresses” and “Block bogon networks”, click on “Save” button then click on “Apply Changes”

![Screen Shot 2021-09-20 at 19 19 10](https://user-images.githubusercontent.com/58029441/134001065-fdc2a1a2-01df-4b20-b7dc-b33efa0f5a40.png)

Click on “System” -> “Advanced” -> “Networking”

![Screen Shot 2021-09-20 at 19 20 28](https://user-images.githubusercontent.com/58029441/134001206-82c1ecb3-5a6a-4392-99f8-115ed75bf801.png)

![Screen Shot 2021-09-20 at 19 22 56](https://user-images.githubusercontent.com/58029441/134001536-264088f7-9484-49d6-ac23-daf224fb1e1e.png)

Important!: Scroll down to the bottom of the page (Network Interfaces)
 
Make sure following items are `checked` (Or the NAT/internet won’t work properly for VMs)

“Hardware Checksum Offloading”

“Hardware TCP Segmentation Offloading”

“Hardware Large Receive Offloading”

Click on “Save” button and cancel reboot

![Screen Shot 2021-09-20 at 19 23 55](https://user-images.githubusercontent.com/58029441/134001666-333ac8bf-e96f-43c2-ade8-b9146b397764.png)

Click on “Firewall” -> “Rules” -> “WAN”

![Screen Shot 2021-09-20 at 19 25 27](https://user-images.githubusercontent.com/58029441/134001832-ab71722e-f060-49e4-9e69-2b9a93585116.png)

![Screen Shot 2021-09-20 at 19 25 53](https://user-images.githubusercontent.com/58029441/134001878-eab16f0f-a4fc-48b4-8095-2f4f9578b4fc.png)

Click on “Add” button to create a rule on WAN

We configure following values for this rule

Action: pass
Interface: WAN
Protocol: TCP
Source: Any (or restrict by IP/subnet)
Destination: WAN Address
Destination port range: HTTPS (Or the custom port)
Description: Allow remote management from anywhere (DANGER!)
Click on “Save” button and then “Apply Changes” button

![Screen Shot 2021-09-20 at 19 28 54](https://user-images.githubusercontent.com/58029441/134002328-3fa636ac-05db-4d11-a660-3ded03aa24c9.png)

![Screen Shot 2021-09-20 at 19 29 34](https://user-images.githubusercontent.com/58029441/134002410-3dd100e2-9fcc-4ad9-bc1a-f340cdafa86e.png)

Now we need to bring back the console or noVNC for pfSense from PVE

Enter “1” (Assign Interfaces) then press Enter key

![Screen Shot 2021-09-20 at 19 30 50](https://user-images.githubusercontent.com/58029441/134002607-3b28f5de-c744-497b-892f-1b3829edbdec.png)

Answer no for VALN setup -> Enter “vtnet0” for WAN -> “vtnet1” for LAN -> “y” to proceed

![Screen Shot 2021-09-20 at 19 32 33](https://user-images.githubusercontent.com/58029441/134002811-84b36ab5-ccd5-4caf-9cca-07dd3590b203.png)

Configure WAN if it’s not configured as DHCP

Enter “2” (Set interface(s) IP address)

“1” to select WAN

When asked “Configure IPv4 address WAN interface via DHCP?”, we answer “y” then press Enter key

Answer “n” for IPv6 unless necessary

Answer “n” for “…revert to HTTP…”


Repeat the above step again but this time we select LAN rather than WAN

– IPv4 address should be “10.1.1.1” since we use “10.1.1.0/24” network for the NAT (Refer to the screenshot from Assumptions & Preparation) (Modify if necessary)

– subnet bit count will be 24 (Modify if necessary)

– Follow the prompt to press Enter key

– Enable DHCP server on LAN by answering “y” when asked

– Configure IP address range for the DHCP pool, here we use 10.0.1.10 as start address, 10.0.1.200 as the end address

– Answer no for “…revert to HTTP…”


Bring back PVE web gui, navigate to node name/cluster name -> pfSense VM -> Hardware
– Double click on “Network Device (net1)”
– Uncheck “Disconnect
– Click on “OK” button (In other words, connect the NIC)

![Screen Shot 2021-09-20 at 19 41 03](https://user-images.githubusercontent.com/58029441/134003856-7115d3db-cdb8-49b8-90db-c1df3c70e945.png)

![Screen Shot 2021-09-20 at 19 41 16](https://user-images.githubusercontent.com/58029441/134003879-311a18bf-a658-4f98-acdd-104065df4193.png)


Now we have a working NAT network for VMs on Proxmox VE

A simple topology graph here:

Internet <-> Router/Modem <-> pfSense VM (On PVE) <-> VM within the NAT network (On PVE)
