# Active Directory Simulation in Azure
Howdy. This is a tutorial on how to use Microsoft Azure to simulate Active Directory using two Virtual Machines. This is useful if you want to test changes to a domain before they go live or if you are simply curious to know how school laptops work.

## Azure - Subscription and Resource Group
First thing you need to do is go to the [Azure Portal](https://portal.azure.com). Create an account for Azure if you don't have one (keep in mind only the first $200 worth of service is free). A subscription in Azure is linked to a payment option and can contain multiple resource groups. For our simulation we will only need one subscription and one resource group under that subscription

![alt text](https://github.com/Cham0i/AD-creation-Azure/blob/main/screen/one.png)

## Virtual Machines - Creation and Configuration
We will need two virtual machines. The first one will be our DC (Domain Controller). The DC is the computer that will host active directory and it will have all the accounts that other computers within the domain will be able to access.

![alt text](https://github.com/Cham0i/AD-creation-Azure/blob/main/screen/two.png)
![alt text](https://github.com/Cham0i/AD-creation-Azure/blob/main/screen/three.png)

Important things to note while making the VMs (Virtual Machines):

1. Both VMs must be under the same subscription, resource group, and region. This should put both VMs in the same VNet (Virtual Network, think of it as LAN). The option to choose a VNet during creation is under "Networking".

 If you want to be sure you did this right, use **NetworkWatcher>Monitoring>Topography** after creating both VMs to verify they are in the same VNet. If you want to be extra sure, you can even try to ping DC from Client; but make sure the DC has ICMP traffic allowed under **VirtualMachines>[your DC name here]>Settings>Networking**. You can use Add Inbound Porting Rule to add ICMP under "Protocol"

2.The image for the DC must be Windows Server. The client may have regular Windows (and it's more realistic if it does), but for this example, my client computer was also a Windows Server.

3. Use the recommended "hardware" Size (technically not hard since its virtual). Using 1 VCPU and 1GB of memory will not be enough to even allow a RDP (Remote Desktop Protocol) connection.

4. Speaking of RDP, make sure RDP connections are allowed so that we may access the computer. This should already be the default option.
5. Write the Username and Passwords for both VMs so you don't forget

## Installing Active Directory Domain Service

For the DC, the NIC (Network Interface Card) will need to be static. If it is set to dynamic the domain controller might change addresses which will leave the client computer attempting to connect to DC using the wrong address.

You can look for the NIC here: **VirtualMachines>[your DC name here]>Settings>Networking** "Network Interface: [Your NIC highlighted in blue here]"

Clicking on it will take you to the NIC overview. Under **Settings>IP Configuration** there will be a IP address record that you must click on. Set the IP Address to static and then save.

![alt text](https://github.com/Cham0i/AD-creation-Azure/blob/main/screen/13.png)

Now we will use RDP to access our DC VM. If you are using Windows, just look up RDP in your computer; since I was using Linux I had to install XRDC as a RDP alternative. Both work the same, just type the Public IP Address of the DC (**VirtualMachines>[your DC name here]>Overview>Essentials**) and connect. 

![alt text](https://github.com/Cham0i/AD-creation-Azure/blob/main/screen/four.png)

You will be prompted to enter a username and password. The username and password are the same ones established during the creation of the VM.

Usually, Server Manager will open on start-up. In case it doesnt, you can easily find it in the start menue (Press Win/Cmd key). Under the Server Manager we will click "2. Add Roles and Features"

![alt text](https://github.com/Cham0i/AD-creation-Azure/blob/main/screen/five.png)

You can ignore most of the set up and click "next" until you reach the "Server Roles" section. In here you will select "Active Directory Domain Services" and "Install" before continuing until you are finished with the software wizard.

![alt text](https://github.com/Cham0i/AD-creation-Azure/blob/main/screen/seven.png)

Once you come back, you will see a flag at the top. Clicking it will open a pop-up. Clicking "promote this server to a domain controller" will open another software wizard.

![alt text](https://github.com/Cham0i/AD-creation-Azure/blob/main/screen/eight.png)

Since we are making a brand new domain we will select the "Add a new forest" option and provide a root domain name below. Beware that the name needs to use a fully qualified domain name (domain.com; domain.net; domain.org; etc.) **Don't forget your domain name, write it down if you have to.**

![alt text](https://github.com/Cham0i/AD-creation-Azure/blob/main/screen/nine.png)

At this point you can ignore everything else in the software wizard. Once you get to the end and start installing, you might loose connection to the VM as it restarts (or prompts you to restart) in order for the changes to take effect. A RDP back into the VM **might** require using the domain admin username  (DomainName.com\Original_VMs_Username or Original_VMs_Username@DomainName.com) instead of the original VM's username. The next section will have screenshot examples of this.

![alt text](https://github.com/Cham0i/AD-creation-Azure/blob/main/screen/ten.png)

## Active Directory Configuration
 
Just like that, we should have Active Directory installed. Now what is left is to add our Client VM to our domain. To do this we are going to:

1-Add an admin account within our DC (and demonstrate how to add other users)

2-Change the ClientVM NIC to use our DC as a DNS server within Azure

3-Find our domain within the ClientVM and use our admin credentials to add the VM to our domain, then allowing other users to RDP into ClientVM.
 
### 1. Account/User Creation Process
 
 add pic to find 11.png
![alt text](https://github.com/Cham0i/AD-creation-Azure/blob/main/screen/11.png)
Add user (preferably under a new file thingy)
![alt text](https://github.com/Cham0i/AD-creation-Azure/blob/main/screen/12.png)
make user into admin


we make another user named joe under student thingy

![alt text](https://github.com/Cham0i/AD-creation-Azure/blob/main/screen/20.png)
 
 ### 2. DNS Set-up
 
Remember how to get to the NIC? Well this time we will go to the Client's NIC. Under **Settings>DNS Server** we will select "Custom" and insert our DC's "NIC Private IP" found under **Virtual Machines>[your DC name here]>Networking

![alt text](https://github.com/Cham0i/AD-creation-Azure/blob/main/screen/15.png)
 
 ### 3. Adding Client Computers to Domain
 
Lastly, we will RDP into our Client VM using the credentials used during creation. Then we will go to **settings>System>About>

Under "Member of" select "Domain" and enter your domain's name. It will ask you to login, for which the credentials will be for any admin account (the one we made under 1. Account/User Creation Process).

![alt text](https://github.com/Cham0i/AD-creation-Azure/blob/main/screen/19.png)

If you are successfull you will be greeted with this:

![alt text](https://github.com/Cham0i/AD-creation-Azure/blob/main/screen/16.png)

add stuff about allowing all users to RDP
and the following is shown

![alt text](https://github.com/Cham0i/AD-creation-Azure/blob/main/screen/21.png)

![alt text](https://github.com/Cham0i/AD-creation-Azure/blob/main/screen/22.png)

![alt text](https://github.com/Cham0i/AD-creation-Azure/blob/main/screen/23.png)
