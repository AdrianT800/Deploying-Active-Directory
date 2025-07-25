![1035255](https://github.com/user-attachments/assets/7cdae74a-7836-4365-af4e-737124531edf)

# Deploying Active Directory
This guide outlines the process of setting up Active Directory on the Domain Controller, creating a domain admin user for administrative tasks, and joining Client-1 to the domain. It also covers configuring Remote Desktop access for a non-admin user on Client-1. As part of this setup, we will enable 10,000 user accounts, allowing them to log in to Client-1 through Remote Desktop Protocol (RDP).<br />


<h2>Video Demonstration</h2>

- ### [Google Drive: Deploying Active Directory](https://drive.google.com/file/d/1mXesRWpn45NtI5WG7xIARDfTxjViDRPj/view?usp=drive_link)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Resources Group, Virtual Machines/Computer, Virtual Machines/Window Server, Virtual Network and Subnet)
- Remote Desktop
- Active Directory Users and Computers (ADUC)
- Server manager

<h2>Operating Systems Used </h2>

- Windows 10 (Pro, version 22H2 - x64 Gen2)
- Windows Server 2022 Datacenter: (Azure Edition - x64 Gen2)

<h2>Deploying Active Directory Steps</h2>

- Step 1: Installing Acfive Directory and Promoting the Domain Controller
- Step 2: Creating a Domain Admin User
- Step 3: Joining Client-1 to the Domain
  
<h2>Lifecycle Stages</h2>

Step 1: Installing Acfive Directory and Promoting the Domain Controller
<p>
<img src="https://i.imgur.com/FnM43QX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To begin, log into the DC-1 virtual machine. Open the Start menu and search for Server Manager. Once it opens, click on Add Roles and Features, and proceed through by clicking Next until you reach the Server Roles section. Select Active Directory Domain Services, and when prompted, click Add Features.
</p>
<br />

<p>
<img src="https://i.imgur.com/4OPw7It.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Continue through until you reach confirmation, check the box to allow the server to restart automatically, then click Install and finally Close once the installation is complete.
</p>
<br />

<p>
<img src="https://i.imgur.com/w21zEev.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After installing the role, you’ll need to promote DC-1 to a domain controller. In Server Manager, click the flag icon in the top-right corner and select Promote this server to a domain controller. Choose "Add a new forest", and enter a domain name such as "mydomain.com". 
</p>
<br />

<p>
<img src="https://i.imgur.com/IkcT9V7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Click Next, then set the Directory Services Restore Mode (DSRM) password to "Password1". Uncheck Create DNS Delegafion. 
</p>
<br />

<p>
<img src="https://i.imgur.com/9b0v4rX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Click Next, uncheck Create DNS Delegafion. 
</p>
<br />

<p>
<img src="https://i.imgur.com/ktGBUgM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Continue through the wizard until you reach the Prerequisites Check. Click Install to complete the promofion. The VM will automafically restart. Once it reboots, log back in using the domain credenfials: Username: mydomain.com\labdemo, Password: Vmdemo12345$
</p>
<br />


Step 2: Creating a Domain Admin User
<p>
<img src="https://i.imgur.com/412tLF7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once logged back in DC-1, open Acfive Directory Users and Computers (ADUC) from the Windows Administrafive Tools. In the domain tree, right-click your domain (e.g., mydomain.com) and create two new Organizafional Units (OUs) named _EMPLOYEES and _ADMINS. Refresh the domain view to confirm their creafion.
</p>
<br />

<p>
<img src="https://i.imgur.com/QMRus98.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Within the _ADMINS OU, create a new user named Jane Doe. Right-click inside the OU, select New > User, and enter the following: Full Name: Jane Doe, Username: jane_admin, Password: Vmdemo12345$. Check the box for Password never expires, then click Next and Finish. Log out of DC-1 and log back in using the new domain admin account: Username: mydomain.com\jane_admin, Password: Vmdemo12345$. From this point forward, use this account for all administrative tasks.
</p>
<br />

<p>
<img src="https://i.imgur.com/xrNBw3h.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To add jane_admin to the "Domain Admins" Security Group, start by right-clicking Jane's account and selecting "Properties." Navigate to the "Member Of" tab, then click "Add." In the field that appears, type "Domain Admins" and click "Check Names" to verify the entry. Once confirmed, click "OK" to add the group, and then click "Apply" to save the changes.. Log out of DC-1 and log back in using the new domain admin account: Username: mydomain.com\jane_admin, Password: Vmdemo12345$. From this point forward, use this account for all administrative tasks.
</p>
<br />


Step 3: Joining Client-1 to the Domain
<p>
<img src="https://i.imgur.com/oS0vs7b.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log into Client-1 using the local admin credentials (Username: labdemo, Password: Vmdemo12345$). Right click Start menu, Open System, then click Rename this PC (Advanced) under Related Settings. In the dialog box, click Change, select Domain, and enter "mydomain.com". When prompted, provide the domain credentials for Jane Doe. After successful authentication, the system will prompt for a restart. Once restarted, log in using the domain credentials.
</p>
<br />

<p>
<img src="https://i.imgur.com/hQqfcNB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To verify the domain join, return to DC-1, open ADUC, and expand the domain tree. Click on Computers and confirm that Client-1 appears in the list. Next, create a new OU named _CLIENTS by right-clicking the domain, selecting New > Organizational Unit, and naming it accordingly. Drag Client-1 into the _CLIENTS OU and refresh the view.
</p>
<br />
