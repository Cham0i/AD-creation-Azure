<h1>Active Directory Simulation Creation in Azure</h1>
Hello. This is a tutorial on how to use Microsoft Azure to simulate Active Directory using 2 Virtual Machines. This is usefull if you want to test implementations to a dummy domain before they are hosted on the real deal. Its also usefull if you want to know how your school laptop works :)

<h2>Azure set up</h2>
First thing you need to do is go to https://portal.azure.com Create an account for Azure if you don't have one (keep in mind only the first $200 worth of service is free). 

![alt text](https://i.imgur.com/d4lzdkz.png)

A subscription in Azure is linked to a payment option and can contain multiple resource groups. For our simulation we will only need 1 subscription
![alt text](https://i.imgur.com/FsU2GSi.png)
and 1 resource group under that subscription
![alt text](https://i.imgur.com/CpRDDBk.png)
We will then need to create 2 virtual machines

<h2>Virtual Machines</h2>
<p>The first one will be our Domain Controller. The domain controller is the computer that will host active directory and it will have all the accounts that other computers within the domain will be able to access</p>
Important things to note while making the virtual machines
1) Both VMs must be under the same subscription, resource group, and region
2) 
![alt text](https://i.imgur.com/NVpaIS8.png)
something
![alt text](https://i.imgur.com/se5fXfw.png)
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
