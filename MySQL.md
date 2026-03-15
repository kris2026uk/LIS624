 Instructions for Downloading and using MySQL

 For downloading the METAVERSION 
 sudo apt install mysql-server

 To download the policy: apt policy mysql-server
And checking the version of MySQL- mysql --version

Then Check the status of the program- systemctl status mysql 

This is an important step! making it -- sudo mysql_secure_installation
 When requested this is what we want for the secure install:
 Validate passwords: Y
 Password validation policy: 0 (zero) for LOW
 Remove anonymous users: Y
 Disallow root login remotely: Y
 Remove test database and access to it: Y
 Reload privilege tables now: Y

then we need to login to mySQL! using this command - sudo mysql -u root
(the -u specifies the username, we are logging into with the name "root")
When your server shows (mysql>) on your line, you are no longer in your home directory, but you are now in the MySQL server
To see the databases use the 'show databases;' command
To exit MySQL you use the \q command to bring you back to your home directory.

We want to create a user separate from the main root user- so we need to close mysql with \q and then from our home directory use this command again to log back in'sudo mysql -u root'
we then use this command to create the new user 
"mysql> create user 'USERNAME'@'localhost' identified by 'PASSWORD';

use ctrl - l to clear the screen within mySQL.

After creating this new user, we need to create a new database for it and give it permissions
you do so with the following commands within mysql

mysql> create database opacdb default character set utf8mb4 collate utf8mb4_0900_ai_ci;
mysql> show databases;
mysql> grant all privileges on opacdb.* to 'opacuser'@'localhost';

These will allow for our user 'opacuser' to have all privileges in the mysql database when logging in from our home directory. 
If you do not want a user to have all the permissions for mysql, you can limit them by dputting in the follow priviledges:
CREATE
DROP
DELETE
INSERT
SELECT
UPDATE
GRANT OPTION



