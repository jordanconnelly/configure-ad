<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


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

The first step will be creating a Virtual Machine (Windows Server) and Resource Group within Azure. When creating the Virtual Machine named "DC-1", choose to create a new Resource Group called AD-Lab. Choose a 2 vcpu size computer so you can use the other 2 vcpus for the other computer you will create in the next few steps. Allow the DC-1 server to create a new Virtual Network & Subnet.
   "DC-1" will serve as our Domanin Controller, which is essentially a Server with Active Directory installed.
<p>
 <img src="https://imgur.com/H3t2C4M.png">
 <p>
 </p>
<br />
 While the DC-1 is deploying you can go ahead and create the Client-1 VM (Windows 10) in Azure. Make sure to file Client-1 in the same Resource Group you just created as well as putting it in the same region.Proceed to the Networking tab to select the same Virtual Network as DC-1, in this case I named it "AD-Lab-vnet".
<p>
<img src="https://imgur.com/fDtS7fa.png">
<img src="https://imgur.com/xpIX3mv.png">
</p>
<p>
<br />
You can confirm the two VM's are using the same Virtual Network by opening both VM's and looking at the Virtual Network on the right of the page. The VM's will not be able to communicate if they are on different Virtual Networks.
<p>
 <img src="https://imgur.com/IwchXKw.png">
 <img src="https://imgur.com/mWvQCbU.png">
</p>
<br />
At this point we will set the DC-1 NIC Private IP Address to static. Using a Static IP Address on a Domain Controller is necessary 
<p>
<img src="https://imgur.com/cDYoinU.png"/>
 <img src="https://imgur.com/ZfZG8BP.png"/>
  <img src="https://imgur.com/q9SoFke.png"/>
</p>
<p>
  
Using remote desktop, log into the domain server. If you aren't familiar with remote desktop, watch this quick tutorial: https://www.youtube.com/watch?v=naUGaqqRA54. In the lower right search bar, type in "Firewall ". Select "Windows Defender Firewall With Advanced Security". Click on the "Protocols" section (Illustrated above). Right-click on ICMP Echo diagnostics and enable ICMPv4 TCP Protocols on Windows Firewall. Once connectivity is established between both machines, install Active Directory and add a domain. The machine restarts by default and log back in with the new domain create and the pre-existing username. Create Organizational Units (OU) and a couple of users. Give one of the users administrator properties by right-clicking on the user, selecting "properties", select "member" tab and add them to "Domain Admins" group. Click "check names" button after typing in "domain admins" to re-affirm the existence of the group name.
</p>
<br />
<img src="https://imgur.com/7ubwpEk.png"/>
<p>
<img src="https://imgur.com/c84A9HK.png"/>
</p>
<p>
Use the Microsoft Azure portal to change the DNS settings of the client machine to the server's private IP address (Illustrated above). Ensure connectivity and then log back into the server to verify that my "client" computer is listed in the "computers" container of the Active Directory domain root.
  <img src="https://imgur.com/TFkdUdP.png"/>
  <img src="https://imgur.com/R9z70ej.png"/>

</p>
Logged back into the client machine with the admin user login and allowed domain users access to remote desktop via system properties. Log out and log into the domain as the admin user. Using powershell as an administrator allows you to run a script that generates random users to the OU within Active Directory. You can also manually create more users.

<img src="https://imgur.com/DJck9Dm.png"/>
<br />
Lastly, pick a random user from the OU and log into the client machine.
