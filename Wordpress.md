# Wordpress Installation

## Wordpress software is how we are going to build our website.

Wordpress is used by many current libraries to house their public facing websites. These websites may pull toghether many different resources and links to resources, and make them available from one place.

1. The first thing needing to be done prior to starting the installation process is to do any updates to your VM using the following commands:

**sudo apt update**

**sudo apt upgrade**

Then it is important to check to see how much space is left on the VM server, since it was set up for 10 G- (mine said I was using 90%). To remove any extra files taking up space on your VM server so run the following commands:

 **sudo apt autoremove** - (this did free up some space on my server, I then had 88%.)

2. Secondly, download the following PHP modules that will be essential to the Wordpress site:
 
 **sudo apt install php-curl php-xml php-imagick php-mbstring php-zip php-intl**
 
 After these are installed, it is important to restart mysql and apache2 use these commands:
 
 **sudo systemctl restart apache2**
 
**sudo systemctl restart mysql**

Now that the VM is updated, its time to download **Wordpress**!

1. First we need to change directories:

**cd /var/www/html**

 We have never installed unzip on this VM so first we need to use this command to install it:
 We will need this to be able to install Wordpress.
 
 **sudo apt install unzip**

2. Now, use the following to download the latest version of Wordpress using this command:
   
**sudo wget https://wordpress.org/latest.zip**

Now unzip the Wordpress file with this command:

**sudo unzip latest.zip**

After unziping the file, delete the latest.zip file -- (I need to do that to clear up any space I can right now.) using this command:

 **sudo rm latest.zip**
 
Now that we have the software downloaded, we want to set up our username and password for Wordpress. **This is really important!** In order to keep our Wordpress site safe, you need to create a strong username and password. We do this through mysql and the root user:

**sudo mysql -u root**

**create user 'wordpress'@'localhost' identified by 'XXXXXXXXX';**

**Now create the database!**

Create the database and grant all the privileges to the user:

1. **create database wordpress;**
2. **grant all privileges on wordpress.* to 'wordpress'@'localhost';**

Confirm the database was created:

3. **show databases;**

then logout
4. **\q**

The last step is making changes to our configuration file, first we need to be in our new wordpress directory. This will add our created database, username and password to the file so that we can run it.

Go to our wordpress directory:

**cd wordpress/**

**cd /var/www/html/wordpress**

1. First copy and save the file as a different name:
   
**sudo cp wp-config-sample.php wp-config.php**

**sudo edit wp-config.php** 

Within this file we need to update the values for **database, username and password** with what we created in the prior step. 

We also need to add the following code to the bottom of the page:

define('FS_METHOD','direct');

Then Save and Exit.

Now bring up wordpress site by using the http:IP ADDRESS/wordpress

**Success!**

When this webaddress comes up, you need to choose a language, a name, password and add your email to login. Once you do that you now have your Wordpress site!
