# Azure AD

## Objective
The objective is to setup a Cloud infrastructure using Azure, to create a domain controller and a windows VM to establish a domain connection and deploy GPO's, create user accounts all across a Cloud environment instead of an on-prem infrastructure.

### Skills Learned
- Cloud Infrastructure & Domain Deployment: Provision Azure resources (vnets, VMs) and deploy a Windows Server Domain Controller with Active Directory and DNS.
- Identity Management & Client Integration: Join client machines to the domain, configure Organizational Units (OUs), and manage user provisioning.
- Operations & Security: Troubleshoot common domain connectivity issues and leverage centralized authentication events for foundational SOC threat monitoring.
- Virtual Networking & Network Security: Configure Azure Virtual Networks (Vnets), subnets, and Network Security Groups (NSGs) to establish secure boundary controls and isolate domain assets.
- DNS Resolution & Routing Rules: Configure custom Vnet DNS server settings to point client workstations directly to the Domain Controller, resolving name-to-IP mismatches during domain join procedures.


# Steps Taken


1. Setup a Azure account for FREE 

First, you need to utilize the free trial version of Azure as it is offered to you for a limited 30 days, during which you will receive $200 credit that you can use for the setup in this lab. Simply navigate to this link and click on "Try Azure for free" https://azure.microsoft.com/en-us/pricing/purchase-options/azure-account?ref=www.google.com&hasfullconsent=true


<img width="1825" height="801" alt="image" src="https://github.com/user-attachments/assets/e4c80d8b-a710-48be-a671-e8a215a21391" />


2. Create a Resource group

Now we need to create a resource group, which is a container, so everything has a home and is easier to manage permissions, apply policies and keep track of cost. 

Navigate to the search bar at the top of the page in Azure and type in "Resource groups". You should see it populate. If not, filter under the "Service" tab and find it via that way. 

<img width="524" height="671" alt="image" src="https://github.com/user-attachments/assets/ca6f8d96-3911-40eb-956a-de73a4bd6a29" />

Once we are in the Resource groups, you will see the "Create" option located at the top left. Click on "Create" 

<img width="1649" height="797" alt="image" src="https://github.com/user-attachments/assets/15d96fed-6199-4bd4-9993-c9a11e3968c9" />

Keep your subscrption set to the default. In my case, its called "Azure Subscription 1"

Resource group name: Azure-AD-Setup

Region: West US 2 (Put this to the region that is closer to you)

The results should look like this: 

<img width="906" height="503" alt="image" src="https://github.com/user-attachments/assets/66c254f2-79b7-4e9a-99ae-5db02ef8b6ec" />

Once you have done this, click on "Review + Create" at the bottom. If everything looks good to you, click on "Create" 


<img width="583" height="903" alt="image" src="https://github.com/user-attachments/assets/4270a507-87b1-469b-bfd3-0b2078da89ce" />

Now, hit the refresh icon in Azure and you should see your resource group you just created. 

<img width="1828" height="396" alt="image" src="https://github.com/user-attachments/assets/a5895552-86b1-4770-8eec-57ca4b52b504" />

3. Create Virtual Machines

Now, we need to create the two virtual machines for this exercise. We are going to create 1x Domain Controller and 1x Windows 11 VM.

So, at the top of the Azure search bar, type in "Virtual Machines" and you should see the service populate. 

<img width="519" height="738" alt="image" src="https://github.com/user-attachments/assets/4aa93a08-3444-4c7e-a56f-ca430340353e" />

From here, click on "Create" then Virtual machine

<img width="1451" height="813" alt="image" src="https://github.com/user-attachments/assets/12de05ed-7367-49ef-a9c9-8ddcbb23dbc4" />

Subscription: Default Azure subscription. 

Resource Group: Add your resource group that you created. In my case it will be Azure-AD-Setup

Virtual Machine Name: Since we are making the Domain Controller first, we will call it "DC1"

Region: Keep default. So in my case, its West US 2

Availability options: Leave this on its default. 

Zone options: Leave this on its default. 

Availability zone: Leave on its default. 

Security type: Leave on its default. 

Image: Select "Windows Server 2022 Datacenter: Azure Edition - x64 Gen2" 

Size: Select "Standard_D2alds_v6 - 2 vcpus, 4 GiB memory" 

Username: Call this what you want, I am going to call it "azureadmin". 

Password: Set this to something secure and long. 

In the end, your page should look like this. Dont worry, we will apply some security rules to only allow your IP to RDP to these servers. 

Part 1: 

<img width="775" height="849" alt="image" src="https://github.com/user-attachments/assets/7bafc29d-53b4-4699-985f-c976c47cd71a" />

Part 2: 

<img width="863" height="502" alt="image" src="https://github.com/user-attachments/assets/a74d42fd-b079-413f-884f-580bf8aad7ee" />

At this stage, you can hit the "Review + Create" at the bottom and validate everything looks right. Azure will warn you that you will be exposing RDP to the internet. This is fine as this is just for testing. You would NEVER want to do this on a system, unless you know what you are doing. We are going to secure it after this step. For now, just click on "Create" 


