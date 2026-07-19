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
<img width="965" height="636" alt="image" src="https://github.com/user-attachments/assets/e6050358-05ea-4bbc-8107-d33de6d66a89" />
</p>

<br />

<h4>Next, create a Virtual Network and Subnet. DC-1 and Client-1 will both live inside this network so they can communicate with each other.</h4>

- In the Azure search bar, type "Virtual networks" and click it when it appears.
- Click "Create" near the top-left of the page.
- On the Basics tab, select your subscription, choose the resource group created above, name the virtual network (e.g., "DomainLab-VNet"), and select the same region used previously.
- Click "Next: IP Addresses," and leave the default address space and subnet, or customize as needed.
- Click "Review + create," then click "Create" once validation passes.
  
<p>
<img width="1007" height="747" alt="image" src="https://github.com/user-attachments/assets/afa6af56-e60b-4758-be88-936971c4a1e4" />
</p>

<br />

<h4>Next, create the Domain Controller VM (Windows Server 2022) named "DC-1"</h4>

- In the Azure search bar, type "Virtual machines" and click it when it appears.
- Click "Create," then select "Azure virtual machine."
- On the Basics tab, select your subscription and resource group, name the VM "DC-1," choose the same region as your virtual network, and select the "Windows Server 2022" image.
- Under the Administrator account, set the Username to "labuser" and the Password to "Cyberlab123!"
- Click "Next: Networking," and confirm the Virtual network and Subnet dropdowns show the network created earlier.
- Click "Review + create," then click "Create" once validation passes.
<p>
<img width="983" height="695" alt="image" src="https://github.com/user-attachments/assets/3eefea9e-628e-45c9-b2be-56bbd8c34395" />
</p>

<br />

<h4>Next, set DC-1's Private IP Address to static</h4>

- On DC-1's overview page in Azure, click "Networking" in the left-hand menu, then click "Network settings", then click the Network Interface link.
- Click "IP configurations" in the left-hand menu, then click on the listed IP configuration.
- Change the "Assignment" setting from "Dynamic" to "Static."
- Confirm the Private IP address field, then click "Save."
<p>
<img width="976" height="726" alt="image" src="https://github.com/user-attachments/assets/f333fb7e-64dc-4d7a-b26d-a9dc5e43b50d" />
</p>

<br />

<h4>Next, log into DC-1 and disable the Windows Firewall (for testing connectivity)</h4>

- On DC-1's overview page in Azure, note its Public IP address (listed on the Overview page).
<b>Connect to DC-1 using one of the following methods:</b>
  - Option A — Azure Portal: Click "Connect," then select "RDP," and download and open the RDP file. Log in using username "labuser" and password "Cyberlab123!"
  - Option B — Remote Desktop Connection app: Click the Start menu on your local computer, type "Remote Desktop Connection," and open it. Enter DC-1's public IP address in the "Computer" field, click "Show Options," and enter "labuser" in the "User name" field, then click "Connect."When prompted, enter the password "Cyberlab123!", then click "OK."
- Right-click the Start menu, click "Run," type "fw.msc," and press Enter (or click "OK").
- Click the "Windows Defender Firewall Properties" link.
- Turn off Windows Defender Firewall for Domain, Private and Public network profiles, click "Apply", then click "OK" to save.


<p>
</p>
<img width="1277" height="895" alt="image" src="https://github.com/user-attachments/assets/df07d5b7-a638-4eff-b4d2-f650d8b99539" />
<br />

<h4>Next, create the Client VM (Windows 10) named "Client-1"</h4>

- In the Azure search bar, type "Virtual machines" and click it when it appears.
- Click "Create," then select "Azure virtual machine."
- On the Basics tab, select the same subscription and resource group used for DC-1, name the VM "Client-1," choose the same region, and select the "Windows 10" image.
- Under Administrator account, set the Username to "labuser" and the Password to "Cyberlab123!"
- Click "Next: Networking," and confirm the Virtual network and Subnet dropdowns match the ones used for DC-1.
- Click "Review + create," then click "Create" once validation passes.
<p>
<img width="1001" height="666" alt="image" src="https://github.com/user-attachments/assets/98a15783-3e9c-42f4-bda6-e479b80f1825" />
</p>

<br />

<h4>Next, set Client-1's DNS settings to DC-1's Private IP address</h4>

- First, copy DC-1's private IP address from its Azure overview page.
- Go to Client-1's overview page, click "Networking" in the left-hand menu, then click "Network settings", then click the Network Interface link.
- Click "DNS servers" in the left-hand menu, select "Custom DNS servers," and enter DC-1's private IP address.
- Click "Apply."
<p>
<img width="1248" height="757" alt="image" src="https://github.com/user-attachments/assets/c24daf35-63c1-431a-9b17-bbc70639cc2c" />
</p>

<br />

<h4>Next, restart Client-1 from the Azure Portal</h4>

- On Client-1's overview page in Azure, click the "Restart" button near the top of the page and confirm if prompted.
- Wait a few minutes for the VM to fully restart.
<p>
<img width="821" height="396" alt="image" src="https://github.com/user-attachments/assets/62408e86-24c3-4af3-b8b0-e730768f28d8" />
</p>

<br />

<h4>Next, log into Client-1</h4>

<b>Connect to Client-1 using one of the following methods:</b>
  - Option A — Azure Portal: Click "Connect," then select "RDP," and download and open the RDP file. Log in using username "labuser" and password "Cyberlab123!"
  - Option B — Remote Desktop Connection app: Click the Start menu on your local computer, type "Remote Desktop Connection," and open it. Enter Client-1's public IP address in the "Computer" field, click "Show Options," and enter "labuser" in the "User name" field, then click "Connect."When prompted, enter the password "Cyberlab123!", then click "OK."
<p>
<img width="1242" height="532" alt="image" src="https://github.com/user-attachments/assets/201e2313-f5a8-4c6c-8eea-78627ded189e" />
</p>

<br />

<h4>Next, attempt to ping DC-1's private IP address from Client-1</h4>

- Within the Client-1 Remote Desktop session, open Command Prompt from the Start menu.
- Type "ping <DC-1-Private-IP-Address>" and press Enter, replacing the placeholder with DC-1's actual private IP address.
- Ensure the ping succeeded — you should see replies coming back from DC-1's IP address.
<p>
<img width="713" height="480" alt="image" src="https://github.com/user-attachments/assets/c96da01b-5c52-4cf7-b473-f6a7803d81d0" />
</p>

<br />

<h4>Next, from Client-1, open PowerShell and run ipconfig /all</h4>

- In the same Command Prompt, open PowerShell from the start menu, type "ipconfig /all" and press Enter.
- Scroll through the output and confirm that the DNS settings show DC-1's private IP Address.
<p>
<img width="1337" height="452" alt="image" src="https://github.com/user-attachments/assets/2880e9a6-adac-4cbe-b495-1f45b4b458e4" />
</p>

<br />

<h4>Finish the lab, but do not delete the VMs in Azure</h4>

- We will use DC-1 and Client-1 for upcoming labs, so leave them in place.
- If you are done for the day and want to save money, simply "Stop" the VMs within the Azure Portal rather than deleting them.

<br />
