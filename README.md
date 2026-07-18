<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the deployment and configuration of a Domain Controller and domain-joined client machine using Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute, Virtual Networks)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used</h2>

- Windows Server 2022
- Windows 10

<h2>Prerequisite Requirements</h2>

- An active Microsoft Azure account with sufficient permissions/credits to create resources
- Basic familiarity with Remote Desktop Protocol (RDP)

<h2>Installation Steps</h2>

<h4>
First, create a Resource Group. This will keep all the resources for this lab organized in one place.
</h4>

- Open a web browser and go to portal.azure.com, then log in with your Azure account credentials.
- In the search bar at the top of the page, type "Resource groups" and click it when it appears.
- Click "Create" near the top-left of the page.
- On the Basics tab, select your subscription, name the resource group (e.g., "DomainLab-RG"), and choose the region closest to you.
- Click "Review + create," then click "Create" once validation passes.
<p>
<img width="938" height="626" alt="image" src="https://github.com/user-attachments/assets/122f9e5f-0a57-4fd6-bd5f-b2386a07485a" />
</p>

<br />

<h4>Next, create a Virtual Network and Subnet. DC-1 and Client-1 will both live inside this network so they can communicate with each other.</h4>

- In the Azure search bar, type "Virtual networks" and click it when it appears.
- Click "Create" near the top-left of the page.
- On the Basics tab, select your subscription, choose the resource group created above, name the virtual network (e.g., "DomainLab-VNet"), and select the same region used previously.
- Click "Next: IP Addresses," and leave the default address space and subnet, or customize as needed.
- Click "Review + create," then click "Create" once validation passes.
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Create Virtual Network and Subnet"/>
</p>

<br />

<h4>Next, create the Domain Controller VM (Windows Server 2022) named "DC-1"</h4>

- In the Azure search bar, type "Virtual machines" and click it when it appears.
- Click "Create," then select "Azure virtual machine."
- On the Basics tab, select your subscription and resource group, name the VM "DC-1," choose the same region as your virtual network, and select the "Windows Server 2022" image.
- Under Administrator account, set the Username to "labuser" and the Password to "Cyberlab123!"
- Click "Next: Networking," and confirm the Virtual network and Subnet dropdowns show the network created earlier.
- Click "Review + create," then click "Create" once validation passes.
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Create Domain Controller VM"/>
</p>

<br />

<h4>Next, set DC-1's Private IP Address to static</h4>

- On DC-1's overview page in Azure, click "Networking" in the left-hand menu, then click the Network Interface link.
- Click "IP configurations" in the left-hand menu, then click on the listed IP configuration.
- Change the "Assignment" setting from "Dynamic" to "Static."
- Confirm the Private IP address field, then click "Save."
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Set Static Private IP"/>
</p>

<br />

<h4>Next, log into DC-1 and disable the Windows Firewall (for testing connectivity)</h4>

- On DC-1's overview page, click "Connect," then select "RDP," and download and open the RDP file.
- Log in using username "labuser" and password "Cyberlab123!"
- Once logged in, open "Windows Defender Firewall" from the Start menu.
- Click "Turn Windows Defender Firewall on or off" in the left-hand menu.
- Select "Turn off Windows Defender Firewall" for both Private and Public network settings, then click "OK" to save.
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disable Windows Firewall on DC-1"/>
</p>

<br />

<h4>Next, create the Client VM (Windows 10) named "Client-1"</h4>

- In the Azure search bar, type "Virtual machines" and click it when it appears.
- Click "Create," then select "Azure virtual machine."
- On the Basics tab, select the same subscription and resource group used for DC-1, name the VM "Client-1," choose the same region, and select the "Windows 10" image.
- Under Administrator account, set the Username to "labuser" and the Password to "Cyberlab123!"
- Click "Next: Networking," and confirm the Virtual network and Subnet dropdowns match the ones used for DC-1.
- Click "Review + create," then click "Create" once validation passes.
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Create Client VM"/>
</p>

<br />

<h4>Next, set Client-1's DNS settings to DC-1's Private IP address</h4>

- First, note DC-1's private IP address from its Azure overview page.
- Go to Client-1's overview page, click "Networking" in the left-hand menu, then click the Network Interface link.
- Click "DNS servers" in the left-hand menu, select "Custom DNS servers," and enter DC-1's private IP address.
- Click "Save."
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Set Client-1 DNS Settings"/>
</p>

<br />

<h4>Next, restart Client-1 from the Azure Portal</h4>

- On Client-1's overview page in Azure, click the "Restart" button near the top of the page and confirm if prompted.
- Wait a few minutes for the VM to fully restart.
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Restart Client-1"/>
</p>

<br />

<h4>Next, log into Client-1</h4>

- On Client-1's overview page, click "Connect," then select "RDP," and download and open the RDP file.
- Log in using username "labuser" and password "Cyberlab123!"
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Log Into Client-1"/>
</p>

<br />

<h4>Next, attempt to ping DC-1's private IP address from Client-1</h4>

- Within the Client-1 Remote Desktop session, open Command Prompt from the Start menu.
- Type "ping <DC-1-Private-IP-Address>" and press Enter, replacing the placeholder with DC-1's actual private IP address.
- Ensure the ping succeeded — you should see replies coming back from DC-1's IP address.
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Ping DC-1 from Client-1"/>
</p>

<br />

<h4>Next, from Client-1, open PowerShell and run ipconfig /all</h4>

- In the same Command Prompt or a new PowerShell window, type "ipconfig /all" and press Enter.
- Scroll through the output and confirm that the DNS settings show DC-1's private IP Address.
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Verify DNS Settings"/>
</p>

<br />

<h4>Finish the lab, but do not delete the VMs in Azure</h4>

- We will use DC-1 and Client-1 for upcoming labs, so leave them in place.
- If you are done for the day and want to save money, simply "Stop" the VMs within the Azure Portal rather than deleting them.

<br />
