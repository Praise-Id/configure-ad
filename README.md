D<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>
<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines, including the deployment of a Domain Controller and a domain-joined client machine.<br />
<h2>Environments and Technologies Used</h2>

Microsoft Azure (Virtual Machines/Compute, Virtual Networks)
Remote Desktop
Active Directory Domain Services
PowerShell
DNS


<h2>Operating Systems Used</h2>

Windows Server 2022
Windows 10


<h2>High-Level Deployment and Configuration Steps</h2>

Step 1: Create a Resource Group
Step 2: Create a Virtual Network and Subnet
Step 3: Create the Domain Controller VM (DC-1)
Step 4: Set DC-1's Private IP Address to Static
Step 5: Log Into DC-1 and Disable the Windows Firewall
Step 6: Create the Client VM (Client-1)
Step 7: Set Client-1's DNS Settings to DC-1's Private IP Address
Step 8: Restart Client-1
Step 9: Log Into Client-1
Step 10: Ping DC-1 from Client-1
Step 11: Verify DNS Settings on Client-1


<h2>Deployment and Configuration Steps</h2>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Create Resource Group"/>
</p>
<p>
<strong>Step 1: Create a Resource Group.</strong> Open a web browser and go to portal.azure.com, then log in with your Azure account credentials. In the search bar at the top of the page, type "Resource groups" and click it when it appears. Click "Create" near the top-left of the page. On the Basics tab, select your subscription, name the resource group (e.g., "DomainLab-RG"), and choose the region closest to you. Click "Review + create," then click "Create" once validation passes.
</p>
<br />
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Create Virtual Network and Subnet"/>
</p>
<p>
<strong>Step 2: Create a Virtual Network and Subnet.</strong> In the Azure search bar, type "Virtual networks" and click it when it appears. Click "Create" near the top-left of the page. On the Basics tab, select your subscription, choose the resource group created above, name the virtual network (e.g., "DomainLab-VNet"), and select the same region used previously. Click "Next: IP Addresses," leave the default address space, and either leave the default subnet as-is or click "Add a subnet" to customize it. Click "Review + create," then click "Create" once validation passes.
</p>
<br />
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Create Domain Controller VM"/>
</p>
<p>
<strong>Step 3: Create the Domain Controller VM (DC-1).</strong> In the Azure search bar, type "Virtual machines" and click it when it appears. Click "Create," then select "Azure virtual machine." On the Basics tab, select your subscription and resource group, name the VM "DC-1," choose the same region as your virtual network, and select the "Windows Server 2022" image. Under Administrator account, set the username to <code>labuser</code> and the password to <code>Cyberlab123!</code>. Click "Next: Networking," and confirm the Virtual network and Subnet dropdowns show the network created in Step 2. Click "Review + create," then click "Create" once validation passes.
</p>
<br />
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Set Static Private IP"/>
</p>
<p>
<strong>Step 4: Set DC-1's Private IP Address to Static.</strong> On DC-1's overview page in Azure, click "Networking" in the left-hand menu, then click the Network Interface link. Click "IP configurations" in the left-hand menu, then click on the listed IP configuration. Change the "Assignment" setting from "Dynamic" to "Static," confirm the Private IP address field, and click "Save."
</p>
<br />
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disable Windows Firewall on DC-1"/>
</p>
<p>
<strong>Step 5: Log Into DC-1 and Disable the Windows Firewall.</strong> On DC-1's overview page, click "Connect," then select "RDP," and download and open the RDP file. Log in using username <code>labuser</code> and password <code>Cyberlab123!</code>. Once logged in, open "Windows Defender Firewall" from the Start menu, click "Turn Windows Defender Firewall on or off" in the left-hand menu, and select "Turn off Windows Defender Firewall" for both Private and Public network settings. Click "OK" to save. (This is done for testing connectivity purposes.)
</p>
<br />
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Create Client VM"/>
</p>
<p>
<strong>Step 6: Create the Client VM (Client-1).</strong> In the Azure search bar, type "Virtual machines" and click it when it appears. Click "Create," then select "Azure virtual machine." On the Basics tab, select the same subscription and resource group used for DC-1, name the VM "Client-1," choose the same region, and select the "Windows 10" image. Under Administrator account, set the username to <code>labuser</code> and the password to <code>Cyberlab123!</code>. Click "Next: Networking," and confirm the Virtual network and Subnet dropdowns match the ones used for DC-1. Click "Review + create," then click "Create" once validation passes.
</p>
<br />
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Set Client-1 DNS Settings"/>
</p>
<p>
<strong>Step 7: Set Client-1's DNS Settings to DC-1's Private IP Address.</strong> First, note DC-1's private IP address from its Azure overview page. Then go to Client-1's overview page, click "Networking" in the left-hand menu, and click the Network Interface link. Click "DNS servers" in the left-hand menu, select "Custom DNS servers," and enter DC-1's private IP address. Click "Save."
</p>
<br />
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Restart Client-1"/>
</p>
<p>
<strong>Step 8: Restart Client-1.</strong> On Client-1's overview page in Azure, click the "Restart" button near the top of the page and confirm if prompted. Wait a few minutes for the VM to fully restart.
</p>
<br />
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Log Into Client-1"/>
</p>
<p>
<strong>Step 9: Log Into Client-1.</strong> On Client-1's overview page, click "Connect," then select "RDP," and download and open the RDP file. Log in using username <code>labuser</code> and password <code>Cyberlab123!</code>.
</p>
<br />
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Ping DC-1 from Client-1"/>
</p>
<p>
<strong>Step 10: Ping DC-1 from Client-1.</strong> Within the Client-1 Remote Desktop session, open Command Prompt from the Start menu. Type <code>ping &lt;DC-1-Private-IP-Address&gt;</code> and press Enter, replacing the placeholder with DC-1's actual private IP address. Confirm that replies are returned, indicating the ping succeeded.
</p>
<br />
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Verify DNS Settings"/>
</p>
<p>
<strong>Step 11: Verify DNS Settings on Client-1.</strong> In the same Command Prompt window, type <code>ipconfig /all</code> and press Enter. Scroll through the output and confirm that the "DNS Servers" entry matches DC-1's private IP address.
</p>
<br />

