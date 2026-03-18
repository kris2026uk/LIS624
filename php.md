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

We want the index.php file to pull up first. In order to do this, we must manually change the order of the files.

1. Verify the documents name and location using the following command:
   **cd /etc/apache2/mods-available/**
   
2. Save a copy of the file dir.conf by using the following command:
   **sudo cp dir.conf dir.conf.bak**

3. Using your editor, open the file:
   **sudo edit dir.conf**

   -insert index.php first in the line of files, save and exit

   Use the following command to check the configuration:
   **sudo systemctl reload apache2**
   **systemctl status apache2**

   *Lastly, we want to create a PHP page*
   Use the following commands to pull up a test editor file, this will automatically name your new file index.php:

   **cd /var/www/html/**
   **Sudo nano index.php**

   Add the following code to the file:

   
 >  <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Browser Detector</title>
</head>
<body>
    <h1>Browser & OS Detection</h1>
    <p>You are using the following browser to view this site:</p>

    <?php
    $user_agent = $_SERVER['HTTP_USER_AGENT'];

    // Browser Detection
    if (stripos($user_agent, 'Edg') !== false || stripos($user_agent, 'Edge') !== false) {
        $browser = 'Microsoft Edge';
    } elseif (stripos($user_agent, 'Firefox') !== false) {
        $browser = 'Mozilla Firefox';
    } elseif (stripos($user_agent, 'Chrome') !== false && stripos($user_agent, 'Chromium') === false) {
        $browser = 'Google Chrome';
    } elseif (stripos($user_agent, 'Chromium') !== false) {
        $browser = 'Chromium';
    } elseif (stripos($user_agent, 'Opera Mini') !== false) {
        $browser = 'Opera Mini';
    } elseif (stripos($user_agent, 'Opera') !== false || stripos($user_agent, 'OPR') !== false) {
        $browser = 'Opera';
    } elseif (stripos($user_agent, 'Safari') !== false && stripos($user_agent, 'Chrome') === false) {
        $browser = 'Safari';
    } else {
        $browser = 'Unknown Browser';
    }

    // OS Detection
    if (stripos($user_agent, 'Windows') !== false) {
        $os = 'Windows';
    } elseif (stripos($user_agent, 'iOS') !== false || stripos($user_agent, 'iPhone') !== false || stripos($user_agent, 'iPad') !== false) {
        $os = 'iOS';
    } elseif (stripos($user_agent, 'Android') !== false) {
        $os = 'Android';
    } elseif (stripos($user_agent, 'Mac') !== false || stripos($user_agent, 'Macintosh') !== false) {
        $os = 'Mac';
    } elseif (stripos($user_agent, 'Linux') !== false) {
        $os = 'Linux';
    } else {
        $os = 'Unknown OS';
    }

    // Output Result
    echo "<p>Your browser is <strong>$browser</strong> and your operating system is <strong>$os</strong>.</p>";
    ?>

</body>
</html>

   
   



