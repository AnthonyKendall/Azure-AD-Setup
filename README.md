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

In the end, your page should look like this: 

Part 1: 

<img width="775" height="849" alt="image" src="https://github.com/user-attachments/assets/7bafc29d-53b4-4699-985f-c976c47cd71a" />

Part 2: 

<img width="863" height="502" alt="image" src="https://github.com/user-attachments/assets/a74d42fd-b079-413f-884f-580bf8aad7ee" />



