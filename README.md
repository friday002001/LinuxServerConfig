## Server Configuration

- IP ADDRESS: 35.165.241.166
- SSH Port: 2200
- URL: http://ec2-35-165-241-166.us-west-2.compute.amazonaws.com/

## Summary of Software and Config. Changes Made

### Dev Environment & Initial SSH Key

1. Obtained Private Key and move files to .ssh folder 
	``mv ~/Downloads/udacity_key.rsa ~/.ssh/``
2. In terminal set file access 
	``chmod 600 ~/.ssh/udacity_key.rsa``
3. SSH into your new instance
	``ssh -i ~/.ssh/udacity_key.rsa root@<IP ADDRESS>``

### Creating new user and permissions

1. Create new grader user
	``sudo adduser grader``
2. Grant User Sudo Priveleges
	* ``sudo visudo``
	* Add grader line under root.
``# User privilege specification``
``root    ALL=(ALL:ALL) ALL``
``grader  ALL=(ALL:ALL) ALL``

### Updating Packages
1. Run ``sudo apt-get update``
2. next ``sudo apt-get upgrade``

### SSH Configuration, new SSH Key
1. Run ``sudo nano /etc/ssh/sshd_config``
2. At the very top change port info
``# What ports, IPs and protocols we listen for``
``Port 2200``
3. Disable remote login of root user changing ``PermitRootLogin without-password`` to ``PermitRootLogin no``
4. Create SSH key on local machine ``ssh-keygen`` which will ask where to save too.
5. On Server create .ssh directory ``mkdir .ssh``
6. Go back to local machine read out contents of key file ``cat .ssh/<file name>``
7. Copy Key then go back to remote server and paste into file ``.ssh/authorized_keys``

### Config UFW (Firewall)
* Run the following to setup the firewall
``sudo ufw default allow outgoing``
``sudo ufw default deny incoming``
``sudo ufw allow 2200``
``sudo ufw allow www``
``sudo ufw allow ntp``

## Apache Install
* ``sudo apt-get install apache2``
* Used the following guide for the remainder [Udacity Blog](http://blog.udacity.com/2015/03/step-by-step-guide-install-lamp-linux-apache-mysql-python-ubuntu.html)

## Flask setup
* Used the following guide [DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)

## PostgreSQL Setup
* Used the following guide [DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps)

## Application DB Setup
* Had to modify
	``engine = create_engine('postgresql://catalog:<password>@localhost/catalog')``
* Install sqlalchemy library
	``sudo apt-get install python-sqlalchemy``
* Run database_setup.py
	``python database_setup.py``
	``python catalog_setup.py``

## OAuth Redirect URI's
* In my project I only used FB OAuth so I allowed the following URIs
	``http://ec2-35-165-241-166.us-west-2.compute.amazonaws.com/``
	``http://35.165.241.166/``


# Issues
1) Facebook OAuth issues.  Had to modify the json calls for the full path.  for example
	``json.loads(open(r'/var/www/catalog/catalog/fb_client_secrets.json', 'r')``


# Resources Used

Resources Used: [P5 How I got through it](https://discussions.udacity.com/t/p5-how-i-got-through-it/15342)
