<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Video Demonstration</h2>

- ### [YouTube: Azure Virtual Machines, Wireshark, and Network Security Groups](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create Initial Resources
- Observe ICMP Traffic
- Configure a Firewall (Network Security Group)
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic
- Lab Cleanup

<h2>Actions and Observations</h2>

<h3>Part 1 - Create Virtual Machines</h3>
<p>
  
- Create Resource Group
  
</p>
<p>
<img src="https://i.postimg.cc/wx115WqY/create-resource-group.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

<p>
 <p>
   
Create virtual machines:
- Put both in the resource group created(netowrk-protocols)
- Create windows virtual machine
- Rename the virtual network
- ensure that the username and password is on a notepad or file
  
</p>
<img src="https://i.postimg.cc/9MKZPKsz/windows-vm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
</p>
<img src="https://i.postimg.cc/WbMgr98b/virtual-network.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
<p>

- Create linux virtual machine
- Ensure it has same resource group as windows-vm with the one created
- Ensure it has the same virtual network as windows-vm

</p>
<p>
<img src="https://i.postimg.cc/kGx5zpYj/linux-vm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.postimg.cc/bN9JR4tr/check-virtual-netowrk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.postimg.cc/nLgczJ6R/ensure-virtual-netowrks-same.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />
