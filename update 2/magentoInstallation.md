# Magento Installation In centOS- 7

### Step-1. Update Your Machine
`
sudo yum update
`
### Step-2. Install Apache in your Machine

`
yum install httpd
`
### Step-3. To check the apache is running well hit the machine ip in a browser
### To open the port of Apache in Firewall
`
firewall-cmd --permanent --add-service=http
`
### Step-4. Restart the Apache to see the change
`
systemctl restart httpd
`
### Step-5. Make a vhost file in cd/etc/httpd/conf.d/vhost.conf

```
<Directory /var/www/html/example.com/public_html>
    Require all granted
</Directory>

<VirtualHost *:80>
    ServerName example.com
    ServerAlias www.example.com
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html/example.com/public_html

    ErrorLog /var/www/html/example.com/logs/error.log
    CustomLog /var/www/html/example.com/logs/access.log combined

    <Directory /var/www/html/example.com/public_html>
        Options Indexes FollowSymLinks MultiViews
        AllowOverride All
    </Directory>

</VirtualHost>
```

### Step-6. Create folder at cd/var/www/html:
`
mkdir -p example.com/public_html,logs
`
### Step-7. Create file in var/www/html/example.com/logs

`
touch access.log error.log
`
### Step-8. Disable selinux by  
`
vim/etc/selinux/config  and disable the selinux
`
### Step-9. To check the Status of Selinux
`
sestatus
`

### Step-10. Install wget for retrieves content from web:
`
yum install wget
`
### Step-11. Install VIM  for editing file
`
yum install vim
`

# Download and install the MySQL RPM

### Step-1. 
```
1. sudo wget http://dev.mysql.com/get/mysql57-community-release-el7-7.noarch.rpm -P /tmp/

2. sudo yum localinstall /tmp/mysql57-community-release-el7-7.noarch.rpm

3. sudo yum update

4. sudo yum install mysql-community-server

```
### Step-2. Start mysql
`
sudo systemctl start mysqld
`
### Step-3. Change password by using temporary password by grep
`
sudo grep 'temporary password' /var/log/mysqld.log
`
### Step-4. Secure mysql and change the temporary password
`
mysql_secure_installation
`
### Step-5. Check mysql 
`
mysql -u root -p
`
### Step-6 .Create a Magento database and user, and set the permissions. In this example, we’ll call our database and user magento, Replace P@ssword1 with a secure password. You may optionally replace the other values as well:

```
1. CREATE DATABASE magento;
2. CREATE USER 'magento' IDENTIFIED BY 'P@ssword1';
3. GRANT ALL PRIVILEGES ON magento.* TO 'magento';
```
### Step-7. Then exit mysql
`
quit
`
# Install PHP

### Step-1. Setup the Webtatic YUM repo
```
1. sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

2. sudo rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm 
```

### Step-2. Install PHP 7.1 and necessary extensions

`
sudo yum install -y mod_php71w php71w-cli php71w-common php71w-gd php71w-mbstring php71w-mcrypt php71w-mysqlnd php71w-xml php71w-bcmath php71w-intl php71w-soap
`
### Step-3. You Can see the list of all Installation module of php
`
yum list installed *php*
`
### Step-4. Modify the php.ini file in /etc/php.ini
```
max_input_time = 30

memory_limit= 2G

error_reporting = E_COMPILE_ERROR|E_RECOVERABLE_ERROR|E_ERROR|E_CORE_ERROR

error_log = /var/log/php/error.log

date.timezone = America/New_York
```

### Step-5. Create the log directory for php and make the apache of the owner of that folder

```
1. sudo mkdir /var/log/php
2. sudo chown apache /var/log/php
```

### Step-6. Write the domain name of host directory in host folder of windows and linux

> WINDOWS: open cmd then : notepad/drivers/etc/hosts (ip  example.com)

> LINUX: /etc/hosts ((ip  example.com))  [NOT NEEDED ALL THE TIME]


### Step-7. Check the PHP is running or not
`
Go to /var/www/html/example.com/public_html/phpinfo.php and write the below line
`
```
<?php phpinfo(); ?>
```
### Step-8. Define the PHP folder in apache
`
Ln -s phpinfo.php index.php
`

### Step-9. Hit the ip in browser for result

# Magento Install

### Step- 1. Download the magento .tar.gz file and you must create a account in magento website

### Step- 2. If you’re in windows then download winscp Software for transfer the file from windows to linux

### Step- 3. Then move the magento.tar file in var/www/html/example.com/magento Folder

### Setp- 4. Unzip the magento.tar file

`
tar -xvf Magento.tar
`
### Step- 5. Create a user
`
sudo useradd magento
`
### Step-6. add the Magento user to the web server’s user group

`
sudo usermod -g apache magento
`

### Step-7. The commands in this step should be run from your Magento installation directory (where you extracted the archive). If you are not still in that directory, navigate there before proceeding: 

```
1. sudo find var vendor pub/static pub/media app/etc -type f -exec chmod g+w {} \;

2. sudo find var vendor pub/static pub/media app/etc -type d -exec chmod g+ws {} \;

3. sudo chown -R magento:apache .

4. sudo chmod u+x bin/magento
```

### Step-8. Restart Apache
`
systemctl restart httpd
`
### Step-8. Switch to the magento user and navigate to the bin directory in your Magento installation folder
```
sudo su magento

cd bin
```

### Step-9. Run the Magento installation script with the following options

```
./magento setup:install --admin-firstname="John" --admin-lastname="Doe" --admin-email="your@email.com" --admin-user="john" --admin-password="password1" --db-name="magento" --db-host="localhost" --db-user="magento" --db-password="P@ssword1"
```

### Step-10. If the database user don’t have the permission of access then use root user and put the database password.

### Step-11. After the installation successful it’ll show the Success Dialogue.

```
[SUCCESS] Magento Installation Complete
[SUCCESS] Magento Admin URl: /admin_lbhhnx
```















