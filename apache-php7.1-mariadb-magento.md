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





