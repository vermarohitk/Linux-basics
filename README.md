Setting Up a Lab

This is a basic tutorial to setup a baseline environment to bringup fedrora linux in virtual box.

We will perform a 3-step procedure

1. Setting up a virtual box on windows PC/laptop (we are using windows 10 enterprise with version 20H2)
2. Download and install a virtual machine template
3. Setting Up a Network


Step-1
1. Setup a virtual box
  Install (or upgrade) VirtualBox (https://www.virtualbox.org/wiki/Downloads)
  I am using 6.1.34 for this installation with out extension packages.
  Add the following directory into your path: C:\Program Files\Oracle\VirtualBox so that you can run VirtualBox cli commands.


Step-2
2. Download and Install the Virtual Machine Template
  Download Fedora 35 (server, VirtualBox, vdi file) from osboxes.org https://www.osboxes.org/fedora/
  user: root pass: osboxes.org (given in the info tab)
  Unzip (I use 7z) the downloaded file - this is a virtual disk image (VDI) file.
  VirtualBox: Create a Linux (RedHat 64G) machine (as a template)
  Use your downloaded VDI file as the system disk.
  
Step-3
3. Configure NAT Networking as explained "https://www.virtualbox.org/manual/ch06.html#network_nat_service"
•	Open cmd line in windows, nevigate to "C:\Program Files\Oracle\VirtualBox" & run following cmd at cmd prompt
VBoxManage natnetwork add --netname linux-nat --network "192.168.122.0/24" --enable --dhcp off
•	Run the cmd to check if network is created:
    C:\Program Files\Oracle\VirtualBox>VBoxManage natnetwork list
    NAT Networks:

    Name:         linux-nat
    Network:      192.168.122.0/24
    Gateway:      192.168.122.1
    DHCP Server:  No
    IPv6:         No
    IPv6 Prefix:  fd17:625c:f037:2::/64
    IPv6 Default: No
    Enabled:      Yes

    1 network found

    C:\Program Files\Oracle\VirtualBox>

•	for the VM, goto settings in vritual box by right-click -> settings, choose Network, and select "NAT Network" from the drop-down list. Make sure that the correct NAT network appears in "Name," linux-nat in our case.
•	Open the "Advanced" option there, and make sure that the "Cable Connected" is checked.
•	Next set the static IP as DHCP is off for this network.
•	Run the virtula machine in the VBox
•	Edit your networking parameters. Create the file name from the interface name:
    o	sudo vi /etc/sysconfig/network-scripts/ifcfg-<if name>
    	TYPE=Ethernet
    	BOOTPROTO=static
    	IPADDR=192.168.122.x (where x is 11,12,13 for k8s-a, k8s-b, k8s-c, 10 for k8s-control, 100 for the host)
    	PREFIX=24
    	ONBOOT=yes
    	GATEWAY=192.168.122.1
    	DNS1=8.8.8.8
o	restart VM 
•	If all goes well, you should be able to ping 8.8.8.8 or any external webserver
  
  
  ![image](https://user-images.githubusercontent.com/94822541/167825942-d74d8f64-505e-408b-9bbe-ddbd40445cbb.png)
