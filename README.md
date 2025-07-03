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
- Authentication type select password instead of SSH key.

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

<h3>Part 2 - Observe ICMP Traffic</h3>
<p>

- install windows app
- Run windows vm by loggin in with credentials and the public IP address of the vm

</p>
<p>
<img src="https://i.postimg.cc/KjVhgKLd/download-windowsapp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.postimg.cc/cJZyYGg4/running-windows-vm-on-windowsapp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>

- Download wireshark
- Use this link - https://www.wireshark.org
- Select windows x64 version

</p>
<p>
<img src="https://i.postimg.cc/VvRQD7xP/download-wireshark.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>

- Open wireshark
- Start packet capture by clicking the shark icon

</p>
<p>
<img src="https://i.postimg.cc/Fs40SPFs/wireshark-packet-capture.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>

- Search for ICMP traffic
- It should be empty

</p>
<p>
<img src="https://i.postimg.cc/Kv8vCLJD/icmpt-traffic.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>

- Open PowerShell
- Retrieve the private ip address of the linux machine
- Ping that ip address in powershell and observe the ping requests

</p>
<p>
<img src="https://i.postimg.cc/d3x15VPY/powershell.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.postimg.cc/59764nHR/oberve-traffic.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>

- Open PowerShell
- Ping google.com
- Observe the traffic in wireshark

</p>
<p>
<img src="https://i.postimg.cc/FKr4JX8g/ping-google-com.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.postimg.cc/mDqfqzD8/observe-traffic-google.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>


<h3>Part 3 - Configuring a Firewall [Network Security Group]</h3>
<p>
<h4>Initiate a perpetual/non-stop ping from your Windows 10 VM to your Ubuntu VM</h4>

- On your Ubuntu VM, find its private IP:

1. Open a terminal and run ip a (look for the IP under your primary network interface, e.g., eth0), or

2. In the Azure portal, go to Virtual machines → [Your Ubuntu VM] → Networking and note the Private IP address.

3. Switch to your Windows 10 VM and open Command Prompt (Win + R → cmd → Enter).


- ping (Ubuntu-Private-IP) -t


- To monitor this traffic, open Wireshark on the Windows 10 VM.

1. Select the network interface used to reach the Ubuntu VM.

2. In the display filter bar, type icmp and press Enter.

3. You’ll see real-time ICMP echo requests and replies between your Windows and Ubuntu VMs.



</p>
<p>
<img src="https://i.postimg.cc/fb0GTLsR/constant-ping.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />
