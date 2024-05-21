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
</p>
<br />
<h2>Setting Up Resources in Azure</h2>

The first step will be creating a Virtual Machine (Windows Server) and Resource Group within Azure. When creating the Virtual Machine named "DC-1", choose to create a new Resource Group called AD-Lab. Allow the DC-1 server to create a new Virtual Network & Subnet.
   "DC-1" will serve as our Domanin Controller, which is essentially a Server with Active Directory installed.
<p>
 <img src="https://imgur.com/H3t2C4M.png">
 <p>
 </p>
<br />
 While the DC-1 is deploying you can go ahead and create the Client-1 VM (Windows 10) in Azure. Make sure to file Client-1 in the same Resource Group and region you just created. Proceed to the Networking tab to select the same Virtual Network as DC-1, in this case I named it "AD-Lab-vnet".
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
At this point we will set the DC-1 NIC Private IP Address to static. Open DC-1 and select Networking > Network Settings, then click on the Network interface (NIC) to open the configuration page. Using a Static IP Address on a Domain Controller is necessary to allow Clients to reliably locate and connect to it. The Domain Controller will serve as the DNS server for the Clients.
<img src="https://imgur.com/S2TAp0P.png">
<p>
<br />
You will notice the Private IP Address is currently Dynamic. Click on "ipconfig1" to open the settings to set the IP Address to static, then confirm by observing the IP configurations again.
<img src="https://imgur.com/zfwhB2b.png">
 <img src="https://imgur.com/DREzxSp.png">
  <img src="https://imgur.com/g2kJRgC.png">
</p>
<br />
<h2>Ensure Connectivity between Client & Domain Controller</h2>
</p>
Login to the Domain Controller with Remote Desktop and enable ICMPv4 on the local Windows Firewall. Type 'firewall" into the Start menu and select "Windows Defender Firewall with Advanced Security". Click on "Inbound Rules" from the menu on the left and sort by Protocol. Enable the two ICMP Echo Requests. This allows inbound ICMP traffic that we will use in the next step, without enabling this the Client-1 will not be able to send Ping requests to the DC.
<img src="https://imgur.com/wIybYB4.png">
<img src="https://imgur.com/VZ6qBsi.png">
</p>
<br />
Login to Client-1 using Remote Desktop, open the Command Prompt (Start>cmd>Enter) and ping DC-1's Private IP Address using "ping -t (ip address)". If done correctly, you will get perpetual Ping replies from DC-1.
<img src="https://imgur.com/HBttApi.png">
</p>
<br />
<h2>Installing Active Directory</h2>
</p>
Open DC-1 in Remote Desktop, open the Server Manager from the Start menu if it isn't already open. Click on "Add roles and features, Next, select "Role-based installation", select DC-1 as the Destination Server, check the box for "Active Directory Domain Services" and Add Features. Select Next until you can click Install.
<p>
<img src="https://imgur.com/tNMHcbP.png">
<br />
</p>
To finish installing Active Directory click on the Notifications tab at the top of the page and select "Promote this server to a domain controller".

<img src="https://imgur.com/9z3yD8k.png">
<br />
</p>
In the pop-up window select "Add a new forest" and type in the Root domain name. Type in a password for the DSRM on the next page. Use all the default settings and click next until you can click Install. Once it has finished installing the computer will need to restart, kicking you out of Remote Desktop.
<p>
<img src="https://imgur.com/esjZWJy.png">
<img src="https://imgur.com/lGJEqVW.png">
<br />
</p>
Log back into DC-1 with the FQDN (Fully Qualified Domain Name) as user: mydomain.com\labuser. This is because the DC-1 is now a Domain Controller, so you are logging in as a user of that instead of a normal computer account user.
<p>
<img src="https://imgur.com/5XzikYB.png">
<br />
</p>
<h2>Creating an Administrator & Normal User Account in Active Directory</h2>
</p>
Now we will create Organizational Units in Active Directory and a couple users. First, type active directory into the Start menu of DC-1 and select "Active Directory Users and Computers" to open the UI for AD.
<p>
<img src="https://imgur.com/aBQwkO5.png">
<br />
</p>
Right click on the "mydomain.com" tab on the left and select New>Organizational Unit to create a new folder for Admins and Employees.
<p>
<img src="https://imgur.com/tg8DifW.png">
<br />
</p>
Open the Admins folder, right click in the blank area to add a new Admin user and password, click Finish.
<p>
<img src="https://imgur.com/3q5wHOb.png
<br />
</p>
To make the new user a Domain Admin, right click the user account and open Properties. Within Properties select the "Member Of" tab and click "Add...". Type "domain" in the object names box and click "Check Names". Select "Domain Admins" from the list & Apply.
<p>
<img src="https://imgur.com/vpDp4PD.png">
<br />
</p>
Log out of DC-1 and log back in with DC-1's Public IP Address as the admin user you just created "mydomain.com\jordan_admin". Minimize DC-1 for now.
</p>
<br />
<h2>Join Client-1 to Your Domain (mydomain.com)</h2>
</p>
From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address by going to Client-1 Network Settings, open the NIC, and select DNS Servers on the left menu. Change the DNS Servers from "Inherit from VM" to "Custom", this is where you input the the DC's Private IP Address. Click Save. Restart Client-1 from the Azure Portal, this will flush the DNS cache.
<p>
<img src="https://imgur.com/sGyKSpy.png">
<br />
</p>
Login to Client-1 as the original admin user (labuser) and join it to the domain. In Client-1 open Settings to the About page, select "Rename this PC". Click "Change...", check the Domain box and type in "mydomain.com". This will prompt you to enter the login info for the Admin account created earlier (mydomain.com\jordan_admin). This will restart the computer.
<p>
<img src="https://imgur.com/UaYssaW.png">
<br />
</p>
Log back into Client-1 as the Admin user created (mydomain.com\jordan_admin), open System Properties by right clicking the Start Menu and selecting "System". Click "Remote Desktop" on the right, then "Select users that can remotely access this PC". Click "Add..." and type in "domain users", select "Check Names" and "OK". Now all Domain Users will be allowed to login to this computer.
