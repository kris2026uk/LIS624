#PHP

PHP is a server side programming language that must be installed on the server in order to process the content.
PHP must be installed/ configured to use with APACHE, it is not automatic. The program interacts with databases, such as MySQL to create content.

To install the software do the following commands:

- **apt show PHP**
- **sudo apt install php libapache2-mod-php**  This installs both PHP and the required Apache package sat the same time
- y to yes install 

Once PHP is installed you must restart Apache by using this command:
**sudo systemctl restart apache2**

After install use the following command to verify installation:
**php -v**

I have verified that I have version PHP 8.3.6

Use the following command to verify Apache is running, it needs to say "enabled and running"
**systemctl status apache2** 

THEN
We must verify the two software programs are running together by creating a small file to test. 

1. First we bring up our document root:
   
**cd /var/www/html/**

2. Then we need to confirm what current files are in the root with the following:
   
**ls**

3. We need to create a new file with PHP, and the file name info:
   
**sudo edit info.php**   I use the 'edit' file editor, if you use a different one, you would replace the name.

*Create the following file in your editor*

<?php
phpinfo();
?>

Then save and exit. Using my http://XXX/info.php website, bring up the file you created and verified it works.

After verfiying, *ensure you delete* using the following command, this file has important private information about your VM:
**sudo rm /var/www/html/info.php**


Troubleshooting: After deleting the file, I backed out too far of my VM, so i needed to re-add my document root.
I could tell I did this when i typed in /var/www/html/ and I kept getting error messages. I needed to be in cd /var/www/html/ in order to pull the directory next.

I saved a copy of the file dir.conf so that if it got messed up, I could reload it.
Then I went into the file and put the index.php file before the index.html file and deleted the additional index.php. after saving and exiting, the system will now look and load the php index file before the html file.



