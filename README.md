<h1>Active Directory Simulation Creation in Azure</h1>
Hello. This is a tutorial on how to use Microsoft Azure to simulate Active Directory using 2 Virtual Machines. This is usefull if you want to test implementations to a dummy domain before they are hosted on the real deal. Its also usefull if you want to know how your school laptop works :)
<h2>Azure set up</h2>
First thing you need to do is go to https://portal.azure.com Create an account for Azure if you don't have one (keep in mind only the first $200 worth of service is free). 
A subscription in Azure is linked to a payment option and can contain multiple resource groups. For our simulation we will only need 1 subscription
and 1 resource group under that subscription
<h2>Virtual Machines</h2>
<p>
The first one will be our Domain Controller. The domain controller is the computer that will host active directory and it will have all the accounts that other computers within the domain will be able to access
</p>

Important things to note while making the virtual machines:

1. Both VMs must be under the same subscription, resource group, and region. This should put both VMs in the same Vnet. You may use Network Watcher tool in Azure to verify this. If you want to be extra sure, you can even try to ping Domain Controller from Client; but make sure the Domain Controller has ICMP traffic allowed in incoming traffic??????

2. Use the recommended "hardware" (not hard since its virtual), using 1 core and 1GB of RAM will not be enough to even allow a RDP connection

3. Make sure RDP connections are allowed. This should be the default option

For the Domain Controller, the NIC will need to be static. If it is set to dynamic the domain controller might change addresses which will leave the client computers unable to connect to the domain controller.
<h2>Install Active Directory Domain Services</h2>
Login to DC and install Active Directory Domain Services
  
  Promote to DC and name the domain.com
  its common that the pc will restart and kick you from RDP 
  
 Restart and re log in with domain.com/admin
 
 Create an admin
 
 use admin to login to domain from client computer to add computer to domain
 
From DC allow any user to login from client

create users and test to see if you can log in from client
