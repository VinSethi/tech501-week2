# Creating an img from DB and App vm

# Step 1
* Create an image by stopping the vm and then going onto capture and image
* Fill in the information

# Step 2
* Once the image is created for the DB and app, go onto the image and create a vm from there

# Step 3 
* Fill in the information as usual 

# Step 4 
* Once the VM is created SSH into it
* Make sure everything workds as it did in the previous VM (reverse proxy, pm2- posts come up)

# Step 5
* Once this is done create a new VM for the app make sure to pic your image from the 'myimage' section
* On the advanced section of the VM input the bash scipt in the user data section.























#!/bin/bash
cd ~/nodejs20-sparta-test-app/app
export DB_HOST=mongodb://10.0.3.4:27017/posts
pm2 start app.js