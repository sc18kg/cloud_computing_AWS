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

