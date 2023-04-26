<p align="center">

<img src="https://i.imgur.com/VPfQd7Z.png" alt="Traffic Inspection"/>
          </p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
Here we will observe different network traffic to and from two virtual machines.We will use Wireshark to analyse traffic between two virtual machines created in Azure.We will also be reconfiguring the virtual machines Network Security Groups and observing the changes. <br />




<h2> Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Power Shell to deploy Various Command-Line actions
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used in Virtual Machines </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>Steps and Actions</h2>

<p>
</p>
<p>
To start you need to create two virtual machine on Microsoft Azure. Name one virtual machine VM1 and the other VM2. For VM1 one select Windows 10 as the operating system. . Important to remember your username/password. Also make sure both virtual machines are in the same Vnetwork.  I recommend selecting both virtual machines with 2vcpus.</p>

<img src="https://i.imgur.com/9iuAbJF.png"/>


<p>Once you are done creating both virtual machines. Log into VM1 one using remote desktop, if you are on a mac you will have to download an app called Microsoft RDP. Once you logged in to VM1 using your useranme and password open internet explorer. Once in internet explorer look for Wireshark and download it in the virtual machine. Wireshark is used to inspect network traffic and also to troubleshoot any problems in the network. </p> 



<img src="https://i.imgur.com/ReIE5nc.png" />
<p>Once installed open Wireshark and filter for ICMP Traffic only.The Internet Control Message Protocol (ICMP) is a protocol that devices within a network use to communicate problems with data transmission.We will ping VM2 private address using Powershell.Then we then use Wireshark to filter and only capture ICMP packets. We will them be able to see the ICMP packets on Wireshark. With Wireshark you can inspect each individual packet.
</p>
<br />
<p>
<img src="https://i.imgur.com/Gs5XrNA.png"/>        

</p>
<p>
 
</p>
<br />
<p>

</p>
<p>
Next, we will perpetually ping VM2 (Linux) using command ping -t. This will continually ping the machine until we decide to stop it. 
</p>
<br />
<img src="https://i.imgur.com/NrvtWD9.png" alt="ping -t"/>

<p> while the VM1(Windows) is perpetually pinging VM2 (Linux)  we will go to back in the Azure portal to access VM2 NSG (Network Security Group). Once in the NSG of VM2 create a new rule to block inbound ICMP. Write value of 200 in priority and name rule DENY_ICMP_PING_FROM_ANYWHERE.  Once we do that we will stop recieving echo replys from the Linux machine. We will block ICMP by creating a new Network Security Group on the Linux machine that will be set to block ICMP. We can allow the traffic by allowing ICMP on the Linux Network Security Groups page on Azure. </p>

</p>
<img src="https://i.imgur.com/8FXlWxY.png"  alt="Disk Sanitization Steps"/>

<p>
<img src="https://i.imgur.com/xcqewfe.png" alt="Deny Rule"/>
          </p>
<p>
Next we will use our Windows machine to SSH to the Linux machine. SSH has no GUI it just gives the user access to the machines CLI. We will set the wireshark filter to capture SSH packets only. When we ssh into the Linux machine with the command prompt "ssh labuser@10.0.0.5" we can see that wireshark starts to immediately capture SSH packets.
</p>
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
<p># Network-Security-Groups-Inspecting-Protocols

