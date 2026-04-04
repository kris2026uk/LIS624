# Wordpress Installation

## Wordpress software is how we are going to build our website.

Wordpress is used by many current libraries to house their public facing websites. These websites bring many different 


The first thing I did prior to starting the installation process was to do any updates to my VM using 
sudo apt update
sudo apt upgrade

Then i checked to see how much space I have left on my server, since we set them up for 10 G- mine said I was using 90%, so I did 

 sudo apt autoremove - this freed up some space on my server, I then had 88%.

 next I downloaded the following PHP modules that will be essential to our wordpress
 
 sudo apt install php-curl php-xml php-imagick php-mbstring php-zip php-intl
 after these were installed, I needed to update mysql and apache2
 
 sudo systemctl restart apache2
 sudo systemctl restart mysql

Now its time to download wordpress!
First we need to change directories :

cd /var/www/html

Now we use the following to download the latest version of wordpress using this command: 

sudo wget https://wordpress.org/latest.zip

 We have never installed unzip on this vm so first we need to use this command to install it:
 sudo apt install unzip
 now you can unzip the wordpress file with this command:
sudo unzip latest.zip

after unziping the file, we can delete the latest.zip file -- i need to do that to clear up any space I can right now.
using this command:

Now, we want to set up our username and password for wordpress. We do this through mysql and the root user 
sudo mysql -u root

create user 'wordpress'@'localhost' identified by 'XXXXXXXXX';

Create a database:

create database wordpress;
grant all privileges on wordpress.* to 'wordpress'@'localhost';
confirm your database was created
show databases;
logout
\q



sudo rm latest.zip

Now we want to make changes to our configuration file, first we need to be in our new wordpress directory

cd wordpress/
ls

cd /var/www/html/wordpress

First we copy and save our file as a different name:
sudo cp wp-config-sample.php wp-config.php

sudo edit wp-config.php
Within this file we need to update the database, username and password. 

We also need to add the following to the bottom of the page:

define('FS_METHOD','direct');

Now I brought up my wordpress site by using my IP ADDRESS/wordpress
When this webaddress comes up, you need to choose a language, a name, password and add your email to login.
