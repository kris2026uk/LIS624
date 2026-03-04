#PHP

PHP must be installed/ configured to use with APACHE, it is not automatic.
Interacts with Databases.

apt show PHP
sudo apt install php libapache2-mod-php  installs both at the same time
y to yes install 

Once PHP - you must restart by using this command:
sudo systemctl restart apache2

after install -- php -v to verify installation

I have verified that I have version PHP 8.3.6

Command - systemctl status apache2  to verify that apache is running, it says "enabled and running"

THEN
Verify it is runnig by creating a small file to test.
cd /var/www/html/ -- this is our document root, 
after this is pulled command ls to see the current files in this root.

sudo edit info.php - creates a new file using php, with the file name info.

I created the file 
<?php
phpinfo();
?>
saved and exited.
Then using my http://XXX/info.php website, I brought up what I created in the VM and verified it worked.

After verfiying, I deleted the file  using sudo rm /var/www/html/info.php

after deleting the file, I backed out too far of my VM, so i needed to re add my document root.
I could tell i did this when i typed in /var/www/html/ and I kept getting error messages. I needed to be in cd /var/www/html/ in order to pull the directory next.