<img width="889" height="895" alt="image" src="https://github.com/user-attachments/assets/d1fdf0d8-a7ca-4d48-8117-6feb5f9a2249" />


4. Secure the RDP port

Now, we need to secure the RDP port we just opened up to the internet. To do this, go to the new VM you just created > Navigate to Networking > Network settings and find the inbound port rule that is open. 

<img width="1877" height="881" alt="image" src="https://github.com/user-attachments/assets/c041fe2a-3b24-45c3-93f7-02ed1c9aec22" />

Click on the RDP name and for the source just select the option "My IP Address" as this will use your IP as the source. This is what we want. We want to be the only ones who can connect to this server via RDP. Nobody else. 

Leave everything else as default and just click on save at the bottom. 


Now that you have created the Domain Controller VM, lets create the Windows 11 VM now. 


Go back to Virtual Machine creation and now select "Windows 11 Pro". Create your user account for it and leave everything else as default. Its going to be very similar to how you just setup the Domain Controller from the first walkthrough. 

<img width="784" height="840" alt="image" src="https://github.com/user-attachments/assets/51fb9b67-3820-44b2-82d4-d7f78cd0aac4" />


Once you have done the first page, navigate over to the Networking tab and click on Virtual Network. Select the same Virtual Network that your Domain Controller is on, so these two devices can talk to one another. Do the same for the subnet right below it as well. 

<img width="934" height="514" alt="image" src="https://github.com/user-attachments/assets/2327ff15-f5af-4ca3-8b3b-08cc81619840" />


Next, click on "Review + Create" and then Create at the bottom to create the VM. 


<img width="883" height="895" alt="image" src="https://github.com/user-attachments/assets/877d3bb3-bab3-49a9-b4aa-717de5ca065a" />


Now, just like the server, we need to secure this RDP port as well. Simply do the exact same steps as you did for the servers RDP port security and that is it for this portion in creating the VMs. 



5. Testing access to nodes

Before we continue, we need to make sure we can RDP to both assets now. To do this, simply go to "Virtual Machines" again and you will see both of your VMs in a running state. You will also see their public IP address as well. This is the IP we need to RDP to. 

<img width="883" height="895" alt="image" src="https://github.com/user-attachments/assets/1c78a02c-c6ce-4962-a016-598907bbd22c" />

On your local machine, open up RDP and type in the public IP address for each of the computers. 

<img width="661" height="345" alt="image" src="https://github.com/user-attachments/assets/b9c63d84-8e68-4f47-8a22-025abd1a5c6e" />

Login in to them and run updates to ensure they are on the latest updates.

Here we can see the server is updated to the latest. I have checked the Win11 computer and it is updated as well. 

<img width="583" height="528" alt="image" src="https://github.com/user-attachments/assets/e638b869-0fb8-43d3-9269-fc61c31099a8" />


After you have used RDP to get to the servers and updated them, the next test we need to do is to ensure they can ping on another. Simply go to each VM and run ping to see if you can ping the other device. 

To find the IP of your two VM's, simply navigate to the virtual machine and find where you see "Private IP Address" 

<img width="1309" height="846" alt="image" src="https://github.com/user-attachments/assets/4294626e-ca92-4987-9177-c82111d68f90" />



Cant connect to the Win11 VM? 

You might not be able to RDP to your Win11 VM. This is due to the Windows Firewall. I had to go to the "Run Command" Feature in the VM to see if the firewall rule was enabled. As you can see, its set to FALSE. I need to switch this to enabled, so I can connect to the VM. Usually with a fresh build of Win11 the built-in firewall rule is disabled or unconfigured until RDP is explicitly turned on. 

<img width="1279" height="892" alt="image" src="https://github.com/user-attachments/assets/43a159f1-8232-4d73-a693-bc561779a7f3" />

Navigate to the Run Command option > Select "RunPowerShellScript" and then paste this script to see if your firewall rules say False as well: 

PS - Get-NetFirewallRule | Select-Object Name, DisplayName, DisplayGroup, Enabled

If it says False, you need to run this: 

PS - Enable-NetFirewallRule -DisplayGroup "Remote Desktop"

Re-Check your firewall rules now using Get-NetFirewallRule -DisplayGroup "Remote Desktop". Notice how its set to True now. 

<img width="823" height="902" alt="image" src="https://github.com/user-attachments/assets/e257cb1b-e2e9-4e46-bc0b-1d720eafa604" />


Now test RDP again to the VM and you should be able to get access this time. I am able to successfully RDP to the Win11 VM


<img width="1636" height="925" alt="image" src="https://github.com/user-attachments/assets/1c584c81-1b9b-4af6-ba39-85b6c2305cde" />



Now, getting back to testing Ping from both machines to ensure they can talk with one another. 

As we can see, the DC1 is not able to ping the Win11 computer 

<img width="2816" height="852" alt="image" src="https://github.com/user-attachments/assets/0267caac-a413-4085-b7af-1196422bcd24" />

We now need to allow ping in the firewall on the Win11 side and the Domain controller. To do this, simply run this PowerShell script in both computers to make this go by quicker. 

