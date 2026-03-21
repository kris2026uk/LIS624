 #Instructions for Downloading and using MySQL

MySQL is an integral part of the LAMP stack. It is neccessary for....

The first step is to ensure that the current software is updated, so use the following commands to check for updates and update if neccessary:

**sudo apt update**
**sudo apt upgrade**

 For downloading the METAVERSION use the following command:
 
 **sudo apt install mysql-server**

 Use the following commands to download the policy and see the version of MySQL: 
 
 **apt policy mysql-server**
 
 **mysql --version**

Then Check the status of the program to ensure that is installed and running by using the following command:

**systemctl status mysql** 

This is an important step! Use the command below to ensure that the Mysql program is secure

**sudo mysql_secure_installation**

 When requested these are the commands need to set up the secure install:
 -Validate passwords: Y
 -Password validation policy: 0 (zero) for LOW
 -Remove anonymous users: Y
 -Disallow root login remotely: Y
 -Remove test database and access to it: Y
 -Reload privilege tables now: Y

Next login to mySQL! Using this command:

**sudo mysql -u root**

(The -u specifies the username, we are logging into with the name "root")

When your server shows (mysql>) on your line, you are no longer in your home directory, but you are now in the MySQL server.

To see the databases use the following command:
**show databases**

There should be the following databases listed:

+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.01 sec)

To exit MySQL you use the following command to bring you back to your home directory:
**\q** 

We want to create a user separate from the main root user, so we need to close mysql with \q and then from our home directory use this command again to log back in:
**sudo mysql -u root**

Then use the following command to create the new username and password (you choose the username and password to enter here):

**create user 'USERNAME'@'localhost' identified by 'PASSWORD'** 

Then use the following command to clear the screen within mysql:

**ctrl - l** 

After creating this new user, we need to create a new database for it and give it permissions.
You do so with the following commands within mysql:

**create database opacdb default character set utf8mb4 collate utf8mb4_0900_ai_ci**
**show databases**
**grant all privileges on opacdb.* to 'USERNAME'@'localhost'**

These will allow for our user 'USERNAME' to have all privileges in the mysql database when logging in from our home directory. 
If you do not want a user to have all the permissions for mysql, you can limit them by putting in the follow priviledges:

CREATE
DROP
DELETE
INSERT
SELECT
UPDATE
GRANT OPTION

Now exit out of mysql again using the **\q** command.

