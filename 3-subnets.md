# Creating a 3 subnet Arichtecture
## Planning the 3 subnet Architecture
![Screenshot of the planning](<Screenshot 2025-01-31 163527.png>)
* States that there is 3 subnets ( Public, Private, DMZ)
* The public Id is eliminated from the Database VM for more security which will not allow you to SSH into the VM
* You can only SSH into app vm and NVA VM- to set up everything for now

## Public subnet - App
* Have a public subnet for this, same as created before
* A link to the database will be created to allow to go into the Database from the app, however a DMZ subnet created to filter
* Create the virtual machine as per normal- make sure to select the 3 subnet option when selecting subnet
* In the user data field change the ip address to the up to date one for the database

## DMZ Subnet- NVA VM
* NVA VM- Network virtual appliance- this is used to filter the traffic to the database
* This now becomes an id of 10.0.3.0
  
## Private subnet -Database
* This now becomes an id of 10.0.4.0
* Public id removed to be able to SSH straight into the Database- to ensure more security
* For the database Enable 

# Steps to create the 3 subnet Architecture 
## Step 1 - Create the Virtual network 
* Name the virtual network "tech501-vineet-3-subnet-vnet"
* Make sure you create a new one everytime for a new project
* Create a public subnet with a starting address of 10.0.2.0
* Create the DMZ subnet with a starting address of 10.0.3.0
* Create a private subnet with a starting address of 10.0.4.0

## Step 2 - Create the Database Virtual Machine
* Keep the settings the same as per usual, change to the ones that are below
* Name the VM "tech501-vineet-in-3-subnet-sparta-app-db"
* Select availability zone 3
* On networking tab, make sure to select the 3 subnet network
* Select the private subnet
* Change public IP to 'none'
* ![Screenshot of creating the DB VM](<Screenshot 2025-02-03 105447.png>)

## Step 3- Create the App VM
* Keep the setting the same as per usual, change to the ones that are below
* Change the name to 'tech501-vineet-in-3-subnet-sparta-app
* On networking tab select the 3 subnet network
* Select the public subnet 
* On advanced tab input the user data- change the export method to the ip address of the sparta app db
![Screenshot of creating an app vm](<Screenshot 2025-02-03 110517.png>)

## Step 4- Checking if the App VM talking to DB VM
* SSH into the App vm 
* Input `ping 10.0.4.4` to talk to the DB VM- allows you to keep a track of things and see if it works

## Step 5- Create the NVA VM
* Name it tech501-vineet-in-3-subnet-sparta-app-nva
* Change zone option to 2
* Make sure to select the 3 subnet network
* Make sure to select the DMZ subnet

## Step 6- Creating the route tables
1. Type in 'route tables' in Azure
2. Create a new route table
3. Name it tech501-vineet-to-private-subnet-rt
4. Go to setting inside it and onto subnets
5. Press on the associate 
6. Select the 3 subnet vnet
7. Select the public subnet
8. Now go to the routes tiles
9. Add a route and name it 'to-private-subnet-route'
10. Destination type 'IP addresses'
11. Next hop type to 'Virtual Appliance'
12. Next hop address to 10.0.3.4

## Step 7- Enabling Ip forwarding 
* Go to the settings tab on the NVA VM and tick the enable IP forwarding

## Step 8- SSH into the NVA VM
1. SSH into the NVA VM
2. Input this code `sysctl net.ipv4.ip_forward` to see if ip forward is enabled in Linux
3. Then nano using this code `sudo nano /etc/sysctl.conf` 
4. Then uncomment out `net.ipv4.conf.all.forwarding` and then save and exit
5. Input `sudo sysctl -p`
6. Then nano using this code `nano congig-ip-tables.sh`
7. Copy this code into the nano file:
   #!/bin/bash
```
#configure iptables
 
echo "Configuring iptables..."
 
#ADD COMMENT ABOUT WHAT THE FOLLOWING COMMAND(S) DO
sudo iptables -A INPUT -i lo -j ACCEPT
sudo iptables -A OUTPUT -o lo -j ACCEPT
 
#ADD COMMENT ABOUT WHAT THE FOLLOWING COMMAND(S) DO
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
 
#ADD COMMENT ABOUT WHAT THE FOLLOWING COMMAND(S) DO
sudo iptables -A OUTPUT -m state --state ESTABLISHED -j ACCEPT
 
#ADD COMMENT ABOUT WHAT THE FOLLOWING COMMAND(S) DO
sudo iptables -A INPUT -m state --state INVALID -j DROP
 
#ADD COMMENT ABOUT WHAT THE FOLLOWING COMMAND(S) DO
sudo iptables -A INPUT -p tcp --dport 22 -m state --state NEW,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -p tcp --sport 22 -m state --state ESTABLISHED -j ACCEPT
 
#uncomment the following lines if want allow SSH into NVA only through the public subnet (app VM as a jumpbox)
#this must be done once the NVA's public IP address is removed
#sudo iptables -A INPUT -p tcp -s 10.0.2.0/24 --dport 22 -m state --state NEW,ESTABLISHED -j ACCEPT
#sudo iptables -A OUTPUT -p tcp --sport 22 -m state --state ESTABLISHED -j ACCEPT
 
#uncomment the following lines if want allow SSH to other servers using the NVA as a jumpbox
#if need to make outgoing SSH connections with other servers from NVA
#sudo iptables -A OUTPUT -p tcp --dport 22 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
#sudo iptables -A INPUT -p tcp --sport 22 -m conntrack --ctstate ESTABLISHED -j ACCEPT
 
#ADD COMMENT ABOUT WHAT THE FOLLOWING COMMAND(S) DO
sudo iptables -A FORWARD -p tcp -s 10.0.2.0/24 -d 10.0.4.0/24 --destination-port 27017 -m tcp -j ACCEPT
 
#ADD COMMENT ABOUT WHAT THE FOLLOWING COMMAND(S) DO
sudo iptables -A FORWARD -p icmp -s 10.0.2.0/24 -d 10.0.4.0/24 -m state --state NEW,ESTABLISHED -j ACCEPT
 
#ADD COMMENT ABOUT WHAT THE FOLLOWING COMMAND(S) DO
sudo iptables -P INPUT DROP
 
#ADD COMMENT ABOUT WHAT THE FOLLOWING COMMAND(S) DO
sudo iptables -P FORWARD DROP
 
echo "Done!"
echo ""
 
#make iptables rules persistent
#it will ask for user input by default
 
echo "Make iptables rules persistent..."
sudo DEBIAN_FRONTEND=noninteractive apt install iptables-persistent -y
echo "Done!"
echo ""
``` 
8. Run `./config-ip-tables.sh` - this will now filter the traffic to DB VM 
9. Post page should still now be working

## Step 9- Creating stricter rules in DB VM
1. Go into the DB VM on Azure
2. Go onto settings and inbound secuirty rules
3. Click on 'Add'
4. Change the source to 'Ip address'
5. Change source Ip address to 10.0.2.0/24
6. Change the service to 'MongoDB' and press add 
7. Now press add again to add another
8. Change destination port ranges to '*'
9. Change priority to '1000'
10. Post page should still be working
11. The pinging will now stop as the traffic is now filtered