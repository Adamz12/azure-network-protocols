<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


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
<img src="https://i.postimg.cc/wx115WqY/create-resource-group.png" width="800" height="800" alt="Disk Sanitization Steps"/>
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
<img src="https://i.postimg.cc/9MKZPKsz/windows-vm.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>
</p>
<img src="https://i.postimg.cc/WbMgr98b/virtual-network.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>
<br />
<p>

- Create linux virtual machine
- Ensure it has same resource group as windows-vm with the one created
- Ensure it has the same virtual network as windows-vm
- Authentication type select password instead of SSH key.

</p>
<p>
<img src="https://i.postimg.cc/kGx5zpYj/linux-vm.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.postimg.cc/bN9JR4tr/check-virtual-netowrk.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.postimg.cc/nLgczJ6R/ensure-virtual-netowrks-same.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>

<h3>Part 2 - Observe ICMP Traffic</h3>
<p>

- install windows app
- Run windows vm by loggin in with credentials and the public IP address of the vm

</p>
<p>
<img src="https://i.postimg.cc/KjVhgKLd/download-windowsapp.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.postimg.cc/cJZyYGg4/running-windows-vm-on-windowsapp.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>

<p>

- Download wireshark
- Use this link - https://www.wireshark.org
- Select windows x64 version

</p>
<p>
<img src="https://i.postimg.cc/VvRQD7xP/download-wireshark.png" height="800" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>

- Open wireshark
- Start packet capture by clicking the shark icon

</p>
<p>
<img src="https://i.postimg.cc/Fs40SPFs/wireshark-packet-capture.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>

<p>

- Search for ICMP traffic
- It should be empty

</p>
<p>
<img src="https://i.postimg.cc/Kv8vCLJD/icmpt-traffic.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>

<p>

- Open PowerShell
- Retrieve the private ip address of the linux machine
- Ping that ip address in powershell and observe the ping requests

</p>
<p>
<img src="https://i.postimg.cc/d3x15VPY/powershell.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.postimg.cc/59764nHR/oberve-traffic.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>

<p>

- Open PowerShell
- Ping google.com
- Observe the traffic in wireshark

</p>
<p>
<img src="https://i.postimg.cc/FKr4JX8g/ping-google-com.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.postimg.cc/mDqfqzD8/observe-traffic-google.png" height="800" width="800" alt="Disk Sanitization Steps"/>
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
<img src="https://i.postimg.cc/fb0GTLsR/constant-ping.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>

<p>
<h4>Open the Network Security Group your Ubuntu VM is using and disable incoming (inbound) ICMP traffic
</h4>

- In the Azure portal, navigate to Virtual machines → [Your Ubuntu VM]

- Under Settings, click Networking

- In the Network Interface panel, click the linked Network security group

- In the NSG overview, select Inbound security rules

- Click Add to create a new rule:

1. Source: Any

2. Source port ranges: *

3. Destination: Any

4. Destination port ranges: *

5. Protocol: ICMP (or ICMPv4)

6. Action: Deny

7. Priority: 290

8. Name: Deny-ICMP-Inbound (or your preferred label)

- Click Add to save the rule and immediately block all incoming ICMP traffic to the Ubuntu VM.



</p>
<p>
<img src="https://i.postimg.cc/V6wmxMt8/deny-icmp-traffic.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>

<p>
<h4>Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity
</h4>

- Go to windows machine
- Observe the ping activity from winodws powershell/command prompt and wireshark
- It should be timing out



</p>
<p>
<img src="https://i.postimg.cc/7hMVBvk4/timing-out.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>

<p>
<h4> Re-enable ICMP traffic for the Network Security Group your Ubuntu VM and go
back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity (should start working)

</h4>

- Delete the rule to block the icmp traffic that we created
- Then go back to the windows machine and observe the traffic
- The requests should be back to working as normal



</p>
<p>
<img src="https://i.postimg.cc/vHGFGbDW/delete-rule.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.postimg.cc/HW4RZR3j/Observe-traffic-resume.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>

