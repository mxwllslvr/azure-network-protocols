<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Video Demonstration</h2>

- ### [YouTube: Azure Virtual Machines, Wireshark, and Network Security Groups](https://www.youtube.com/watch?v=TaRA-Bq5ixM)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (RDP, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Windows Server 2022

<h2>High-Level Steps</h2>

- Step 1: Create and Configure Azure Virtual Machines
- Step 2: Install Wireshark on the Windows Virtual Machine
- Step 3: Observe Network Traffic with Wireshark
- Step 4: Configure and Test Network Security Groups (NSGs)

<h2>Actions and Observations</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create two virtual machines in Microsoft Azure: one running Windows 10 (21H2) and another running Windows Server 2022 (or use the VMs from <a href="https://github.com/mxwllslvr/Configuring-On-premises-Active-Directory-within-Azure-VMs/"> Configuring On-premises Active Directory within Azure VMs</a>.) Ensure both VMs are in the same virtual network to allow communication. For both VMs, enable Remote Desktop Protocol (RDP) by adding an inbound rule in their NSGs for port 3389. Assign public IP addresses to both VMs and note their private IP addresses for later use. Verify connectivity by using Remote Desktop to connect to the Windows 10 VM and the Windows Server 2022 VM from your local machine.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
On the Windows 10 VM, download and install Wireshark from the official website. Launch Wireshark and select the network interface associated with the VM’s private IP address. Start capturing packets without applying any filters to observe all network traffic. To generate traffic for analysis, perform actions such as pinging the Windows Server 2022 VM’s private IP from the Windows 10 VM (using `ping <Windows-Server-Private-IP>` in Command Prompt) and accessing a website via a browser to generate HTTP/HTTPS traffic. Observe the captured packets in Wireshark, noting protocols like ICMP (for ping) and TCP (for HTTP/HTTPS).
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In the Azure Portal, navigate to the NSG associated with the Windows Server 2022 VM. Create a new inbound security rule to block ICMP traffic (protocol: ICMP, action: Deny, priority: lower than the allow rules). From the Windows 10 VM, attempt to ping the Windows Server 2022 VM again and observe in Wireshark that ICMP packets are sent but no responses are received, indicating the NSG rule is blocking the traffic. Next, create an outbound rule in the Windows 10 VM’s NSG to block HTTP traffic (port 80). Attempt to access a website from the Windows 10 VM and verify in Wireshark that TCP SYN packets are sent but no connection is established. Revert these rules to restore connectivity and observe the traffic resuming in Wireshark.
</p>
<br />
