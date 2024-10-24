# Azure-Secure-Linux-Server-Deployment
Azure Cloud Shell is an interactive, authenticated, browser-accessible terminal for managing Azure resources. It provides the flexibility of choosing the shell experience that best suits the way you work, either Bash or PowerShell.

## NETWORK TOOPOLOGY
 ![SOC]

 ## KEY OBJECTIVES
 1️⃣ Create a Resource Group: This is the foundation for your Azure resources—like the sturdy walls of a castle that keep everything inside organized and secure. 🏰

2️⃣ Create a VNET (Virtual Network): Think of this as laying down the roads and highways for your data to travel on. It's where your virtual machines will reside and communicate, much like a bustling city. 🛣️

3️⃣ Deploy Azure Bastion: This is your high-security gatekeeper, ensuring that only the right people get through to your virtual machines, much like a bouncer at an exclusive club. 🕶️

4️⃣ Create two Ubuntu Virtual Machines: Here, you’ll summon your digital minions—two Ubuntu VMs ready to serve your whims. It’s like having your own tech-savvy butler duo! 🤖🤖

5️⃣ Connect to the VMs and establish communication using the simple ping command: Time for a friendly chat! With a quick ping, you'll ensure your VMs can talk to each other—think of it as sending a text to check if your friend is still awake at 3 AM. 📱💬

6️⃣ Clean Up Resources: After all the fun, it’s time to tidy up! Just like putting away your toys after playtime, you’ll want to ensure everything is cleaned up properly to avoid any lingering costs. 🧹

## STEP 1 Create Azure Resource Group

I used New-AzResourceGroup to create a resource group to host the virtual network and the VMs, to create a resource group named ***Aurora-RG*** in the ***centralus*** Azure region.

![SOC]

## STEP 2: Create Virtual Network
Use New-AzVirtualNetwork to create a virtual network named ***Aurora-Vnet*** with IP address prefix ***10.0.0.0/16*** in the ***Aurora-RG*** resource group and ***centralus*** location

![SOC]

## Next: Create a Subnet 
configured subnet, and associated the subnet configurations to the virtual Network to a subnet within the virtual network. Use Add-AzVirtualNetworkSubnetConfig to create a subnet configuration named ***Aurora-Subnet*** with address prefix ***10.0.0.0/24***

![SOC]
