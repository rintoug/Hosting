##  Install Magento 2 using Composer on Ubuntu 18.04 with Apache2, MariaDB and PHP 7.1 Support

```
adduser rinto
```

```
usermod -aG sudo rinto
```

```
sudo apt update
sudo apt install apache2
```

```
sudo systemctl stop apache2.service
sudo systemctl start apache2.service
sudo systemctl enable apache2.service
```


Now open Apache configuration file and add the below snippet at the below of the all content. This will enable .htaccess rules required for Magento

sudo nano ```/etc/apache2/sites-available/000-default.conf```
```
<Directory "/var/www/html">
    AllowOverride All
</Directory>
```

Open Apache settings file to set the Global ServerName
```
sudo nano /etc/apache2/apache2.conf
```
Add this line at the end of the file, then save and exit

#### Optional
```
ServerName <server_IP>
```
Check for any errors

```
sudo apache2ctl configtest
```
Enable Apache rewrite

```
sudo a2enmod rewrite
```
Restart Apache for any changes to take effect

```
sudo systemctl restart apache2
```





### Step 2: Install MariaDB Database Server


```
sudo apt-get install mariadb-server mariadb-client

```

```
sudo systemctl stop mariadb.service
sudo systemctl start mariadb.service
sudo systemctl enable mariadb.service
```
After that, run the commands below to secure MariaDB server by creating a root password and disallowing remote root access.

Give some tough password, otherwise you will have poroblems when login with phpmyadmin

```
sudo mysql_secure_installation

```

```
sudo mysql -u root -p
```
### Step 3: Install PHP 7.1 and Related Modules
```
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:ondrej/php
```

```
sudo apt update
```

```
sudo apt install php7.1 libapache2-mod-php7.1 php7.1-common php7.1-gmp php7.1-curl php7.1-soap php7.1-bcmath php7.1-intl php7.1-mbstring php7.1-xmlrpc php7.1-mcrypt php7.1-mysql php7.1-gd php7.1-xml php7.1-cli php7.1-zip
```

Move the index.php as the first in the below file, to serve the PHP as first option

```
sudo nano /etc/apache2/mods-enabled/dir.conf
```

Restart apache for changes to take effect

```
sudo systemctl restart apache2

```




```
sudo nano /etc/php/7.1/apache2/php.ini
```
Chane the below for magento 2

```
file_uploads = On
allow_url_fopen = On
short_open_tag = On
memory_limit = 256M
upload_max_filesize = 100M
max_execution_time = 360
date.timezone = America/Chicago
```


```
sudo systemctl restart apache2.service
```

```
sudo nano /var/www/html/phpinfo.php
```

```
<?php phpinfo( ); ?>
```

http://localhost/phpinfo.php


### Step 4: Install phpmyadmin

```
sudo apt-get install phpmyadmin php7.1-mbstring php7.1-gettext -y
```

```
sudo systemctl restart apache2
```

### How to solve the phpmyadmin not found issue after upgrading php and apache?

Open apache.conf using your favorite editor, mine is nano :)

sudo nano /etc/apache2/apache2.conf

Then add the following line at the end of file:
```
Include /etc/phpmyadmin/apache.conf
```

### can't login as mysql user root from normal user account in ubuntu 18.04

- First, connect in sudo mysql

```
sudo mysql -u root
```
Check your accounts present in your db
```
SELECT User,Host FROM mysql.user;
+------------------+-----------+
| User             | Host      |
+------------------+-----------+
| admin            | localhost |
| debian-sys-maint | localhost |
| magento_user     | localhost |
| mysql.sys        | localhost |
| root             | localhost |
Delete current root@localhost account
```
```
mysql> DROP USER 'root'@'localhost';
Query OK, 0 rows affected (0,00 sec)
```
- Recreate your user
```
mysql> CREATE USER 'root'@'%' IDENTIFIED BY '';
Query OK, 0 rows affected (0,00 sec)
```
- Give permissions to your user (don't forget to flush privileges)
```
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
Query OK, 0 rows affected (0,00 sec)
```
```
mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0,01 sec)
```
- Exit MySQL and try to reconnect without sudo.


### Create a magento user
```
sudo adduser magento
```

```
sudo usermod -g www-data magento
```

```
sudo chown magento:www-data /var/www/html/
```


### Install Composer

```
sudo curl -sS https://getcomposer.org/installer | php
```
Move composer file to the required directory

```
sudo mv composer.phar /usr/local/bin/composer
```




