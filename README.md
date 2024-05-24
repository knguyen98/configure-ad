
---

<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Deploying On-Premises Active Directory in Azure</h1>
This tutorial guides you through setting up an on-premises Active Directory environment within Azure Virtual Machines.<br />

<h2>Environments and Technologies Utilized</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)


<h2>Deployment and Configuration Steps</h2>

<p>
</p>
<p>
In this lab, we'll create two VMs within the same VNET. One VM will serve as the Domain Controller, while the other will act as the Client machine. The Domain Controller's IP will be set to static since it's offering Active Directory services to the Client machine. The Client machine will then be joined to the domain, and its DNS settings will be configured to use the Domain Controller as its DNS server. 
</p>
<br />

<p>
<img src="https://i.imgur.com/d22FHIm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To ensure connectivity, we'll attempt to ping the Domain Controller (DC-1) from the Client machine (Client-1). Initially, the ping may fail. To resolve this, we'll enable ICMPv4 on the firewall of DC-1, allowing successful pinging from Client-1.
</p>
<br />

<p>
<img src="https://i.imgur.com/HvZBWzc.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/1lrrGPw.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
Next, we'll log back into DC-1 to install Active Directory Users & Computers. After promoting the VM to a Domain Controller, setting up a new forest as "mydomain.com", and restarting, we'll log back in as "mydomain.com\labuser" to verify successful execution of AD Users & Computers.
</p>
<img src="https://i.imgur.com/cGjvRke.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
We'll proceed to create Organizational Units (OUs), starting with "_EMPLOYEES" and "_ADMINS". Additionally, we'll create a new user named Jane Doe with the username "Jane_admin", assigning her to the domain admins security group.
</p>
<img src="https://i.imgur.com/hL7g5Y5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
<img src="https://i.imgur.com/kcgvzdE.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
Moving forward, we'll join Client-1 to the domain "mydomain.com" by configuring its DNS settings to use DC-1's Private IP address. Upon verification, Client-1 should be utilizing DC-1 as its DNS server.
</p>
<img src="https://i.imgur.com/jbrGTXW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
<img src="https://i.imgur.com/kvcm2cY.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To complete the domain joining process, we'll navigate to Client-1's system settings, select "About", then "Rename this PC (advanced)", and finally change the domain to "mydomain.com". Authentication with mydomain.com\labuser credentials will complete the process.
</p>
<br />
<p>
  <p>
<img src="https://i.imgur.com/Ze0Em5e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
With Client-1 now part of the domain, we'll set up remote desktop access for non-administrative users. After logging into Client-1 as an admin, we'll open System Properties, navigate to "Remote Desktop", and grant "domain users" access to remote desktop.
</p>
<br />

<p>
  <p>
<img src="https://i.imgur.com/SApOKiE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Finally, to verify that normal users can RDP into Client-1, we'll utilize a PowerShell script to create thousands of users in the domain. After user creation, we'll select one and RDP into Client-1 using their credentials.
</p>
<br />
<img src="https://i.imgur.com/EzWG8ug.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<p>
  <p>
<img src="https://i.imgur.com/Gkpe68K.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/n3gMwQV.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
As demonstrated, the PowerShell script successfully created a user named "bab.hubo", with whom we were able to log into Client-1 as a normal user.
</p>
