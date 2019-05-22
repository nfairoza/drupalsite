Apache2 config in another repo


#!/bin/bash

sudo su
apt-get update
apt-get install apache2 -y
apt-get install -y php5 libapache2-mod-php5 php5-mysqlnd php5-curl php5-gd php5-intl php-pear php5-imagick php5-imap php5-mcrypt php5-memcache php5-ming php5-ps php5-pspell php5-recode php5-snmp php5-sqlite php5-tidy php5-xmlrpc php5-xsl php5-apcu
a2enmod rewrite ssl
systemctl restart apache2
apache2ctl -M | egrep 'ssl|rewrite'
cd /var/www/html
echo "<?php phpinfo(); ?>" > info.php
rm -f /var/www/html/info.php
apt-get install mysql-server mysql-client -y


cd /etc/apache2/
mkdir ssl
cd ssl/
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/drupalssl.key -out /etc/apache2/ssl/drupalssl.crt
chmod 600 *

mkdir -p /var/www/drupal
cd /etc/apache2/sites-available
vim drupal.conf


<VirtualHost *:80>
                ServerName www.mydrupal.co
                DocumentRoot /var/www/drupal

                # Redirect http to https
                RedirectMatch 301 (.*) https://www.mydrupal.co$1
        </VirtualHost>

        <VirtualHost _default_:443>

                # Server Info
                #ServerName www.mydrupal.co
               # ServerAlias mydrupal.co
               ServerAdmin nfairoza@amazon.com

                # Web root
                DocumentRoot /var/www/drupal

                # Log configuration
                ErrorLog ${APACHE_LOG_DIR}/drupal-error.log
                CustomLog ${APACHE_LOG_DIR}/drupal-access.log combined

                #   Enable/Disable SSL for this virtual host.
                SSLEngine on

                # Self signed SSL Certificate file
                SSLCertificateFile      /etc/apache2/ssl/drupalssl.crt
                SSLCertificateKeyFile /etc/apache2/ssl/drupalssl.key

                <Directory "/var/www/drupal">
                        Options FollowSymLinks
                        AllowOverride All
                        Require all granted
                </Directory>

                <FilesMatch "\.(cgi|shtml|phtml|php)$">
                                SSLOptions +StdEnvVars
                </FilesMatch>
                <Directory /usr/lib/cgi-bin>
                                SSLOptions +StdEnvVars
                </Directory>

                BrowserMatch "MSIE [2-6]" \
                                nokeepalive ssl-unclean-shutdown \
                                downgrade-1.0 force-response-1.0
                # MSIE 7 and newer should be able to use keepalive
                BrowserMatch "MSIE [17-9]" ssl-unclean-shutdown

        </VirtualHost>


apachectl configtest
a2ensite drupal
systemctl restart apache2


# install Drupal

apt-get install git drush -y
cd /var/www/drupal
drush dl drupal-8
cp drupal-8.0.1  .
cd sites/default
cp default.settings.php settings.php
cp default.services.yml services.yml
mkdir files/
chmod a+w *
cd /var/www/
chown -R www-data:www-data drupal/



