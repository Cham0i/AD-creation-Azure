# AD-creation-Azure
Creating a Domain Controller and Client VM within Azure to simulate Active Directory set up

Hello. This is a tutorial on how to use Microsoft Azure to simulate Active Directory using 2 Virtual Machines

First thing you need to do is go to https://portal.azure.com
Create and account for Azure if you don't have one (keep in mind only the first $200 worth of service is free).

A subscription in Azure is linked to a payment option and can contain multiple resource groups. For our simulation we will only need 1 subscription and 1 resource group under that subscription

We will then need to create 2 virtual machines

The first one will be our Domain Controller
  The domain controller is the computer that will host active directory and it will have all the accounts that other computers withing the domain will be able to access
  
  The NIC will need to be static, because if it is dynamic the domain controller might change addresses which will leave the client computers unable to connect to the domain controller
  
  

The second one will be our Client
  It is used to represent one of many computers in the domain which is able to use the domain controller to access the embeded users.
  
  Use the same resource group and Virtual Network (LAN essentially) as the domain controller. You can use network watcher to confirm. Login to client and try to ping DC
  
  using DC firewall enable ICMPv4 to make sure the ping succeeds
  
  
  Time to install Active Directory services
  
  Login to DC and install Active Directory Services
  
  Promote to DC and name the domain.com
  its common that the pc will restart and kick you from RDP 
  
 Restart and re log in with domain.com/admin
 
 Create an admin
 
 use admin to login to domain from client computer to add computer to domain
 
From DC allow any user to login from client

create users and test to see if you can log in from client
