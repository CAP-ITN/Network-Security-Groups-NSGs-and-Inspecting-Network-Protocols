# NSGs-and-Inspecting-Network-Protocols
<p align="center">

  ![Azure](https://github.com/user-attachments/assets/6f70d6b9-e903-427b-b200-efcf8e5bc098)

</p>

<h1>Network Security Groups - Prerequisites</h1>
This section outlines the processes in installing and checking on Network Activity <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computer)
- Remote Desktop

<h2>Operating Systems Used </h2>

- Windows 10</b> (22H2)
- Ubuntu 22.04 LTS</b> (Linux)

<h2>List of Prerequisites</h2>

- WireShark
- PowerShell


  
<h2>Step 1: Observing ICMP Traffic</h2>

<p>

![NSG1](https://github.com/user-attachments/assets/814fe247-3bc8-423e-b42c-7e927f8e62da)


</p>
<p>
-Connect to Windows 10 VM: Use Remote Desktop to access your Windows 10 Virtual Machine.
-Install Wireshark: Download and install Wireshark within your Windows 10 VM.
-Start Packet Capture: Open Wireshark, select your active network interface, and start packet capture.
-Filter for ICMP Traffic: In the Wireshark filter bar, enter icmp and press Enter to display only ICMP traffic.
-Ping Ubuntu VM: Retrieve the private IP of your Ubuntu VM (Linux-VM) and initiate a ping from Windows 10 VM using: ping <10.0.0.5>
  
</p>
<br />

<p>

![NSG4](https://github.com/user-attachments/assets/dbccb7d8-841a-4d26-b8c7-627703764ec5)

<h2>Step 2: Configuring a Firewall (Network Security Group) to Block ICMP Traffic</h2>

![NSG5-5](https://github.com/user-attachments/assets/ccd60c94-7686-46b4-9c20-bd4a9f0b6075)

</p>



<p>
Initiate a Continuous Ping: Start a continuous ping from the Windows 10 VM in Powershell to the Ubuntu VM to observe traffic in real-time:
ping <10.0.0.5> -t
Modify the Network Security Group (NSG) through Azure:
Locate the inbound rules and disable ICMP (ping) traffic by blocking ICMP packets.
Observe Changes in Wireshark & Ping Output:
In Wireshark, notice that ICMP requests are sent, but there are no replies.
In the Command Prompt/PowerShell, you should see Request Timed Out messages.
Re-enable ICMP Traffic:
Go back to the Ubuntu VM's NSG and allow inbound ICMP traffic again.
Verify ICMP is Working Again:
In Wireshark, you should now see ICMP replies appearing again.
In Command Prompt/PowerShell, the ping responses should resume.
Stop Ping Activity: Use Ctrl + C to stop the ongoing ping test.
</p>
<br />

<p>

<h2>Step 3: Observing the Traffic of Protocols</h2>

![NSG7](https://github.com/user-attachments/assets/e160d5ed-b661-42d7-8624-588926338106)



<p>
To observe SSH traffic in Wireshark, start a packet capture on your Windows VM and filter for SSH traffic (tcp.port == 22). Open PowerShell and initiate an SSH connection to your Ubuntu VM using its private IP: ssh labuser@ <privateIPaddress>, then enter your credentials. As you type commands, observe the SSH packets appearing in Wireshark. To end the session, type exit and press Enter, stopping the SSH traffic.
</p>
<br />

<p>

![NSG9-5](https://github.com/user-attachments/assets/9ea85e41-c476-446f-b7cb-2205727982e4)


</p>
<p>
To observe DHCP traffic in Wireshark, start a packet capture on your Windows VM and filter for DHCP traffic (udp.port == 67 || udp.port == 68). Open PowerShell as admin and run a script: ipconfig /release followed by ipconfig /renew to request a new IP address. Watch the DHCP packets appear in Wireshark as the VM communicates with the DHCP server.
</p>
<br />

<p>

![NSG10](https://github.com/user-attachments/assets/bf930805-75a3-4e5d-97b1-51fe58cbf391)

</p>
<p>
To observe DNS traffic in Wireshark, start a packet capture on your Windows VM and filter for DNS traffic (tcp.port == 53). Open PowerShell and run nslookup Nike.com and nslookup Amazon.com to query their IP addresses. Watch the DNS request and response packets appear in Wireshark.
</p>
<br />

![NSG11](https://github.com/user-attachments/assets/780fa656-d5d7-4f4e-b2c9-bdf1ddee85c4)

</p>
<p>
To observe RDP traffic in Wireshark, start a packet capture on your Windows VM and filter for RDP traffic (tcp.port == 3389). Notice the continuous stream of packets even when idle. This occurs because RDP constantly transmits data to maintain a live stream between computers, ensuring a smooth remote session.
</p>
<br />