<p>
<h4> Stop ping activity

</h4>

- Click control and C together to stop the ping activity
- Stop wireshark activity



</p>
<p>
<img src="https://i.postimg.cc/bY7HK9YM/stop-wireshark-activity.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>

<p>
<h4> Observe SSH Traffic

</h4>

- Open Wireshark and select the network interface

- In the filter bar, type ssh and press Enter

- Open Windows PowerShell

- Run ssh username@[Ubuntu-Private-IP]

- Enter the password when prompted
  
- Type commands (username, pwd, etc) into the linux SSH connection and observe SSH traffic spam in WireShark


- Exit ssh connection by typing "exit" and entering it on power shell



</p>
<p>
<img src="https://i.postimg.cc/cHnqvts5/connecting-ssh.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>

<p>
<h4> Observe DHCP Traffic

</h4>

- Filter DHCP traffic in Wireshark (dhcp)
(To capture only DHCP packets and observe the negotiation steps.)

- Run ipconfig /renew in PowerShell
(Shows that a simple renew doesn’t display the full handshake in the wireshark.)


</p>
<p>
<img src="https://i.postimg.cc/mrvh3SRY/iprenew-own.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>

<p>
  
- In order to get the full dhcp handshake create a notepad with ipconfig /release
ipconfig /renew (Automates releasing and renewing the IP to force the full DHCP cycle.)
  
- Save as dhcp.bat in C:\ProgramData (All Files)
(Places the script in a system-accessible location for easy execution.)

- In PowerShell, cd C:\ProgramData and ls
(Verifies the script is in the correct directory before running.)

- Execute the script with .\dhcp.bat
(Triggers both release and renew, you’ll temporarily lose network connectivity as the IP is released; once the renew completes, the VM automatically re-obtains its IP. In Wireshark you’ll see the full Discover, Offer, Request, and Acknowledgment sequence.)

</p>
<p>
<img src="https://i.postimg.cc/528rN4yf/notepad.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.postimg.cc/tCxS2qhF/files-dhcp.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.postimg.cc/FsyqhF7N/cd-dhcp.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.postimg.cc/XJ5Lxx54/wireshark-full-dhcp.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>

<p>
<h4> Observe DNS Traffic

</h4>

- Filter DNS traffic in Wireshark (dns)
 (To capture only DNS query and response packets.)

- Click the Restart icon in Wireshark
 (Clears existing packets so you start with an empty capture.)

- Open Windows PowerShell
 (To issue DNS lookup commands.)

- Run nslookup google.com
 (Generates a DNS query for google.com and displays its IP address.)

- Run nslookup disney.com
 (Generates a DNS query for disney.com and displays its IP address.))

- Observe the DNS queries and responses in Wireshark
 (Confirms that the VM is correctly resolving domain names via DNS.)


</p>
<p>
<img src="https://i.postimg.cc/hvMfWzQh/dns-nslookup.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>

<p>
<h4> Observe RDP Traffic

</h4>

- Now open Wireshark and apply the display filter: tcp.port == 3389

- You’ll immediately see a flood of RDP packets—this “spam” is just your active Remote Desktop session continuously sending screen updates, input events, and keep-alive messages between your client and the VM.


</p>
<p>
<img src="https://i.postimg.cc/zBgccKMS/rdp.png" height="800" width="800" alt="Disk Sanitization Steps"/>
</p>

<p>
<h3> Conclusion

</h3>

This lab walked through securing and inspecting traffic between Azure VMs: you pinged and captured ICMP, used SSH from PowerShell to your Ubuntu VM, examined DHCP and DNS negotiations, and monitored RDP packets in Wireshark. You also applied a Network Security Group rule to block inbound ICMP. Be sure to shut down—or delete if you’re finished—all your VMs to avoid unnecessary charges. Thanks for following along, and I hope this exercise sharpened your Azure networking and troubleshooting skills!


</p>

<br />
