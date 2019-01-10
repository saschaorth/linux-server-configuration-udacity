# Linux server configuration Udacity

## Start a new Ubuntu Linux Server instance on Amazon Lightsail
- Create an AWS account
- Choose **Lightsail** under **Services**
- Click **Create instance** button on the home page
- Select **Linux/Unix** platform
- Select **OS Only** and pick **Ubuntu**
- Select an instance plan
- Name your instance
- Click **Create**

## SSH into your Server
- Download the private key from the **SSH keys** section in the **Account** section on Amazon Lightsail. 
- Create a new file under `~/.ssh` .e.g: `~/.ssh/lighsail.rsa`
- Paste the content of the downloaded file into `~/.ssh/lighsail.rsa` 
with: `cat ~/Downloads/LightsailDefaultKey-eu-central-1.pem > lightsail.rsa`
- Change the the file permissions to: `chmod 600 ~/.ssh/lighsail.rsa`
- Login to the server with: `ssh -i ~/.ssh/lighsail.rsa ubuntu@54.93.126.222`

## Update all currently installed packages

- Upgrade package indexes with: `sudo apt-get update`
- Upgrade all installed packages with `sudo apt-get upgrade`

## Change the SSH port from 22 to 2200. Make sure to configure the Lightsail firewall to allow it.

- Edit file with `nano /etc/ssh/sshd_config` and change `Port 22` to `Port 2200` 
- Restart the SSH service with: `sudo service ssh restart

## Configure the Uncomplicated Firewall (UFW)

- Check firewall status: `sudo ufw status`
- Set default firewall to deny all incomings with: `sudo ufw default deny incoming`
- Set default firewall to allow all outgoings with: `sudo ufw default allow outgoing`
- Allow SSH with:`sudo ufw allow ssh`
- Allow incoming TCP packets on port 2200 to allow SSH with: `sudo ufw allow 2200/tcp`
- Allow incoming TCP packets on port 80 to allow www with: `sudo ufw allow www`
- Allow incoming UDP packets on port 123 to allow NTP with: `sudo ufw allow 123/udp`
- Close port 22 with: `sudo ufw deny 22`
- Enable firewall with: `sudo ufw enable`
- Check out current firewall status with: `sudo ufw status`
- Update the firewall configuration on Amazon Lightsail website under **Networking**. Delete default SSH port 22 and add **port 80, 123, 2200**
- Open up a new terminal and you can now ssh in via the new port 2200 
with: `ssh -i ~/.ssh/lightsail.rsa ubuntu@54.93.126.222 -p 2200`