We are going to use the LAMP set up that we already have to learn more about relational databases.
First, we must create a new database, which must be done by our root user.

Use the following command to login to mysql:

**sudo mysql -u root**

Now we are going to create our new database within mysql and grant all the priviledges for our 'opacuser' to make changes to our new database.

**create database DinnerDB;**

**grant all privileges on DinnerDB.* to 'opacuser'@'localhost';**

Now we exit mysql as the root user in order to log back in as our 'opacuser' to update our database.

**\q**

Login to mysql with the following commands, then check to ensure our database above was created by :

**sudo mysql -u opacuser**

Within mysql it is important to remember to use **\c** to clear the screen.
This command should bring up all the databases and have them listed:

**show databases;**

 This command is choosing what database to use:
 
**use DinnerDB;**





