# Azure-Secure-Linux-Server-Deployment
Azure Cloud Shell is an interactive, authenticated, browser-accessible terminal for managing Azure resources. It provides the flexibility of choosing the shell experience that best suits the way you work, either Bash or PowerShell.

## NETWORK TOPOLOGY
 ![SOC](https://github.com/Virus192/Azure-Secure-Linux-Server-Deployment/blob/main/Images/photo_5825543737802081412_w.jpg)

 ## KEY OBJECTIVES
 1️⃣ Create a Resource Group: This is the foundation for your Azure resources—like the sturdy walls of a castle that keep everything inside organized and secure. 🏰

2️⃣ Create a VNET (Virtual Network): Think of this as laying down the roads and highways for your data to travel on. It's where your virtual machines will reside and communicate, much like a bustling city. 🛣️

3️⃣ Deploy Azure Bastion: This is your high-security gatekeeper, ensuring that only the right people get through to your virtual machines, much like a bouncer at an exclusive club. 🕶️

4️⃣ Create two Ubuntu Virtual Machines: Here, you’ll summon your digital minions—two Ubuntu VMs ready to serve your whims. It’s like having your own tech-savvy butler duo! 🤖🤖

5️⃣ Connect to the VMs and establish communication using the simple ping command: Time for a friendly chat! With a quick ping, you'll ensure your VMs can talk to each other—think of it as sending a text to check if your friend is still awake at 3 AM. 📱💬

6️⃣ Clean Up Resources: After all the fun, it’s time to tidy up! Just like putting away your toys after playtime, you’ll want to ensure everything is cleaned up properly to avoid any lingering costs. 🧹

## STEP 1 Create Azure Resource Group

I used New-AzResourceGroup to create a resource group to host the virtual network and the VMs, to create a resource group named ***Aurora-RG*** in the ***centralus*** Azure region.

![SOC](https://github.com/Virus192/Azure-Secure-Linux-Server-Deployment/blob/main/Images/photo_5823291937988397667_w.jpg)

## STEP 2: Create Virtual Network
Use New-AzVirtualNetwork to create a virtual network named ***Aurora-Vnet*** with IP address prefix ***10.0.0.0/16*** in the ***Aurora-RG*** resource group and ***centralus*** location

## Next: Create a Subnet 
configured subnet, and associated the subnet configurations to the virtual Network to a subnet within the virtual network. Use Add-AzVirtualNetworkSubnetConfig to create a subnet configuration named ***Aurora-Subnet*** with address prefix ***10.0.0.0/24***

![SOC](https://github.com/Virus192/Azure-Secure-Linux-Server-Deployment/blob/main/Images/photo_5825543737802081335_w.jpg)

## STEP 3: Deploy Azure Bastion
Azure Bastion enables secure, browser-based access to virtual machines (VMs) within your virtual network using Secure Shell (SSH) or Remote Desktop Protocol (RDP). This solution eliminates the need for VMs to have public IP addresses, client software, or any additional configurations, enhancing security and simplifying management. By leveraging Azure Bastion, cloud engineers can ensure that remote connections are securely facilitated through private IP addresses, reducing the attack surface and adhering to best practices for cloud security.

First, I configured a dedicated Bastion subnet within the virtual network. This subnet is exclusively reserved for Azure Bastion resources and must be named **AzureBastionSubnet** to ensure proper functionality and compliance with Azure requirements.

![SOC](https://github.com/Virus192/Azure-Secure-Linux-Server-Deployment/blob/main/Images/photo_5825543737802081340_w.jpg)

## Next Create Bastion Public IP

- set up a public IP address for the Bastion host, allowing it to access SSH and RDP over port 443.
- Then Finally, I used the **New-AzBastion** command to create a new Standard SKU Bastion host in ***AzureBastion-Subnet***.

![SOC](https://github.com/Virus192/Azure-Secure-Linux-Server-Deployment/blob/main/Images/photo_5825543737802081346_w.jpg)

## STEP 4: Create Ubuntu Servers
For the first Server, (Aurora-Server1) Set the administrator and password and provision the virtual Machine;

![SOC](https://github.com/Virus192/Azure-Secure-Linux-Server-Deployment/blob/main/Images/photo_5823291937988397644_w.jpg)

Use New-AzVM to create two VMs named ***Aurora-Server1*** and ***Aurora-Server2*** in the subnet of the virtual network. When prompted for credentials, enter usernames and passwords for the VMs.

![SOC](https://github.com/Virus192/Azure-Secure-Linux-Server-Deployment/blob/main/Images/photo_5823291937988397645_w.jpg)

## STEP 5 Lauch Servers and Establish Connection

Next, I connected to my VMs in the Azure Portal using the Bastion host I deployed earlier. This allows for secure RDP (3389) and SSH (22) connections over port 443, eliminating the need for public-facing IP addresses and enhancing the security of our connections. 🔒

First Server (**Aurora-Server1**)

    -In the portal, search for and select Virtual machines. 🖥️
    -On the Virtual machines page, select Aurora-Server1.
    -In the Overview section for Aurora-Server1, click Connect.
    -On the Connect to virtual machine page, select the Bastion tab.
    -Choose Use Bastion.
    -Enter the username and password you created for the Aurora-Server1, then click Connect. 🚀

![SOC](https://github.com/Virus192/Azure-Secure-Linux-Server-Deployment/blob/main/Images/photo_5823291937988397647_w.jpg)

Then, at the bash prompt for Aurora-Server1, enter the command ping -c 4 Aurora-Server2. This command sends four ICMP echo requests to Aurora-Server2, allowing you to check the network connectivity between the two servers. It will display the response times and confirm whether Aurora-Server2 is reachable from Aurora-Server1. Monitoring this connectivity can help diagnose network issues or verify that both servers are properly communicating within your infrastructure. 🌐

![SOC](https://github.com/Virus192/Azure-Secure-Linux-Server-Deployment/blob/main/Images/photo_5823291937988397648_w.jpg)

As we can see, the ping was successful, with all packets delivered. This indicates that Aurora-Server1 can successfully communicate with Aurora-Server2, confirming that the network connection between the two servers is functioning properly. ✅

Now, we can close the Bastion connection to Aurora-Server1. 
## To connect to Aurora-Server2; 
Simply repeat the steps outlined earlier for accessing Aurora-Server1. This will ensure that you can establish a secure connection to Aurora-Server2 and perform any necessary tasks or checks on that server as well. 🔄

![SOC](https://github.com/Virus192/Azure-Secure-Linux-Server-Deployment/blob/main/Images/photo_5823291937988397649_w.jpg)

Then, at the bash prompt for Aurora-Server2, enter the command ping -c 6 Aurora-Server1. This command sends four ICMP echo requests to Aurora-Server1, allowing you to check the network connectivity between the two servers. It will display the response times and confirm whether Aurora-Server1 is reachable from Aurora-Server2. Monitoring this connectivity can help diagnose network issues or verify that both servers are properly communicating within your infrastructure. 🌐

![SOC](https://github.com/Virus192/Azure-Secure-Linux-Server-Deployment/blob/main/Images/photo_5823291937988397651_w.jpg)

## Final STEP: De-provisioning

When you've completed your tasks with cloud resources, it's crucial to de-provision them properly to prevent unnecessary billing costs. Failing to do so can lead to unexpected charges, especially if resources are left running.

In my PowerShell session, I used the Remove-AzResourceGroup cmdlet to delete the entire resource group along with all its associated resources. This command ensures that everything within that resource group, including virtual machines, storage accounts, and any other resources, is removed in one go, streamlining the cleanup process.

Here's the command I executed:
***Remove-AzResourceGroup -Name "YourResourceGroupName" -Force***

![SOC](https://github.com/Virus192/Azure-Secure-Linux-Server-Deployment/blob/main/Images/photo_5823291937988397660_w.jpg)

Adding the -Force parameter bypasses the confirmation prompt, allowing for a quicker cleanup. Always double-check the resource group name to avoid accidentally deleting the wrong resources. By de-provisioning effectively, you help manage your cloud costs and maintain a tidy environment. 🧹💰

## Conclusion and Summary
I have successfully completed the objectives for this homelab, and I appreciate how this experience pushed me beyond my comfort zone in the GUI environment, allowing me to become more proficient in using the Azure Cloud shell.

During this project:

    I fully provisioned and de-provisioned Two Ubuntu Server environment using Azure PowerShell.
    I set up and deployed a secure Bastion host to enable remote communication over port 443. The Bastion deployment is on the same subnet as my VMs, facilitating communication between them via their respective private IP addresses.

This project not only enhanced my technical skills but also reinforced the importance of efficient resource management and secure configurations in cloud environments. 

