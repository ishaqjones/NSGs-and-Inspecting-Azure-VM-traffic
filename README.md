<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, I observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, DHCP, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used</h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create a Resource Group in Microsoft Azure
- Create a Windows Virtual Machine (VM)
- Create a Linux (Ubuntu) VM 
- Observe Vnet within Network Watcher

<h2>Actions and Observations</h2>

<p>
Using Windows Remote Desktop to connect to Windows 10 VM, we download and install Wireshark and filter for ICMP traffic. 
</p>

<p>
  
![filter icmp traff](https://github.com/ishaqjones/NSGs-and-Inspecting-Azure-VM-traffic/assets/156931487/a01068fe-23b1-4aa8-8148-4189ae2f13d3)

</p>

<br />

<p>
We retrieve the private IP address of the Ubuntu VM (10.0.0.5) and attempt to ping it from the Windows 10 VM.
</p>

<p>
  
![private IP add Ubuntu](https://github.com/ishaqjones/NSGs-and-Inspecting-Azure-VM-traffic/assets/156931487/77bba62b-11c1-4b6a-a000-58151d45348f)

</p>

<p>
  
 ![ping ubuntu](https://github.com/ishaqjones/NSGs-and-Inspecting-Azure-VM-traffic/assets/156931487/47ecaa03-0e63-4e11-9c3d-10bb97c48fcb)
 
</p>


<br />

<p>
We observe the ping requests and replies from both VMs (windows 10 and Ubuntu) reflected in Wireshark.
</p>

<p>
  
![observe icmp in wireshark](https://github.com/ishaqjones/NSGs-and-Inspecting-Azure-VM-traffic/assets/156931487/faf6a98b-a2a9-4e32-af14-3496ccdb75ff)

</p>

<br />

<p>
  From the Windows 10 VM, we open the command line and attempt to ping a public website wwww.google.com to observe the traffic in Wireshark. We notice that Google has the IP address 172.253.122.147 as the destination with 10.0.0.4 (our host machine's private IP address)  as the source. All packets from Google are successful. 
</p>

<p>
  
  ![ping google result](https://github.com/ishaqjones/NSGs-and-Inspecting-Azure-VM-traffic/assets/156931487/8c383f2e-0c43-4ef4-ba0b-159cce564fbf)

</p>

<br />

<p>
  Next, we'll want to initiate, disable, and observe a perptual ping from the Windows 10 VM to the Ubuntu (Linux) VM We'll achieve this by first initiating the perptual ping.
</p>

<p>
  
  ![perpetual ping](https://github.com/ishaqjones/NSGs-and-Inspecting-Azure-VM-traffic/assets/156931487/5875a7b1-0c11-479d-b081-614e2a41327b)

</p>
<br />
<p>
  Returning to Microsoft Azure we'll open the Network Security Group Ubuntu VM is utilizing and disable incoming (inbound) traffic.
</p>
<p>
  
  ![create rule](https://github.com/ishaqjones/NSGs-and-Inspecting-Azure-VM-traffic/assets/156931487/6cf24cc3-3451-4a28-b6d3-c75f449b1c5a)

  ![imbound rule](https://github.com/ishaqjones/NSGs-and-Inspecting-Azure-VM-traffic/assets/156931487/1059aefb-25c8-451e-bd51-a2c46b9a9fcd)

  ![deny icmp](https://github.com/ishaqjones/NSGs-and-Inspecting-Azure-VM-traffic/assets/156931487/b1d6ea47-2415-4ed6-be35-c9ca5da1dd7f)

</p>

<br />

<p>
  We observe that after  about a minute, the requests have timed out. 
</p>

<p>

  ![request timed out](https://github.com/ishaqjones/NSGs-and-Inspecting-Azure-VM-traffic/assets/156931487/4f207288-5395-4a73-b546-ec3145e091e2)

</p>
<br />

<p> 
To reenable ICMP traffic we navigate back to the NSG (Network Security Group) in Ubuntu (Linux) VM and 'allow' traffic to resume under the assigned moniker. 
</p>

<p>
  
  ![select rule](https://github.com/ishaqjones/NSGs-and-Inspecting-Azure-VM-traffic/assets/156931487/36e82d60-1aaa-4fa3-84bc-d0cee0653e84)

  ![update rule](https://github.com/ishaqjones/NSGs-and-Inspecting-Azure-VM-traffic/assets/156931487/10490574-df5b-4714-9f20-b1919f57c02a)


</p>
<br />

<p>
  After updating the NSG rule to 'allow' ICMP traffic, the pings resumed. 
</p>
<p>
  
  ![IMCP disable enable](https://github.com/ishaqjones/NSGs-and-Inspecting-Azure-VM-traffic/assets/156931487/1d33b18b-6309-4f67-8d98-cd54b01c5827)

</p>
<br />
<p>
 To halt the perpetual ping we simply input ctrl + C. 
</p>
<p>
  
  ![stop request](https://github.com/ishaqjones/NSGs-and-Inspecting-Azure-VM-traffic/assets/156931487/e3d068b9-fda7-4de9-9d94-09a3de6de9c0)

</p>
<br />
<p>
  Next, we'll observe SSH Traffic. This can be achieved by typing 'SSH' or tcp.port == 22 in the top filter bar. 
</p>
<p>
  
  ![filter ssh](https://github.com/ishaqjones/NSGs-and-Inspecting-Azure-VM-traffic/assets/156931487/07b10982-043f-4bdd-a94d-7d634a58b8e7)

</p>
<br />
<p>
  Next, we'll open the command line and 'SSH into' our Ubuntu (Linux) VM and use the password we assigned when creating the Ubuntu VM. The password will be invisible but the input is valid. Note the SSH traffic after signing in. 
</p>
<p>
  
  ![ssh act 1](https://github.com/ishaqjones/NSGs-and-Inspecting-Azure-VM-traffic/assets/156931487/46abc0a8-d889-4e15-84ce-c3846bbcc9cb)

  ![ssh sign in act 2](https://github.com/ishaqjones/NSGs-and-Inspecting-Azure-VM-traffic/assets/156931487/5cb67eb1-b168-4dc5-a454-e9f72cc9f759)


</p>
<br />
<p>
  Continuing, we'll next observe DHCP Traffic.  This is achieved through a similar methodology of filtering via DHCP. This time however we'll type 'ipconfig /renew' to renew the IP address assigned to the computer from a DHCP server. This helps troubleshoot connectivity issues or issue  a new IP address lease. Note the DHCP traffic. The (Request) from 10.0.0.4 (our computer) has been acknowledged (ACK) by the DHCP server.
</p>
<p>
  
 ![filter DHCP](https://github.com/ishaqjones/NSGs-and-Inspecting-Azure-VM-traffic/assets/156931487/f25a85a0-80d6-495b-96b8-e174cdad1e06)

 ![DHCP traffic](https://github.com/ishaqjones/NSGs-and-Inspecting-Azure-VM-traffic/assets/156931487/39f3e794-ba45-45be-a7eb-7f7b216f7663)

 
</p>
<br />
<p>
  Moving on to DNS traffic we'll observe it through websites like Google and Disney using 'nslookup'. nslookup is a command line tool used to query Domain Name System (DNS) servers to obtain various types of records such as IP addresses associated with domain names for instance. We'll begin with Google first using DNS' port number 53 in the top filter bar (tcp.port==53) , open the command line and type 'nslookup google.com' and note the traffic in Wireshark. 
  
</p>
<p>
  
  ![DNS traffic 1](https://github.com/ishaqjones/NSGs-and-Inspecting-Azure-VM-traffic/assets/156931487/dafdf535-75b1-4c94-b699-a737be607383)

</p>
<br />
<p>
  Finally, we use nslookup for Disney to view its name resolution and traffic in Wireshark. It's interesting to note Disney's 'Server Unknown' status which can happen for various issues ranging from misconfiguration to Firewall/Security Restrictions that block  nslookup from accessing the authoritative DNS server.  In any event, we've received DNS traffic from Disney's server as it's confirmed in the packets received via Wireshark. We can explore the packets further but we'll save that for another tutorial ;) 
</p>
<p>
  
  ![DNS traffic 2 disney](https://github.com/ishaqjones/NSGs-and-Inspecting-Azure-VM-traffic/assets/156931487/0a3e6fd3-bdb3-4503-8668-d6e1238d1891)

</p>





