There are a few more steps to creating our library site. 

We need to download OMEKA to continuing to create our library site. It is important to read the websites download instructions and system 
requirements here before starting the download: https://omeka.org/classic/docs/Installation/System_Requirements/

but first, we need to check the versions of what we have already installed. 
first 
php --version
mysql --version

We also need to confirm there are two php extensions that are required for the OMEKA, (as it states on its system_requirements site) 
mysqli and exif extensions  - to see the php extenstions use this command and verify that what is needed is on the list.

php -m

The last requirement is Image Magick, which we have not yet downloaded, use this command to get this software:

sudo apt install imagemagick

sudo a2enmod rewrite

sudo systemctl restart apache2

The first step is creating a user and database on our system for Omeka, replace omeka and xx with a username and password-
create user 'OMEKA'@'localhost' identified by 'XXXXXXXXX';
create database OMEKA;
grant all privileges on OMEKA.* to 'wordpress'@'localhost';
show databases;
\q

Now! We need to download Omeka--

this is done using these commands:

cd /var/www/html
sudo wget https://github.com/omeka/Omeka/releases/download/v3.2/omeka-3.2.zip  -- this link is found by going to the download page and hovering over the download button, then right click and copy the link
cd /var/www/html
sudo wget https://omeka.org/classic/download/
