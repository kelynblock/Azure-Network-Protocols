# Azure-Network-Protocols<p align="center">
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

- In Azure, create the resources: One Resource Group; Two Virtual Machines & Network Watcher
- Use Remote Desktop & Download Wireshark; Filter traffic using icmp protocol
- Ping using Windows Powershell & create rule in Network Security Groups
- Utilize filters (SSH; DHCP; DNS; & RDP) & observe the traffic in WireShark

<h2>Actions and Observations</h2>

<p>
<img src="https://i.imgur.com/9LjUA8L.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In Azure, create a resource group that will be getting attached to the two virtual machines. Create two virtual machines, one with the Windows 10 Pro image, and the other with Ubuntu Server 20.04 LTS. Both machines must be connected to the same resource group initially created. In the Azure search bar, search for Network Watcher, scroll down to & click on Topology, select the resource group that was created at the beginning & select the virtual network attached to it. This picture is a diagram or confirmation of these steps.
</p>
<br />

<p>
<img src="https://i.imgur.com/PYgbPSh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In Azure, obtain (copy it) the public IP address of the Windows VM; open a Remote Desktop Session, paste the IP address onto it & logon with the VM credentials. Open the web browser & search for WireShark; download the Windows edition. Open WireShark, click on Ethernet Adapter & click on the blue fin (located on upper left, below File) to capture traffic. Enter icmp in the top bar; this will filter & only display traffic using the icmp protocol. (WireShark will turn red if you try using an incorrect/unrecognizable filter, & green when correct. As you type it will show suggestions which you can click on and press enter as a short-cut). Next, open CMD prompt in the WindowsVM & send a ping request, www.mlb.com, forcing IPv4 address only (ping www.mlb.com -4) & observe WireShark.
</p>
<br />

<p>
<img src="https://i.imgur.com/4v3rLzo.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In Azure, obtain the private IP address of the UbuntuVM, open Windows PowerShell in the WindowsVM remote desktop session and in PowerShell, ping the UbuntuVM with a -t (this option will send perpetual (nonstop) pings; can be cancelled by pressing ctrl & c).
<p>
<img src="https://i.imgur.com/2cy9lk5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
While the UbuntuVM is getting nonstop pings, go into Azure >UbuntuVM-NSG (Network Security Group) >Settings >Inbound Rules (displays the existing rules) >Add new rule (for this purpose just select ICMP as protocol) >Action >Deny >Set priority as 200 (not relevant here, but if it was, the priority must be set to lower # than any existing one so that it takes precedence over any existing rule that may otherwise essentially overrule the rule you're trying to put in effect).
</p>
<br />
<p>
<img src="https://i.imgur.com/NKHbRun.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once the changes we just made (blocked ICMP traffic) went into effect, WireShark & PowerShell both show that there's been a time out. The ping request was interrupted. We can revert the changes by simply going back to the NSG settings and allowing the traffic in the same rule created.

</p>
<br />
<p>
<img src="https://i.imgur.com/9BeDyEW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now we use SSH as a filter in WireShark & SSH into the UbuntuVM from the WindowsVM PowerShell. After successful login, I enter some CMDS & WireShark shows the traffic.

</p>
<br />
<p>
<img src="https://i.imgur.com/5fQkdUB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Here I use the dhcp filter in WireShark, and in PowerShell enter ipconfig /renew. This will request a new IP address from the dhcp server, & WireShark displays the traffic.

</p>
<br />
<p>
<img src="https://i.imgur.com/6Qi8QXD.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Using the dns filter in WireShark and the nslookup CMD in PowerShell, we see the traffic generated as we asked the dns server for Google's IP address.

</p>
<br />
<p>
<img src="https://i.imgur.com/uMWmOOC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lastly, we use RDP as a filter (tcp.port==3389) (Same filter; RDP uses Port 3389) in WireShark and notice that it's generating hundreds of packets since we are in an active RDP session.
