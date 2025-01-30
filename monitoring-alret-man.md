# Monitoring alert management 
## Step 1- Loging into VM    
* SSH into VM and get the app running
## Step 2- Go onto Azure 
1. Move to the monitoring tab
2. Scroll down to the performance and utilisation section

## Step 3 - Selecting the tiles to monitor
1. To select which tiles, click on the pin 
2. Once that is done it will ask you to either select a dashboard or create a new one

## Step 4- Creating a dashboard
1. Type to name the dashboard
2. Set it to public and then press create

## Step 5- Changing the timing of the monitoring of the tiles
1. Click into the tile that you want to change the duration of the file
2. Change it to every 30 minutes and press save

## Step 6- Testing the load
1. Go to the git bash where you are signed into the VM
2. Download the apache util by running `sudo apt-get install apache2-utils`
3. Then run the `ab -n 10000 -c 200 http://20.254.64.46/` - this will create the load 
4. Go back to the tiles and see the different tiles to see the change and load.

# Creating Virtual Machine Scale sets
## Step 1- Go on to Virtual Machine Scale Set and create a new one
* Select autoscaling 
* Also configure towards the bottom to ensure that:
  * - Defauly Instance Count: 2
    - Minimum: 2
    - Maximum: 3
    - CPU threshold: 75%
    - Increase instance count by: 1
![Picture of the first page of creating VMSS](<Screenshot 2025-01-30 160911.png>)


## Step 2- Go to the Disks section
* Put in the information as per usual

## Step 3 - Networks section
* Click on advanced for NIC and change it to the one that was used for the app so it will also allow SSH,HTTP,3000
![Screenshot of network section](<Screenshot 2025-01-30 161052.png>)

## Step 4- Create a load balancer
![Screenshot of creating a load balancer](<Screenshot 2025-01-30 161200.png>)

## Step 5- Advanced file section
* Input the user data with the bash script
![Screenshot of the advanced file section](<Screenshot 2025-01-30 161856.png>)

## Step 6- Once the VMSS is created run the frontend ip address in the browser
 ![Screenshot of the scale set working](<Screenshot 2025-01-30 163746.png>)

 ## SSH into the scale set
 `ssh -i ~/.ssh/tech501-vineet-az-key -p 50000 adminuser@20.254.85.245`
[screenshot of terminal](<Screenshot 2025-01-30 170932.png>)