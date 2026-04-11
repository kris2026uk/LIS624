# Omeka Installation

Omeka is used for sharing digital collections and is an important step to building our library.   
There are a few more steps to creating our library site. 

We need to download OMEKA to continuing to create our library site. It is important to read the websites download instructions and system requirements here before starting the download:  https://omeka.org/classic/docs/Installation/System_Requirements/

1. First, we need to check the versions of what we have already installed using these commands to ensure they match the requirements for OMEKA, these are listed on their website.

`php --version`
`mysql --version`


We also need to confirm there are two php extensions that are required for the OMEKA, (as it states on its system_requirements site) 
mysqli and exif extensions  - to see the php extentions use this command and verify that what is needed is on the list.

'''
php -m
'''

The last requirement is ImageMagick, which we have not yet downloaded, use this command to get this software:

'''
sudo apt install imagemagick

sudo a2enmod rewrite

sudo systemctl restart apache2
'''

The first step is creating a user and database on our system for Omeka, replace omeka and xx with a username and password-
create user 'OMEKA'@'localhost' identified by 'XXXXXXXXX';
create database OMEKA;
grant all privileges on OMEKA.* to 'OMEKA'@'localhost';
show databases;
\q

Now! We need to download Omeka--

this is done using these commands:

cd /var/www/html
sudo wget https://github.com/omeka/Omeka/releases/download/v3.2/omeka-3.2.zip  -- this link is found by going to the download page and hovering over the download button, then right click and copy the link

Then you want to unzip the file by using this command:
sudo unzip omeka-3.2.zip

You can now check your VM to confirm it worked.
The file should show up with both a zipped version and an unzipped version.

Now we want to save the file with a new name, so that we don't have to remember the version number:
sudo mv omeka-3.2/ omeka

ll 
to confirm the change was made.

now we want to add our information to the Omeka file 
sudo edit db.ini

Host: localhost 
Username: (what you chose for your username above)
Password: (your password above)
Database Name : (above)

We also need to add the database information to our .hdaccess file
cd
sudo edit .hdaccess

add:
<Directory /var/www/html/omeka/>
  Options Indexes FollowSymLinks
  AllowOverride All
  Require all granted
</Directory>

now restart apache2 and mysql

you should now be able to pull up the OMEKA login page with your link http://XXXX/omeka/ 
from there finish the set up -create a new login and password for OMEKA site
