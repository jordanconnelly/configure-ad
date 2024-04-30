<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Creating a Resource Group with Server and Client
- Configuring TCP Proctols in Windows Firewall
- Installing Active Directory and User Groups
- Connecting Virtual Machine to Cloud Server and Adding Users

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://github.com/jordanconnelly/configure-ad/assets/165927916/974a3b3b-7cf2-44da-b977-f96c77a929fe"/>
</p>
<p>
Within Azure, create two virtual machines. Select "Window Data server 2022" for the first operating system. You will also want to select "2 VCPUs". Meaning, two machines can be used in this session. The second VM will be "Windows 10". The domain server will be named "DC-1" and the user will be named "Client". To ensure that the IP address of the server doesn't change throughout the process, select the "static" option in the network settings.
</p>
<br />

<p>
<img src="https://github.com/chrisrraP/configure-ad/raw/main/DC-1%20Firewall%20Setup.png"/>
 <img src="https://github.com/chrisrraP/configure-ad/raw/main/DC-1%20Add%20Forest.png"/>
  <img src="https://github.com/chrisrraP/configure-ad/raw/main/DC-1%20Add%20User%20Role.png"/>
</p>
<p>
  
Using remote desktop, log into the domain server. If you aren't familiar with remote desktop, watch this quick tutorial: https://www.youtube.com/watch?v=naUGaqqRA54. In the lower right search bar, type in "Firewall ". Select "Windows Defender Firewall With Advanced Security". Click on the "Protocols" section (Illustrated above). Right-click on ICMP Echo diagnostics and enable ICMPv4 TCP Protocols on Windows Firewall. Once connectivity is established between both machines, install Active Directory and add a domain. The machine restarts by default and log back in with the new domain create and the pre-existing username. Create Organizational Units (OU) and a couple of users. Give one of the users administrator properties by right-clicking on the user, selecting "properties", select "member" tab and add them to "Domain Admins" group. Click "check names" button after typing in "domain admins" to re-affirm the existence of the group name.
</p>
<br />
<img src="https://github.com/chrisrraP/configure-ad/raw/main/Change%20Client%20one%20DNS%20Server.png"/>
<p>
<img src="https://github.com/chrisrraP/configure-ad/raw/main/Confirm%20Client%20Server%20Changed.png"/>
</p>
<p>
Use the Microsoft Azure portal to change the DNS settings of the client machine to the server's private IP address (Illustrated above). Ensure connectivity and then log back into the server to verify that my "client" computer is listed in the "computers" container of the Active Directory domain root.
  <img src="https://github.com/chrisrraP/configure-ad/raw/main/Allow%20users%20server%20access.png"/>
  <img src="https://github.com/chrisrraP/configure-ad/raw/main/Generate%20Random%20Users.png"/>

</p>
Logged back into the client machine with the admin user login and allowed domain users access to remote desktop via system properties. Log out and log into the domain as the admin user. Using powershell as an administrator allows you to run a script that generates random users to the OU within Active Directory. You can also manually create more users.

<img src="https://github.com/chrisrraP/configure-ad/raw/main/Login%20as%20new%20user%20to%20server.png"/>
<br />
Lastly, pick a random user from the OU and log into the client machine.
