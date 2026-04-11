# Omeka Installation

Omeka is used for sharing digital collections and is an important step to building our library.   
There are a few more steps to creating our library site. 

We need to download OMEKA to continuing to create our library site. It is important to read the websites download instructions and system requirements here before starting the download:  https://omeka.org/classic/docs/Installation/System_Requirements/

1. First, we need to check the versions of what we have already installed using these commands to ensure they match the requirements for OMEKA, these are listed on their website.

`php --version`

`mysql --version`


2. Second, we also need to confirm there are two php extensions that are required for the OMEKA, (as it states on its system_requirements site) 
mysqli and exif extensions  - to see the php extentions use this command and verify that what is needed is on the list.

`php -m`


3. Third, we have an additional software requirement, ImageMagick, which we have not yet downloaded, use these commands to download and install this software:

`sudo apt install imagemagick`

`sudo a2enmod rewrite`

`sudo systemctl restart apache2`

## Omeka Download

1. The first step is creating a user and database on our system for Omeka.
 This is done within MySQL:
   
  `sudo mysql -u root`

This code is entered within mysql, ensure you replace omeka with your username and XXX with your password.

`create user 'OMEKA'@'localhost' identified by 'XXXXXXXXX';`  
`create database OMEKA;`  
`grant all privileges on OMEKA.* to 'OMEKA'@'localhost';`  
`show databases;`  
`\q`  

2. Second, we need to download Omeka using the following commands:

`cd /var/www/html`  
`sudo wget https://github.com/omeka/Omeka/releases/download/v3.2/omeka-3.2.zip`  

-- this link is found by going to the download page and hovering over the download button, then right click and copy the link

Then you want to unzip the file by using this command:

`sudo unzip omeka-3.2.zip`  

You can now check your VM to confirm it worked:

`ll`

The file should show up with both a zipped version and an unzipped version.

Now we want to save the file with a new name, so that we don't have to remember the version number:

`sudo mv omeka-3.2/ omeka`

`ll` -- Use this command to check to see if the name was changed.

3. Third, for OMEKA to connect correctly to our VM, we want to add our information to the Omeka file"
   
`sudo edit db.ini`

-Host: localhost
-Username: (what you chose for your username above)
-Password: (your password above)
-Database Name : (above)

4. Fourth, we also need to add the database information to our .hdaccess file:

`cd`  
`sudo edit .hdaccess`

add:  

`<Directory /var/www/html/omeka/>  
  Options Indexes FollowSymLinks
  AllowOverride All  
  Require all granted  
  </Directory> `

Now restart apache2 and mysql:

`sudo systemctl apache2 restart`  
`sudo systemctl mysql restart`

You should now be able to pull up the OMEKA login page with your link http://IPADDRESS/omeka/  

From there finish the set up by creating a new login and password for OMEKA site (not the same for what you put in the VM).

**SUCCESS!** **You have download and set up Omeka**
