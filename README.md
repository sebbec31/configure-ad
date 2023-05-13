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

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create and Admin and User Account in Active Directory
- Join Client-1 to your domain
- Setup Remote Desktop for non-administrative users on Client-1

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/0f85b760-43d4-419c-8873-4198e48deb25" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
The first step is to create the resources necessary in order to run this lab properly. Name the resource group "AD-Lab", and the virtual machhine "DC-1". Make sure you choose a region that is close to you, and select "Windows Server 2022". For the size, make sure that the VM has at least 2 VCPU's then go ahead and select "Review + Create".
</p>
<br />

<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/273c6d47-3a01-456b-93a3-373a8608afc0" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go ahead and create another virtual machine within the same resource group and name it "Client-1". Choose the same region, and select "Windows 10 Pro, version 22H2 - x64 Gen2". Make it a similar size, then go to "Networking" and make sure it is under the same virtual network as the other VM.
</p>
<br />

<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/b0bb6ac2-3996-4efe-86ef-ad0152ba6fe8" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now go into DC-1 through the Azure portal > Networking > Network Interface: dc-xxxx.
</p>
<br />

<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/55f7c7e7-ed72-43c1-8b34-d8eee338b3d7" height="80%" width="80%" alt="1"/>
</p>
<p>
Head over to "IP Configurations" and click "ipconfig1".
</p>
<br />


<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/52d74705-1786-4f9b-918c-fbfd39105b49" height="80%" width="80%" alt="2"/>
</p>
<p>
Change the IP from "Dynamic" to "Static" and click "Save".
</p>
<br />


<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/87bfbe3f-30e6-48ec-94e8-2e8b49508fd2" height="80%" width="80%" alt="3"/>
</p>
<p>
Now, login into thhe "Client-1" VM and open the CommandPrompt. Try to ping the "DC-1" Private IP by typing in "ping -t". The request will be timed out, but be sure to keep this VM running as we will fix this in the next few steps.
</p>
<br />


<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/7eac0622-69b8-4736-8b6b-813a29b4e22a" height="80%" width="80%" alt="4"/>
</p>
<p>
Login into the "DC-1" VM and open "Windows Defender Firewall with Advanced Security"
</p>
<br />


<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/0d0e45ea-a55b-42d0-8372-a7ccfcd01cf2" height="80%" width="80%" alt="5"/>
</p>
<p>
From here, go to inbound rules > Enable both "Core Networking Diagnostics". This will allow ICMPv4 traffic that ping uses.
</p>
<br />


<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/46b6097f-69a2-4ab6-99de-5ca73eadb88b" height="80%" width="80%" alt="6"/>
</p>
<p>
Head back to the "Client-1" VM and now there should be replies in the CommandPrompt. If this is happening go ahead and stop the pinging by pressing "CTRL + C" in the Command line.
</p>
<br />


<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/bcfb6d77-f036-48ff-81a3-37f5fd530a59" height="80%" width="80%" alt="7"/>
</p>
<p>
Now we are going to install Active Director. Inside the "DC-1" VM go into "Server Manager" and click on "Add roles and features". Continue to click on "Next" until you reach "Server Roles", and once you do go ahead and check "Active Directory Domain Services". Finally, click "Add Features" then continue to click "Next" to install.
</p>
<br />


<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/56145dad-ed9e-45f1-8c34-21000b3cd6c8" height="80%" width="80%" alt="8/>
</p>
<p>
In this step, we are going to promote the VM as a main controller. Click on the flag with the caution sign inside of "Server Manager" and click "Promote this server to a domain controller".
</p>
<br />

<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/fabdfaa5-040e-49a8-9497-eb1358951ba2" height="80%" width="80%" alt="9/>
</p>
<p>
Go ahead and check "Add a new forest" and name it "mydomain.com" and click next.
</p>
<br />

<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/9432538a-d1ef-4c91-80e0-33179a62a3dc" height="80%" width="80%" alt="10"/>
</p>
<p>
Create a "DSRM' password, although this password will not be used in this tutorial so do not worry about it too much. 
</p>
<br />

<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/27dba8e0-3ae9-41af-b054-e35e4e19a4f9" height="80%" width="80%" alt="11"/>
</p>
<p>
Once you create a password and continue the VM sign you out and restart.
</p>
<br />

<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/54421335-f21b-4be9-8c4c-3d118be743f6" height="80%" width="80%" alt="12"/>
</p>
<p>
 Once the "DC-1" VM has restarted connect back into it with RDC. This time we will be using a different user from the new root domain we added. Type "mydomain.com\labuser" as the user and use the same password that you created with the VM.

</p>
<br />

<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/52cdd56f-5ab5-428e-bc58-dafb2cedc30f" height="80%" width="80%" alt="13"/>
</p>
<p>
Now go to the windows search bar, type "Active Directory Users and Computers" and click it. 
</p>
<br />

<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/7d9421c3-6ba6-4634-b6d1-6330a1e17cce" height="80%" width="80%" alt="14"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/59e90252-fc9b-4bca-966c-2a3d9c6aea71" height="80%" width="80%" alt="15"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/9204b430-be71-4c1e-b2c3-431a073e9162" height="80%" width="80%" alt="16"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/29270441-2dc5-4357-8cf4-4fa521798295" height="80%" width="80%" alt="17"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/d4dc4485-b04c-4c6a-ab51-7dba65e3b5d6" height="80%" width="80%" alt="18"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/d27de5f9-277a-4fa4-a732-a233afea2dec" height="80%" width="80%" alt="19"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/ce59c5e0-89ff-4bde-a6a9-50b8aa8539c7" height="80%" width="80%" alt="20"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/3e530d67-b0c6-4780-a2da-2843913624a6" height="80%" width="80%" alt="21"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/890eee97-3931-4001-839e-72fc0f1ff432" height="80%" width="80%" alt="22"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/c7cfaa91-a563-45c2-92be-47446791d16f" height="80%" width="80%" alt="23"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/1777043b-5565-42d2-815c-3e5b8b7c7f61" height="80%" width="80%" alt="24"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://github.com/sebbec31/configure-ad/assets/125160491/e91f4ded-7019-4792-a4bc-f94fc6a5dd1d" height="80%" width="80%" alt="25"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />



