# cloud_computing_AWS
## Creating a AWS VM

- First log into AWS and navigate to EC2
- Ensure Location is Ireland 
- Click Launch Instance
- Choose Ubuntu Server 16.04 LTS (HVM), SSD Volume Type as the machine
- Keep t2 micro selected for the size unless stated otherwise
- Next Configure Instance Details
 - You want 1 Instace
 - Network to be default (vpc)
 - Subnet : DevOps Student default 1a
 - Auto-assign Public IP : Enable
- Next: Add Storage
- Next: Add Tags
 - Key: Name
 - Value: SRE_kieron_app (keep to naming convention)
- Next: Configure Security Group
 - Select an existing Secuirty group
 - sre_kieron_app ( was the one I created for the app)
 - This had ssh type, on port 22 source: my ip
 - Custom TCP for port 3000 (This is for the app to run on port 3000) source: 0.0.0.0/0
- Review and Launch
