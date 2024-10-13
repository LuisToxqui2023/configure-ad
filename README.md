<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)


<h2>Deployment and Configuration Steps</h2>


<p>
In this lab, we will create two VMs in the same VNET. One will be a Domain Controller, the other will be a Client machine. We will change the DC to a static IP because it's offering Active Directory services to the client machine. The client machine will be joined to the domain. We will control the DNS settings on the client machine, the client machine will use the DC as its DNS server.
</p>
<br />

![av1](https://github.com/user-attachments/assets/6113bdcf-5fee-4591-96ee-6e9bb4920af5)

<p>
DC-1 has to have a static Private IP Address. Client one will connect to DC-1 to ensure connectivity we will try to ping DC-1 from Client-1. At first, the ping will not work correctly. We have to enable ICMPv4 on the firewall on DC-1. Now we can ping DC-1 successfully from Client-1
</p>
<br />

![av2](https://github.com/user-attachments/assets/a3075f14-db48-4f5f-b82c-b31ddb319cb8)

![av3](https://github.com/user-attachments/assets/94d5ae1f-d914-46c8-8f70-887fe8c10a3b)

<p>
Now we will log back into DC-1 to install AD Users & Computers. Promote the VM to DC, set up a new forest as "mydomain.com" afterward restart then log back into DC-1 as user: "mydomain.com\labuser". If you performed the steps properly you should be able to run AD Users & Computers as shown below.
</p>
<br /> 

![av4](https://github.com/user-attachments/assets/d1bd18ad-5809-4d0a-aca7-389a14d5d7d1)

<p>
Excellent! We can start creating Organizational Units (OU). Let's first create an OU named _EMPLOYEES. Create another OU named _ADMINS. To do that, right-click on the domain area. Select new->Organizational Unit and fill out the field. Then click inside of your OU and right-click, select new and select user, and fill out the information for your new user. The user should be named Jane Doe, she is going to be an Admin so her username will be Jane_admin. Lastly, add Jane to the domain admins security group.
</p>
<br />

![av5](https://github.com/user-attachments/assets/5a58a62f-a738-4803-934c-c74c2d810c7d)

![av6](https://github.com/user-attachments/assets/f4852681-021f-4684-b8d8-f9eb2b347418)

<p>
From now on you can use Jane_admin as the administrator account. Now we will join Client-1 to the domain (mydomain.com) from the azure portal we will change client-1's DNS settings to the DC's Private IP address. After you do that restart Client-1 from within the Azure portal. Our picture below shows verification that client-1 is on the DC-1 DNS.
</p>
<br />

![av7](https://github.com/user-attachments/assets/0a8eba21-7b24-4f04-8e9a-82bec52989c3)

![av8](https://github.com/user-attachments/assets/abbba814-e9a7-4ece-a215-fc4728012134)

<p>
We have to join Client-1 to the domain in order to do so navigate to your system settings and go to about. Off to the right select rename this pc (advanced). From there select to change the domain. Enter "mydomain.com" after that enter your credentials from mydomain.com\labuser. Your computer will restart and then client-1 will be a part of mydomain.com
</p>
<br /> 

![av9](https://github.com/user-attachments/assets/822ad85c-a523-4fb4-9852-8b67a4194e20)

<p>
Wonderful Client-1 is now a part of the domain. Now we will set up a remote desktop for non-administrative users on Client-1. We have to log into Client-1 as an admin and open system properties. Click on "Remote Desktop", and allow "domain users" access to the remote desktop. After completing those steps you should be able to log into Client-1 as a normal user.
</p>
<br /> 

![av10](https://github.com/user-attachments/assets/8e64b808-df1e-4e0a-8edd-84c360b1a40a)

<p>
Lastly, to verify that normal users can RDP into Client-1 we will use a script to generate thousands of users into the domain. We will input the script in PowerShell, after the users are created we will select one and RDP into Client-1.
</p>
<br />

![av11](https://github.com/user-attachments/assets/db842e21-b055-4836-91c5-742ef66cf431) 

![av12](https://github.com/user-attachments/assets/508d0b4b-029f-4edf-94dd-a25ab31996ff) 

![av13](https://github.com/user-attachments/assets/7d0f16a9-42e2-4d4f-9010-de1bb4bab69e) 

<p>
As you can see the PowerShell script created a user with the username "bab.hubo" We were able to login to Client-1 with his credentials as a normal user.  
</p>


