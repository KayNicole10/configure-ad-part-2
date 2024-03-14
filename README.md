# configure-ad-part-2
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in Azure (Part Two)</h1>
This is part two of a tutorial that outlines the implementation of on-premises Active Directory within Azure Virtual Machines. In this segment, I'll continue by installing Active Directory Domain Services on the Windows Server 2022 Virtual Machine, turning it into a domain controller, and setting up an admin account named "Jane Doe". <br />
<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 

<h2>Deployment and Configuration Steps</h2>

- Step 1: Change Windows Server's IP address from dynamic to static
- Step 2: Check Connectivity Between the Virtual Machines 
- Step 3: Install Active Directory Domain Services on Windows Server 2022 
- Step 4: Turn Windows Server 2022 into Domain Controller
- Step 5: Create "Jane Doe" admin account 

<h2>Step 1: Change Windows Server's IP address from dynamic to static</h2>

<p>
<img src="https://i.imgur.com/QfWj1tr.png" height="80%" width="80%" alt="static"/>
</p>
<p>
First, I will navigate to DC-1'S IP configuration settings and switch its private IP address from dynamic to static. This will ensure that the IP address for DC-1 will stay the same. </p>
<br />

<h2>Step 2: Check Connectivity Between the Virtual Machines</h2>

<p>
<img src="https://i.imgur.com/XRw5yip.png" height="80%" width="80%" alt="Connectivity"/>
</p>
<p>
To check the connectivity of the two virtual machines, I will login to Client-1 and try pinging DC-1's private IP address. "Request timed out" is showing up because ICMPv4 is disabled for DC-1. </p>

<p>
<img src="https://i.imgur.com/VzVOuEU.png" height="80%" width="80%" alt="ICMPv4"/>
</p>

<p>
<img src="https://i.imgur.com/6cd4rsz.png" height="80%" width="80%" alt="Core Networking Diagnostics"/>
</p>


<p>
To fix this, I will login to DC-1 and open Windows Defender Firewall. Select "Inbound Rules", and right-click to enable "Core Networking Diagnostics - ICMP Echo Request". This enables ICMP traffic on DC-1. </p>

<p>
<img src="https://i.imgur.com/Kpk616F.png" height="80%" width="80%" alt="ping"/>
</p>

<p>
Now the ping is successful, and we can now see that Client-1 and DC-1 are on the same network, and are able to connect to each other. </p>

<p>
<br />

<h2>Step 3: Install Active Directory Domain Services on Windows Server 2022</h2>

<p>
Now we are ready to install Active Directory on DC-1. (Keep in mind, DC-1 is what I name the Windows Server 2022). In the server manager on DC-1, Select "Add roles and features", and select DC-1 to be the destination server. </p>

<p>
<img src="https://i.imgur.com/QZe50mp.png" height="80%" width="80%" alt="install"/>
</p>

<p>
<img src="https://i.imgur.com/KmJYIup.png" height="80%" width="80%" alt="destination"/>
</p>

<p>
Select "Active Directory Domain Services", and proceed with installation. </p>

<p>
<img src="https://i.imgur.com/fg5ftzF.png" height="80%" width="80%" alt="Active Directory Domain Services"/>
</p>

<p>
<img src="https://i.imgur.com/B2Md4Cd.png" height="80%" width="80%" alt="installation."/>
</p>

<p>
<img src="https://i.imgur.com/CEXHARR.png" height="80%" width="80%" alt="Active Directory"/>
</p>

<br />

<h2>Step 4: Turn Windows Server 2022 into Domain Controller</h2>

<p>
Now that Active Directory Domain Services is installed, I will now turn the server into a domain controller. In order to do this, select "Promote this server into a domain controller" at the top right of the server manager. </p>

<p>
<img src="https://i.imgur.com/ZsUg1VO.png" height="80%" width="80%" alt="Domain Controller"/>
</p>

<p>
Now we add a new forest and root domain name. For this example, I will choose "mylab.com". </p>

<p>
<img src="https://i.imgur.com/F8KyRVL.png" height="80%" width="80%" alt="domain name"/>
</p>

<p>
The server will restart automatically once installation begins.  </p>

<p>
<img src="https://i.imgur.com/y4puNml.png" height="80%" width="80%" alt="server"/>
</p>

<p>
<img src="https://i.imgur.com/XKk4EsI.png" height="80%" width="80%" alt="begin"/>
  
</p>

<p>
<img src="https://i.imgur.com/lnBm5fw.png" height="80%" width="80%" alt="successful"/>
</p>

<p>
Now I will log back in using the new username "mylab.com\labuser". </p>

<p>
<img src="https://i.imgur.com/bOygW7p.png" height="80%" width="80%" alt="new username"/>
</p>

<br />

<h2>Step 5: Create "Jane Doe" admin account </h2>

<p>
<img src="https://i.imgur.com/5DHbkc9.png" height="80%" width="80%" alt="Users and Computers"/>
</p>
<p>
In the last step, I will open Active Directory Users and Computers and create two new organizational units. These will be listed under the domain name. One will be labeled "_EMPLOYEES", and the other "_ADMINS". </p>

<p>
<img src="https://i.imgur.com/dBsWw0m.png" height="80%" width="80%" alt="_EMPLOYEES"/>
</p>

<p>
<img src="https://i.imgur.com/QoREXOY.png" height="80%" width="80%" alt="_ADMINS"/>
</p>

<p>
<img src="https://i.imgur.com/bwXoRfx.png" height="80%" width="80%" alt="organizational units"/>
</p>

<p>
Next, I will begin creating a new admin account in the "_ADMINS" folder. Simply right-click, select "new", then "user". In this example, I will use the name "Jane Doe", and the username "jane_admin". </p>

<p>
<img src="https://i.imgur.com/WRV0i3o.png" height="80%" width="80%" alt="new"/>
</p>

<p>
<img src="https://i.imgur.com/Xq7jytQ.png" height="80%" width="80%" alt="admin"/>
</p>

<p>
<img src="https://i.imgur.com/p3ZRh3P.png" height="80%" width="80%" alt="account"/>
</p>

<p>
Even though Jane Doe is added to the "_ADMINS" folder, she still needs to be promoted as an admin. I will do this by making her account a member of the "Domain Admins" group. Simply right-click the name, select "properties", and "member of" .Then, select "Add".  </p>

<p>
<img src="https://i.imgur.com/UMVsGjD.png" height="80%" width="80%" alt="member"/>
</p>

<p>
Type "Domain Admins", then select "Ok".  </p>

<p>
<img src="https://i.imgur.com/zLmmTch.png" height="80%" width="80%" alt="Domain Admins"/>
</p>


<p>
If we type "whoami" in the command line, we can see that our username is still under "labuser". I will log back in as jane_admin, a new admin account. </p>

<p>
<img src="https://i.imgur.com/YUWqmj9.png" height="80%" width="80%" alt="whoami"/>
</p>

<p>
<img src="https://i.imgur.com/PySrMDe.png" height="80%" width="80%" alt="log"/>
</p>

<p>
<img src="https://i.imgur.com/3lqiWdt.png" height="80%" width="80%" alt="new admin account"/>
</p>

<br />
