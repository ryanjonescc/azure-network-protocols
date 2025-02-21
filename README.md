<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
This tutorial explores network traffic to and from Azure Virtual Machines using Wireshark while also experimenting with Network Security Groups.



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>Actions and Observations</h2>

<p>
</p>
<p>
Welcome to this tutorial on Network Security Groups and Network Protocol Inspection. To begin, create two virtual machines (VMs) on Azure—one running Linux and the other running Windows 10. Both VMs should have two CPUs and be on the same virtual network (VNET).  

Next, download and install Wireshark on the Windows machine using the following link: [Wireshark Download](https://www.wireshark.org/download.html). Once installed, open Wireshark and set a filter for ICMP traffic. ICMP is a network layer protocol used to relay messages about network connection issues, and it is the protocol used by the ping command to test connectivity between hosts. By filtering Wireshark to capture only ICMP packets and pinging the private IP address of the Linux machine, you can visually observe the packet exchange in Wireshark.</p>
<br />
<p>
<img src="https://i.imgur.com/IIUShxp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We can inspect each individual packet and see the actual data that is being sent in each ping. the picture below demonstrates just that. 
</p>
<br />
<p>
<img src="https://i.imgur.com/GLxSIG3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In the next part of the lab, we will continuously ping the Linux machine using the `ping -t` command. This will send repeated ping requests until manually stopped. While the Windows machine is actively pinging the Linux machine, we will configure the Linux firewall to block inbound ICMP traffic. Once this rule is applied, the Windows machine will stop receiving echo replies from the Linux machine.  

To block ICMP traffic, we will create a new Network Security Group (NSG) for the Linux machine and configure it to deny ICMP requests. If we want to allow ICMP traffic again, we can modify the settings in the Network Security Group section on the Azure portal.</p>
<br />
<img src="https://i.imgur.com/5vXO75R.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/Asl80tN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
Next, we will use the Windows machine to establish an SSH connection to the Linux machine. SSH does not provide a graphical interface; instead, it grants access to the machine’s command-line interface (CLI).  

Before connecting, we will set the Wireshark filter to capture only SSH packets. Then, using the command prompt, we will initiate the SSH connection with the command `ssh labuser@10.0.0.5`. As soon as the connection is made, Wireshark will begin capturing SSH packet activity in real time.</p>
<br />
<img src="https://i.imgur.com/zteR41r.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now we will use wireshark to filter for DHCP. DHCP is the Dynamic Host Configuration Protocol this works on ports 67/68. It is used to assign IP addresses to machines. We will request a new ip address with the command "ipconfig /renew". Once we enter the command wireshark will capture DHCP traffic.
</p>
<br />
<img src="https://i.imgur.com/vU8fpQf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Time to filter DNS traffic. We will set wireshark to filter DNS traffic. We will initiate DNS traffic by typing in the command "nslookup www.google.com" this command essentially asks our DNS server what is google's IP address.
</p>
<br />
<img src="https://i.imgur.com/VMcwmsO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lastly we will filter for RDP traffic. When we enter tcp.port==3389 traffic is spammed non stop because we are using Remote Desktop Protocol to connect to our Virtual Machine. 
</p>
<br />
<img src="https://i.imgur.com/VxXGv6X.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<p>

<br />
