# Cloud Computing Repo - AWS

**Creating a AWS VM**

## First log into AWS and navigate to EC2
- Ensure Location is Ireland 
- Click Launch Instance
- Choose Ubuntu Server 16.04 LTS (HVM), SSD Volume Type as the machine
- Keep t2 micro selected for the size unless stated otherwise
## Next Configure Instance Details
 - Number of Instances: You want 1 Instance (unless stated otherwise)
 - Network: be default (vpc)
 - Subnet: DevOps Student default 1a
 - Auto-assign Public IP: Enable
## Next: Add Storage
- Here you can change the size of the VM
## Next: Add Tags
 - Key: Name
 - Value: SRE_kieron_app (keep to naming convention)
## Next: Configure Security Group
 - Select an existing Secuirty group
 - sre_kieron_app ( was the one I created for the app)
 - This had ssh type, on port 22 source: my ip
 - Custom TCP for port 3000 (This is for the app to run on port 3000) source: 0.0.0.0/0
## Review and Launch
- Once launched, run these commands to be able to ssh into the VM using:
```
chmod 400 "the_key.pem"
ssh -i "the_key.pem" "IP_ADDRESS_HERE" 
```
## Now the VM has been created
 - Install the dependencies Step by Step
 ```
 sudo apt-get update -y
sudo apt-get upgrade -y
sudo apt-get install nginx
sudo apt-get install nodejs -y
sudo apt-get install npm -y
sudo apt-get install python-software-properties -y
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
```
## Exit and Transfer the App file over to the VM
```
scp -ri "the_key.pem" "Location of File to Transfer" "IP_ADDRESS_HERE":/route/to/place
```
## Shell into the App and Completed the last steps
```
ssh -i "the_key.pem" "IP_ADDRESS_HERE"
cd /home/ubuntu/app
sudo npm install pm2 -g
sudo npm install
sundo npm start
```
## The app should now be running on the IP available for all.

## Now the Database VM needs to be created
- Ensure Location is Ireland 
- Click Launch Instance
- Choose Ubuntu Server 16.04 LTS (HVM), SSD Volume Type as the machine
- Keep t2 micro selected for the size unless stated otherwise
- 
## Next Configure Instance Details
 - Number of Instances: You want 1 Instance (unless stated otherwise)
 - Network: be default (vpc)
 - Subnet: DevOps Student default 1a
 - Auto-assign Public IP: Enable
## Next: Add Storage
- Here you can change the size of the VM
## Next: Add Tags
 - Key: Name
 - Value: SRE_kieron_db (keep to naming convention)
## Next: Configure Security Group
 - Create a new security group for the db
 - sre_kieron_db ( was the one I created for the app)
 - This had ssh type, on port 22 source: my ip
 - Custom TCP for port 27017 with source: (insert the public IP of the running app)
## Review and Launch
- Once launched, run these commands to be able to ssh into the VM using:
```
ssh -i "the_key.pem" "IP_ADDRESS_HERE_FOR_DB" 
```
## Now the VM has been created
 - Install the dependencies Step by Step
 ```
sudo apt-get update -y
sudo apt-get upgrade -y
wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list
sudo apt-get update
sudo apt-get install -y mongodb-org
sudo systemctl start mongodb
sudo systemctl status mongodb
sudo systemctl enable mongod
```
## Once the DB VM is up and running we need to change the mongo.conf file
```
sudo nano /etc/mongo.conf
ip 127.0.0.1 change to 0.0.0.0 port: 27017
sudo systemctl restart mongod
sudo systemctl enable mongod
sudo systemctl status mongod
```
## Setting up the Reverse proxy on the APP VM is Next
```
ssh -i "the_key.pem" "IP_ADDRESS_HERE_FOR_APP" 
sudo nano /etc/nginx/sites-available/default
location / {
    proxy_pass http://localhost:3000;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade
    proxy_set_header Connection 'upgrade;
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
    
}
sudo nginx -t
sudo systemctl restart nginx
```
## Now this is complete, Lets navigate to the app directory to run the final commands
```
cd /home/ubuntu/app
export DB_HOST=IP_OF_DB_VM:27017/posts
printenv DB_HOST
node seeds/seed.js
npm start
```
## Now the website should be running on the APP IP address with and showing the data on /posts
