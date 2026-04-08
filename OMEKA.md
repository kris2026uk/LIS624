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


Now! We need to download Omeka--

cd /var/www/html
sudo wget https://omeka.org/classic/download/
