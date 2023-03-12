# Active Directory Simulation in Azure
Howdy. This is a tutorial on how to use Microsoft Azure to simulate Active Directory using two Virtual Machines. This is useful if you want to test changes to a domain before they go live. It's also useful if you want to know how your school laptop works.

## Azure - Subscription and Resource Group
First thing you need to do is go to the [Azure Portal](https://portal.azure.com). Create an account for Azure if you don't have one (keep in mind only the first $200 worth of service is free). A subscription in Azure is linked to a payment option and can contain multiple resource groups. For our simulation we will only need one subscription and one resource group under that subscription

![alt text](https://github.com/Cham0i/AD-creation-Azure/blob/main/screen/one.png)

## Virtual Machines - Creation and Configuration
We will need two virtual machines. The first one will be our DC (Domain Controller). The DC is the computer that will host active directory and it will have all the accounts that other computers within the domain will be able to access.

![alt text](https://github.com/Cham0i/AD-creation-Azure/blob/main/screen/two.png)
![alt text](https://github.com/Cham0i/AD-creation-Azure/blob/main/screen/three.png)

Important things to note while making the VMs (Virtual Machines):

1. Both VMs must be under the same subscription, resource group, and region. This should put both VMs in the same VNet (Virtual Network, think of it as LAN). The option to choose a VNet during creation is under "Networking".

If you want to be sure you did this right, use **Network Watcher>Monitoring>Topography** after creating both VMs to verify they are in the same VNet. If you want to be extra sure, you can even try to ping DC from Client; but make sure the DC has ICMP traffic allowed under **VirtualMachine>[your DC name here]>Settings>Networking**. You can use Add Inbound Porting Rule to add ICMP under "Protocol"

2.The image for the DC must be Windows Server. The client may have regular Windows (and it's more realistic if it does), but for this example, my client computer was also a Windows Server.

3. Use the recommended "hardware" Size (technically not hard since its virtual). Using 1 VCPU and 1GB of memory will not be enough to even allow a RDP (Remote Desktop Protocol) connection.

4. Speaking of RDP, make sure RDP connections are allowed so that we may access the computer. This should already be the default option. 

## Active Directory Domain Service Creation

For the DC, the NIC (Network Interface Card) will need to be static. If it is set to dynamic the domain controller might change addresses which will leave the client computer attempting to connect to DC using the wrong address.

You can look for the NIC here: **Virtual Machine>[your DC name here]>Settings>Networking** "Network Interface: [Your NIC highlighted in blue here]"

Clicking on it will take you to the NIC overview. Under **Settings>IP Configuration** there will be a IP address record that you must click on.

![alt text](https://github.com/Cham0i/AD-creation-Azure/blob/main/screen/13.png)

Set the IP to static and then save.

<h2>Install Active Directory Domain Services</h2>
Login to DC and install Active Directory Domain Services
  
  Promote to DC and name the domain.com
  its common that the pc will restart and kick you from RDP 
  
 Restart and re log in with domain.com/admin
 
 ## Adding Admins, Clients, and Users
 
 Create an admin
 
 use admin to login to domain from client computer to add computer to domain
 
From DC allow any user to login from client

create users and test to see if you can log in from client
