# Linux server configuration Udacity

## Details for reviewer

- Public ip: `18.197.0.52`
- SSH port: `2200`
- Item Catalog URL: `http://ec2-18-197-0-52.eu-central-1.compute.amazonaws.com/catalog/`

## Start a new Ubuntu Linux Server instance on Amazon Lightsail
- Create an AWS account
- Choose **Lightsail** under **Services**
- Click **Create instance** button on the home page
- Select **Linux/Unix** platform
- Select **OS Only** and pick **Ubuntu**
- Select an instance plan
- Name your instance
- Click **Create**

[Source](https://aws.amazon.com/lightsail/?p=tile)

## SSH into your Server
- Download the private key from the **SSH keys** section in the **Account** section on Amazon Lightsail. 
- Create a new file under `~/.ssh` .e.g: `~/.ssh/lighsail.rsa`
- Paste the content of the downloaded file into `~/.ssh/lighsail.rsa` 
with: `cat ~/Downloads/LightsailDefaultKey-eu-central-1.pem > lightsail.rsa`
- Change the file permissions to: `chmod 600 ~/.ssh/lighsail.rsa`
- Login to the server with: `ssh -i ~/.ssh/lighsail.rsa ubuntu@18.197.0.52`

[Source](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-1604)

## Update all currently installed packages

- Run updates with: `sudo apt-get update && sudo apt-get dist-upgrade`

[Source](https://serverfault.com/questions/265410/ubuntu-server-message-says-packages-can-be-updated-but-apt-get-does-not-update)

## Change the SSH port from 22 to 2200

- Edit file with `nano /etc/ssh/sshd_config` and change `Port 22` to `Port 2200` 
- Restart the SSH service with: `sudo service ssh restart`

[Source](https://www.tecmint.com/change-ssh-port-in-linux/)

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
with: `ssh -i ~/.ssh/lightsail.rsa ubuntu@18.197.0.52 -p 2200`

[Source](https://help.ubuntu.com/community/UFW)

## Disable root login

- Go to `/etc/ssh/sshd_config` and change `PermitRootLogin prohibit-password` to `PermitRootLogin no`
Run `sudo service sshd restart`

[Source](https://www.tecmint.com/disable-root-login-in-linux/)

## Create a new user named grader

- Create user with: `sudo adduser grader`

[Source](https://classroom.udacity.com/nanodegrees/nd004/parts/b2de4bd4-ef07-45b1-9f49-0e51e8f1336e/modules/56cf3482-b006-455c-8acd-26b37b6458d2/lessons/4331066009/concepts/48010894680923)

## Give grader the permission to sudo

- Create file to add user to the sudoers with: `touch /etc/sudoers.d/grader`
- Open the file with: `sudo /etc/sudoers.d/grader`
- Add the following line to the file: `ALL=(ALL:ALL) ALL` and save it

[Source](https://classroom.udacity.com/nanodegrees/nd004/parts/b2de4bd4-ef07-45b1-9f49-0e51e8f1336e/modules/56cf3482-b006-455c-8acd-26b37b6458d2/lessons/4331066009/concepts/48010894710923)

## Create ssh keys for grader

- Create ssh keys on you local machine with `ssh-keygen`
- Save the files generated under `~/.ssh`
- Upload your public key (the one that ends with .pub) to your remote server)
- On the remove server run the following commands to activate the public key 
`sudo mkdir /home/grader/.ssh`
`sudo chown grader:grader /home/grader/.ssh`
`sudo chmod 700 /home/grader/.ssh`
`sudo touch /home/grader/.ssh/authorized_keys`
`sudo nano /home/grader/.ssh/authorized_keys` and paste the public key
`sudo chown grader:grader /home/grader/.ssh/authorized_keys`
`sudo chmod 644 /home/grader/.ssh/authorized_keys`

- Enable password authentication by editing: `sudo nano /etc/ssh/sshd_config` and change `PasswordAuthentication yes` to `PasswordAuthentication no`
- Now run `sudo service ssh restart`

[Source](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-1604)

## Configure the local timezone to UTC

- Run `sudo dpkg-reconfigure tzdata`. First select **none of the above**, then select **UTC**

[Source](https://askubuntu.com/questions/138423/how-do-i-change-my-timezone-to-utc-gmt)

## Install and configure Apache to serve a Python mod_wsgi application

- Install Apache with: `sudo apt-get install apache2`
- Check if it worked by going to `http://18.197.0.52`. It worked if you see the **Apache2 Ubuntu Default Page**
- Install mod_wsgi with: `sudo apt-get install libapache2-mod-wsgi python-dev`
- Enable mod_wsgi with: `sudo a2enmod wsgi`
- Restart Apache with: `sudo service apache2 restart`

[Source](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)

## Install and configure PostgreSQL

- Install PostgreSQL with: `sudo apt-get install postgresql`
- Create database user with: `sudo -u postgres createuser -P catalog`
- Create database named **catalog** owned by database user **catalog** with: `sudo -u postgres createdb -O catalog catalog`

[Source](https://help.ubuntu.com/community/PostgreSQL)

## Install and create virtalenv

- Run `pip install virtualenv`
- Go to `/var/www/catalog`
- Run `virtualenv venv`
- Activate virtualenv with `source venv/bin/activate`

[Source](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)

## Install the following dependencies

- Run `pip install python-pip`
- Run `pip install flask`
- Run `pip install httplib2`
- Run `pip install requests`
- Run `pip install oauth2client`
- Run `pip install sqlalchemy`
- Run `pip install psycopg2`

## Install git

- Run `sudo apt-get install git`

## Clone the item-catalog app from Github

- Go to `/var/wwww`
- Create directory with: `sudo mkdir catalog`
- Change owner of the catalog directory with: `sudo chown -R ubuntu:ubuntu catalog/`
- Navigate into the folder and clone item-catalog with: `git clone https://github.com/saschaorth/item-catalog.git`
- Create catalog.wsgi file to serve the application over mod_wsgi and add the following:
```
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, "/var/www/catalog/")

from catalog import app as application
application.secret_key = 'super_secret_key
```
- Rename **item-catalog** to **catalog** with: `mv item-catalog/ catalog`
- Go into catalog and change **application.py** to **__intit__.py** with: `mv application.py __intit__.py`
- Inside **__init__.py** change path for **client_secrets.json** to `'/var/www/catalog/catalog/client_secrets.json'`
- Also change line `app.run(host='0.0.0.0', port=8000)` to `app.run()`
- Update file `'/var/www/catalog/catalog/client_secrets.json'` with host name `http://ec2-18-197-0-52.eu-central-1.compute.amazonaws.com`

[Source](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)

## Setup virtual host

- Create file with: `sudo touch /etc/apache2/sites-available/catalog.conf` and add the following:
```
<VirtualHost *:80>
        ServerName 18.197.0.52
        ServerAdmin admin@18.197.0.52
        WSGIDaemonProcess catalog python-path=/var/www/catalog:/var/www/catalog/venv/lib/python2.7/site-packages  python-home=/var/www/catalog/venv
        WSGIProcessGroup catalog
        WSGIScriptAlias / /var/www/catalog/catalog.wsgi
        <Directory /var/www/catalog/catalog/>
                Require all granted
        </Directory>
        Alias /static /var/www/catalog/catalog/static
        <Directory /var/www/catalog/catalog/static/>
                Require all granted
        </Directory>
        ErrorLog ${APACHE_LOG_DIR}/error.log
        LogLevel warn
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
- Restart Apache with: `sudo service apache2 reload`

##### Sources
- [Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)
- [modwsgi configuration directives](https://code.google.com/archive/p/modwsgi/wikis/ConfigurationDirectives.wiki#WSGIDaemonProcess)

## Disable Apache default page

- Run `sudo a2dissite 000-default.conf`
- Restart Apache with:`sudo service apache2 reload`

[Source](https://www.digitalocean.com/community/questions/how-do-i-remove-apache2-ubuntu-default-page)

## Enable sudo a2ensite catalog.conf

- Run `sudo a2ensite catalog.conf`
- Restart Apache with:`sudo service apache2 reload`

[Source](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)

## Update database path

- Replace `'sqlite:///itemcatalog.db'` in the following files with: `'postgresql://catalog:<password>@localhost/catalog'`
- Files: 
`/var/www/catalog/catalog/models.py`
`/var/www/catalog/catalog/test_items.py`
`/var/www/catalog/catalog/__init__.py`

## Set up database schema

- Run `python models.py`
- Run `python test_items.py`

## Make .git inaccessible

Go to `/var/www/catalog` create file with `sudo nano .htaccess`
Add the following file and save: `RedirectMatch 404 /\.git`

[Source](https://stackoverflow.com/questions/6142437/make-git-directory-web-inaccessible)