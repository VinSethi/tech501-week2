## For later 

* sudo apt-get update -y 


* sudo apt-get upgrade -y. = Requires user input. Just press tab and enter to go to ok


## Code to deploy database
* sudo apt-get install gnupg curl = Didn't require user input

### Download gpg key
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \             
   --dearmor

### create file list
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list 

### Updating the packages
* sudo apt-get update = to update 

### Had to provide user input
sudo apt-get install -y mongodb-org=7.0.6 mongodb-org-database=7.0.6 mongodb-org-server=7.0.6 mongodb-mongosh mongodb-org-mongos=7.0.6 mongodb-org-tools=7.0.6   =- going to do the install

###
* sudo systemctl enable mongod

### To start MongoDB
* sudo systemctl start mongod

### Configure the bindIP    
* sudo nano /etc/mongod.conf

### restart the mongo db service 
* sudo systemctl restart mongod

### Linking the db to the app vm
* export DB_HOST=mongodb://10.0.3.4:27017/posts


reverse proxy
To reverse proxy:
1. sudo nano /etc/nginx/sites-available/default
2. Then change the "try_files $uri $uri/ =404;" with proxy_pass:https(local host number)
3. Then save and exit and open a new terminal to ssh in and load the posts without the :3000


pm2:
1. globally use = **sudo npm install -g pm2**
2. Check to see if it is installed pm2 --version
3. Then go into the file where the 'app' is
4. Once in the 'app' folder use pm2 start app.js
5. Once it has started, restart the it with  "pm2 restart app"
6. To stop pm2 use this: "pm2 stop app"

![Terminal screenshot for pm2](<Screenshot 2025-01-28 163304.png>)






