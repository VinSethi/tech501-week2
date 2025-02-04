- [Monitoring alert management](#monitoring-alert-management)
    - [Step 1- Loging into VM](#step-1--loging-into-vm)
    - [Step 2- Go onto Azure](#step-2--go-onto-azure)
    - [Step 3 - Selecting the tiles to monitor](#step-3---selecting-the-tiles-to-monitor)
    - [Step 4- Creating a dashboard](#step-4--creating-a-dashboard)
    - [Step 5- Changing the timing of the monitoring of the tiles](#step-5--changing-the-timing-of-the-monitoring-of-the-tiles)
    - [Step 6- Testing the load](#step-6--testing-the-load)
- [Creating Virtual Machine Scale sets](#creating-virtual-machine-scale-sets)
    - [Step 1- Go on to Virtual Machine Scale Set and create a new one](#step-1--go-on-to-virtual-machine-scale-set-and-create-a-new-one)
    - [Step 2- Go to the Disks section](#step-2--go-to-the-disks-section)
    - [Step 3 - Networks section](#step-3---networks-section)
    - [Step 4- Create a load balancer](#step-4--create-a-load-balancer)
    - [Step 5- Advanced file section](#step-5--advanced-file-section)
    - [Step 6- Once the VMSS is created run the frontend ip address in the browser](#step-6--once-the-vmss-is-created-run-the-frontend-ip-address-in-the-browser)
    - [SSH into the scale set](#ssh-into-the-scale-set)
- [Creating an Alert for the VM CPU](#creating-an-alert-for-the-vm-cpu)
    - [Step 1- Create a VM](#step-1--create-a-vm)
    - [Step 2 Create a Dashboard](#step-2-create-a-dashboard)
    - [Step 3- Put some load on the VM](#step-3--put-some-load-on-the-vm)
    - [Step 4- Create an alert rule](#step-4--create-an-alert-rule)
    - [Step 5-Setting up the alert rule](#step-5-setting-up-the-alert-rule)
    - [Step6 - Severity of the message](#step6---severity-of-the-message)
    - [Step 7- Put load on the vm again](#step-7--put-load-on-the-vm-again)
    - [Step 8- Getting the email](#step-8--getting-the-email)

# Monitoring alert management 

### Step 1- Loging into VM    
* SSH into VM and get the app running
### Step 2- Go onto Azure 
1. Move to the monitoring tab
2. Scroll down to the performance and utilisation section

### Step 3 - Selecting the tiles to monitor
1. To select which tiles, click on the pin 
2. Once that is done it will ask you to either select a dashboard or create a new one

### Step 4- Creating a dashboard
1. Type to name the dashboard
2. Set it to public and then press create

### Step 5- Changing the timing of the monitoring of the tiles
1. Click into the tile that you want to change the duration of the file
2. Change it to every 30 minutes and press save

### Step 6- Testing the load
1. Go to the git bash where you are signed into the VM
2. Download the apache util by running `sudo apt-get install apache2-utils`
3. Then run the `ab -n 10000 -c 200 http://20.254.64.46/` - this will create the load 
4. Go back to the tiles and see the different tiles to see the change and load.

# Creating Virtual Machine Scale sets
### Step 1- Go on to Virtual Machine Scale Set and create a new one
* Select autoscaling 
* Also configure towards the bottom to ensure that:
  * - Defauly Instance Count: 2
    - Minimum: 2
    - Maximum: 3
    - CPU threshold: 75%
    - Increase instance count by: 1
![Picture of the first page of creating VMSS](<Screenshot 2025-01-30 160911.png>)


### Step 2- Go to the Disks section
* Put in the information as per usual

### Step 3 - Networks section
* Click on advanced for NIC and change it to the one that was used for the app so it will also allow SSH,HTTP,3000
![Screenshot of network section](<Screenshot 2025-01-30 161052.png>)

### Step 4- Create a load balancer
![Screenshot of creating a load balancer](<Screenshot 2025-01-30 161200.png>)

### Step 5- Advanced file section
* Input the user data with the bash script
![Screenshot of the advanced file section](<Screenshot 2025-01-30 161856.png>)

### Step 6- Once the VMSS is created run the frontend ip address in the browser
 ![Screenshot of the scale set working](<Screenshot 2025-01-30 163746.png>)

 ### SSH into the scale set
 `ssh -i ~/.ssh/tech501-vineet-az-key -p 50000 adminuser@20.254.85.245`
[screenshot of terminal](<Screenshot 2025-01-30 170932.png>)

# Creating an Alert for the VM CPU
### Step 1- Create a VM
* Create the VM as per usual
* Use app image in the vm

### Step 2 Create a Dashboard
* Create a dashboard pinning the CPU into the dashboard

### Step 3- Put some load on the VM
* Put some load on the VM to see the CPU Spike
* Use `sudo apt-get install apache2-utils` and `ab -n 10000 -c 200 http://20.254.64.46/` to create load
![Creating load](<Screenshot 2025-02-04 165008.png>)

### Step 4- Create an alert rule 
* Go onto the VM and into the monitoring section on the left
* Onto the alert tile
* Click on the alert rules 
  ![Screenshot of azure alerts](<Screenshot 2025-02-04 165141.png>)
* Create a new alert rule

### Step 5-Setting up the alert rule
![Setting up the alert rule](<Screenshot 2025-02-04 165342.png>)
* Create an action group next stating email as way of communication

### Step6 - Severity of the message
* Set the severity to warning
![Severity of message](<Screenshot 2025-02-04 165558.png>)
* Put tags in and review and create 

### Step 7- Put load on the vm again

### Step 8- Getting the email
* You should now get the email warning about CPU Threshold 
  ![Screenshot of the email](<Screenshot 2025-02-04 164235.png>)


