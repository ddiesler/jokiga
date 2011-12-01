## Installing Joomla on EC2

###  Create a Linux instance on EC2

In your security group allow access to ssh and http 

### Login to your instance and add your public key
	
	$ ssh -i ~/.ssh/yourname-ec2.pem ec2-user@ec2-107-20-110-250.compute-1.amazonaws.com
	$ echo ssh-rsa AAAA...k= yourname@domain.org >> .ssh/authorized_keys

### Update your Linux system 

	$ sudo yum -y update

### Install MySQL and the Apache, PHP, Git

	$ sudo yum install -y mysqld-server httpd php-mysql git

### Download and unpack joomla 

	$ sudo chown -R ec2-user.apache /var/www/html
	$ wget http://joomlacode.org/gf/download/../Joomla_1.7.3-Stable-Full_Package.zip
	$ unzip Joomla_1.7.3-Stable-Full_Package.zip  -d /var/www/html

### Change the httpd config

	$ sudo vi /etc/httpd/conf/httpd.conf
	...
	User ec2-user
	Group apache

### Add MySQL and Apache to runlevel and start the deamons

	$ sudo chkconfig --level 3 mysqld on
	$ sudo service mysqld restart

	$ sudo chkconfig --level 3 httpd on
	$ sudo service httpd restart

### Put the Joomla installation under version control

	$ cd /var/www/html
	$ git init
	$ git add --all
	$ git config --global user.name "Your Name"
	$ git config --global user.email "yourname@domain.org"
	$ git commit -a -m "Initial joomla import"

### Create a MySQL database 

	$ mysqladmin -u root create jokiga

### Complete the browser based Joomla installation

### Make a database backup

	$ mkdir /var/www/html/mysqldump 
	$ mysqldump -u root jokiga > mysqldump/backup-`date +%Y%m%d_%H%M%S`.sql
	$ git add --all
	$ git commit -a -m "Joomla post install checkpoint"