PS - Enable-NetFirewallRule -Name "CoreNet-Diag-ICMP4-EchoRequest-In"

I can now ping the Win11 VM and it can ping the server


DC -> Win11 ping: 

<img width="493" height="207" alt="image" src="https://github.com/user-attachments/assets/98f59019-7227-4c9b-804c-84c818953b4c" />

Win11 -> DC1 ping: 

<img width="1344" height="708" alt="image" src="https://github.com/user-attachments/assets/fff829c7-cf12-44c0-8f33-cec9be661b01" />


Side note: We will have to assign a static IP to the Domain Controller before proceeding. To do this, we need to statically set the IP in Azure. 

Go to the Domain Controller VM > Networking > Network Settings > Network Interface 

<img width="1851" height="850" alt="image" src="https://github.com/user-attachments/assets/cd1e11bf-4984-419b-99df-b43019c28c1a" />

From here, click on "ipconfig1" at the bottom 

<img width="1488" height="756" alt="image" src="https://github.com/user-attachments/assets/a95c396e-273d-4c1b-ad7d-103b76f0b7a2" />

Then click on "Static" under Private IP Address settings

<img width="572" height="503" alt="image" src="https://github.com/user-attachments/assets/976a80e0-b077-4992-9603-b507955b5bc6" />


Keep the IP that its on or change it. Its up to you for this portion. Once you have the IP, just click on Save at the bottom. 

Once you save the settings, restart your Domain Controller VM. 

From here, we can now continue on with setting up AD for the domain controller. 




Perfect, now both machines can ping each other. The next step is to create AD on the domain controller. 


6. Creating AD on a domain controller

RDP to your domain controller VM that you created. Once in, go to "Manage" at the top right, then click on "Add Roles and Features" 

<img width="1344" height="708" alt="image" src="https://github.com/user-attachments/assets/bb815fef-5e22-4619-879b-15752815c0b2" />

From here, click Next at the bottom

<img width="862" height="596" alt="image" src="https://github.com/user-attachments/assets/58b6d96a-2a6a-4815-babd-5528318a5be5" />


Click on "Role-based or feature-based installation" then click Next at the bottom. 


<img width="961" height="610" alt="image" src="https://github.com/user-attachments/assets/71538e4b-d9f4-4b82-978f-86d425b32a11" />


Leave everything selected for the destination server and just click on Next 


<img width="1029" height="678" alt="image" src="https://github.com/user-attachments/assets/2cd02e4d-c74e-4501-ae64-b3cfdd41313f" />

From here, select "Active Directory Domain Services" AND "DNS Server". Select add features for both. 

AD: 

<img width="956" height="604" alt="image" src="https://github.com/user-attachments/assets/d0faabd6-94d5-4fa5-adc0-0f88a435d68f" />

DNS: 

<img width="553" height="468" alt="image" src="https://github.com/user-attachments/assets/6dbbd66c-c326-4f30-99fd-9e2fa3b0e97b" />

You will get a Pop up stating that there is no static IP detected. You can ignore this as we statically assigned the IP in Azure. Just click on continue. 

<img width="681" height="486" alt="image" src="https://github.com/user-attachments/assets/1a83ee71-5fca-4a21-89b2-2b26b544d90c" />


Click Next for the rest of everything until you get to the end of the page for "Confirm installation selections". Once here, double check everything and then click Install: 


<img width="825" height="599" alt="image" src="https://github.com/user-attachments/assets/53ce1a5a-bc5d-40e8-8433-48f836a940b3" />


When the install is finish, we want to hit "Promote to a domain controller" 

<img width="803" height="582" alt="image" src="https://github.com/user-attachments/assets/b3a92c6b-4b31-4334-b69c-aefe0529fbd8" />


From here, select "Add a new forest" and type the domain name you want it to be. For me I will do Astra.local 

<img width="959" height="679" alt="image" src="https://github.com/user-attachments/assets/0653063d-38f1-40b8-846b-c3b62b3fb08a" />

Click next and leave everything default here. Just set a password for DSRM

<img width="918" height="696" alt="image" src="https://github.com/user-attachments/assets/18774d04-cea4-41c8-a2b1-453c03dd2c42" />


Click next on the DNS options and just skip that. 

Next, wait for the NETBIOS name to appear. Once it appears, click on next 


<img width="964" height="656" alt="image" src="https://github.com/user-attachments/assets/117f20ff-605c-4618-8ba1-983c53b4b043" />


Leave the Paths section as default and just hit next. 

Click on next for the review options section as well 


Make sure prerequisite checks pass and then click on install 


<img width="1075" height="748" alt="image" src="https://github.com/user-attachments/assets/d1156271-4996-450b-b5d8-e4b76815a2ca" />


The server will reboot after this. After its done rebooting, RDP back into the server. You will notice the domain name is now appended to the end of the hostname. 

<img width="406" height="409" alt="image" src="https://github.com/user-attachments/assets/367f8762-d27b-4231-a8c3-8f831da7fcb7" />


Okay, now that we are back in the server, we need to 



