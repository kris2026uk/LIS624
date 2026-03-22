 #Instructions for Downloading and using MySQL

MySQL is an integral part of the LAMP stack. MySQL is where the relational database is created and works together with Apache web server and PHP programming language to enable the database be available on the web.

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

This is an important step! Use the command below to ensure that the Mysql program is secure:

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

Now we need to update the bash shell. To open the bash shell use your text editor and the following command:

**edit ~/.bashrc**

Scroll down and add the following language at the bottom of the file"

"export MYSQL_PS1="[\d]> "

Then save and exit the file.

Now its time to create your *database*!!

Use the following command to login to mysql-- *remember!!* when typing in your password it will appear like it is not working. It is! Type in your password and hit enter and you will get logged in.

**mysql -u USERNAME -p**

Then to bring you the opac database type in these commands, this will pull up :

**show databases;**
**use opacdb;**

Use the following command to create the table for our book database:
It is important to ensure that all the formatting matches below, or the command will not work. This command creates a table with author, title, and copyright year, with an ID auto-created with each new entry, and that ID is the primary key.

**create table books (
        id int unsigned not null auto_increment,
        author varchar(150) not null,
        title varchar(150) not null,
        copyright year not null,
        primary key (id)
);**

To confirm the table was created correctly use the following command:

**show tables;**
**describe books;**

Use the following command to add the author names, book titles, and copyright years to the tables:

**insert into books (author, title, copyright) values
('Jennifer Egan', 'The Candy House', '2022'),
('Imbolo Mbue', 'How Beautiful We Were', '2021'),
('Lydia Millet', 'A Children\'s Bible', '2020'),
('Julia Phillips', 'Disappearing Earth', '2019');**

You can test to confirm the books have been added by using the following command:

**select * from books;**

You can practice making changes to your database by using the following commands. These commands can add/remove data from your database:

**select author from books;**

**select copyright from books;**

**select author, title from books;**

**select author from books where author like '%millet%';**

**select title from books where author like '%mbue%';**

**select author, title from books where title not like '%e';**

**select * from books;**

**alter table books add publisher varchar(75) after title;**

**describe books;**

**update books set publisher='Simon & Schuster' where id='1';**

**update books set publisher='Penguin Random House' where id='2';**

**update books set publisher='W. W. Norton & Company' where id='3';**

**update books set publisher='Knopf' where id='4';**

**select * from books;**

**delete from books where author='Julia Phillips';**

**insert into books
       (author, title, publisher, copyright) values
       ('Emma Donoghue', 'Room', 'Little, Brown & Company', '2010'),
       ('Zadie Smith', 'White Teeth', 'Hamish Hamilton', '2000');**
       
**select * from books;**

**select author, publisher from books where copyright < '2011';**

**select author from books order by copyright;**

**\q**

The last part of our LAMP stack is bringing all the parts together. We do this first by insalling the PHP support for MySQL. Use the following command:

**sudo apt install php-mysql**

Then we must restart both software systems using the following commands:

**sudo systemctl restart apache2**

**sudo systemctl restart mysql**

We now have to create a login in our root directory, so that our PHP can coonnect to MySQL. Use the following commands to create a file in the directory. We also need to change ownership fo the file and its permissions. Apache will beable to read the file but it will not be made public. *this is an important security step!* The *chmod* command changes the files permissions and the *shown* command changes its ownership.

**cd /var/www**

**sudo touch login.php**

**sudo chmod 640 login.php**

**sudo chown :www-data login.php**

**ls -l login.php**

**sudo nano login.php**

These steps only created the file, we must now add our login and database to it so that it will store and use the information. Add the following to the file:

>
<?php // login.php

$db_hostname = "localhost";

$db_database = "opacdb";

$db_username = "USERNAME";

$db_password = "PASSWORD";

?>

>

Now we create a PHP file for our website. Use the following commands to create the files:

**cd /var/www/html**

**sudo edit opac.php**

Copy the following text to add it to your file:

>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MySQL Server Example</title>
</head>
<body>

    <h1>A Basic OPAC</h1>
    <p>We can retrieve all the data from our database and book table using a couple of different queries.</p>

    <?php
    // Load MySQL credentials securely
    require_once '/var/www/login.php';

    // Enable detailed MySQL error reporting
    mysqli_report(MYSQLI_REPORT_ERROR | MYSQLI_REPORT_STRICT);

    // Establish database connection
    $conn = new mysqli($db_hostname, $db_username, $db_password, $db_database);

    if ($conn->connect_error) {
        die("Connection failed: " . $conn->connect_error);
    }

    echo "<h2>Query 1: Retrieving Publisher and Author Data</h2>";

    // Query using prepared statement
    $stmt = $conn->prepare("SELECT publisher, author FROM books");
    $stmt->execute();
    $result = $stmt->get_result();

    while ($row = $result->fetch_assoc()) {
        echo "<p>Publisher " . htmlspecialchars($row["publisher"]) .
             " published a book by " . htmlspecialchars($row["author"]) . ".</p>";
    }

    $stmt->close();

    echo "<h2>Query 2: Retrieving Author, Title, and Date Published Data</h2>";

    $stmt2 = $conn->prepare("SELECT author, title, copyright FROM books");
    $stmt2->execute();
    $result2 = $stmt2->get_result();

    while ($row = $result2->fetch_assoc()) {
        echo "<p>A book by " . htmlspecialchars($row["author"]) .
             " titled <em>" . htmlspecialchars($row["title"]) .
             "</em> was released in " . htmlspecialchars($row["copyright"]) . ".</p>";
    }

    $stmt2->close();
    $conn->close();
    ?>

</body>
</html>
>

Save and exit the file.

The last step is testing and viewing our file!!

Test the file first by using this command:
If you have any errors, they will show after you have added these commands:

**sudo php -f /var/www/login.php**

**sudo php -f /var/www/html/opac.php**

If you have no errors, amazing!! The very last step is testing your webpage:

Using your VM IP address open a browser, adding the /opac.php at the end of it:

**http://34.60.97.144/opac.php**

*SUCCESS!!*





