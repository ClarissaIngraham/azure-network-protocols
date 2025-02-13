<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />






<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (ICMP, SSH, DHCP, DNS, RDP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>Actions and Observations</h2>
Welcome first you will be going over where you will create the resource group and the 2 VM's(Virtual Machines). You will first create a resource group in Azure, and within that resource group you will create two VM's. One will be a Linux machine and the other will be a Windows 10 machine. These 2 VM's must be assigned to the same virtual network and have 2vcps (2 Virtual Central Processing Units).
</p>
<br />
<p>

![image](https://github.com/user-attachments/assets/ef7b58a6-4ded-4bfc-9841-d66e5ebad39c)
<p>

</p>
<p>
Next you will connect the Windows 10 VM to a remote desktop. Once that is done, you will download Wireshark on the Windows 10 VM. Here is the link to download Wireshark https://www.wireshark.org/download.html. Wireshark is a network protocol analyzer that will help you capture and inspect data packets traveling through a network. It allows you to monitor network traffic in real-time and also troubleshoot network issues. Once you have Wireshark installed you will begin to filter for ICMP(Internet Control Message Protocol) traffic. This is a network layer protocol used to seek out any network connection issues, ping is used for these types of issues. Then you will use powershell to ping, ping is a process that checks if a host is reachable and uses ICMP echo request and echo reply messages. You will ping the private IP address of your Linux VM so you can see the packets on Wireshark. (Ex: ping 10.0.0.5)
</p>
<br />

![image](https://github.com/user-attachments/assets/392411c3-d92f-4e55-880f-e76356666126)
<p>

<p>
</p>
<p>
You will now initiate a continuous ping from your Windows 10 VM to your Linux VM. This will continuously ping until you decide to stop it. To perform this action, in powershell you will enter ping and the Linux private IP address (ex: ping 10.0.0.5 -t). While this continuous ping is happening, you will go into the Linux VM and create a inbound security rule within the network security group. This rule will block any ICMP incoming traffic. To allow the ICMP traffic to continue again, you will go back into the Linux VM network security group and delete the inbound security rule you created.
</p>
<br />

<p>


![image](https://github.com/user-attachments/assets/411a8ebb-b038-45c4-8dc5-b077e99dfe04)

</p>
<br />

<p>

![image](https://github.com/user-attachments/assets/640f57f8-29b9-4c59-8dd6-0f89437d9ea0)


</p>
<p>
You will now observe SSH (secure shell) traffic in wireshark from the Windows VM. SSH allows us to securely access and manage network devices, usually remotely. So from the Windows 10 VM you're going to securely access the Linux VM by using the command ssh, the username of the Linux VM and the Linux private IP address, ex(ssh labuser@10.0.0.5). You will now see that Wireshark starts to capture the SSH packets.
</p>
<br />


![image](https://github.com/user-attachments/assets/1f1a5eae-d604-441f-acac-7e92316bf546)


Now you will filter for DHCP(Dynamic Host Configuration Protocol). It is used to automactically assign IP addresses and uses ports 67 and 68. You will request a new IP address, by saving the commands ipconfig /release and ipconfig /renew on the Windows VM and then running those commands in powershell. During this process the Windows VM will release the old IP address and automatically restart with a new IP address.


![image](https://github.com/user-attachments/assets/e15caf45-7675-4a75-a3b7-82f8820a20e1)


Now it is time to filter for DNS(Domain Name System) traffic. DNS translates domain names into IP addresses. We will first filter for DNS in Wireshark. In the Windows 10 VM we will open up powershell and type the command nslookup yahoo.com. This process allows the DNS server to translates the domain name, which is yahoo.com into an IP address.




![image](https://github.com/user-attachments/assets/9e2bb639-4c8d-4bb6-90c1-95f020a14490)


Last but not least, we will now filter for RDP(Remote Desktop Protocol). RDP is a protocol that allows you to connect to a computer or server remotely over a network. We will first type tcp.port == 3389 in wireshark, there you be able to non-stop spamming. The reason we are seeing this is because we are currently connected to a VM using RDP, meaning traffic is always being transmitted.


![image](https://github.com/user-attachments/assets/9733138c-f360-4513-87c6-96588c3f64e7)




