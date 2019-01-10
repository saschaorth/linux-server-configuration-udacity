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