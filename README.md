Apache2 config in another repo

```

      

#drupal official installation script
#!/bin/bash

#Install php apache mysql
sudo apt-get install php libapache2-mod-php php-mysql php-xml php-mysql php-curl php-gd php-imagick php-imap php-mcrypt php-recode php-tidy php-xmlrpc -y
sudo apt-get update && sudo apt-get dist-upgrade && sudo sudo apt-get autoremove -y
sudo apt-get install apache2 -y
sudo apt-get install wget git unzip nano -y
sudo apt-get install mysql-client-core-5.7 -y
sudo apt install php7.0-cli -y
sudo apt-get install php-xml -y
sudo apt-get install php7.0-common -y
sudo apt-get install php7.0-gd
sudo apt-get install php7.0-intl
sudo apt-get install php7.0-xsl
sudo apt-get install php-mbstring -y


#start apache
sudo service apache2 restart
sudo a2enmod rewrite
sudo a2enmod env
sudo a2enmod dir
sudo a2enmod mime


#install composer

php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '48e3236262b34d30969dca3c37281b3b4bbe3221bda826ac6a9a62d6444cdb0dcd0615698a5cbe587c3f0fe57a54d8f5') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"

sudo mv composer.phar /usr/local/bin/composer

#Download Drupal core using Composer
composer require alchemy/zippy

composer create-project drupal-composer/drupal-project:8.x-dev drupalsite --no-interaction

composer install --no-dev
sudo service apache2 restart
cd ~
sudo cp drupalsite /var/www/html -r
sudo rm /var/www/html/index.html
sudo chmod -R 755 /var/www/html/*
sudo chown -R www-data:www-data /var/www/html/*




http://54.172.12.231/admin/reports/updates/install



```

make sure you have proper permissions chmod for your site.

the database settings are inside settings.php 

make sure you copy hidden  files (.htaccess)

you can change maz upload size or php confis in .htaccess or php.ini file

enabel manage updates to see install theme button in drupalsite.

check apache settings in apache.config . file and also in the drupal.config or default.config file .

