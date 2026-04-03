The next step in creating our ILS (integrated library system) is creating a cataloging module.
This will give backend users such as catalogers and administrators the ability to update the catalog
without using mySQL coding to add each record.
Creating this cataloging module essentially creates a form for users (catalogers) to fill out,
rather than typing in code into a database. This reduces the potential for coding errors and creates
a more streamlined, faster way to add records.

The first thing we have to do is create a new directory in our VM.
Use the following commands to do this:

cd /var/www/html

sudo mkdir cataloging

Next we use our text editor to create our index.html file within our new directory:

cd cataloging

sudo edit index.html

>
<!DOCTYPE html>
<html>
<head>
    <title>Enter Records</title>
</head>
<body>
    <h1>OPAC Library Administration</h1>

    <p>This is the library administration page for entering records into the OPAC.</p>
    <p>Please do not use this page unless you are an authorized cataloger.</p>

    <form action="insert.php" method="post">
        <label for="author">Author:</label>
        <input type="text" name="author" id="author" required><br><br>

        <label for="title">Book Title:</label>
        <input type="text" name="title" id="title" required><br><br>

        <label for="publisher">Publisher:</label>
        <input type="text" name="publisher" id="publisher" required><br><br>

        <label for="copyright">Copyright:</label>
        <input type="date" name="copyright" id="copyright" required>

        <input type="submit" value="Submit">
    </form>
</body>
</html>
>

Save and exit


Next we needto create another PHP file, similar to what we did with our OPAC, but this form will add 
information into our OPAC.

sudo edit index.html

>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cataloging: Data Entry</title>
</head>
<body>

<h1>Cataloging: Data Entry</h1>

<?php

// Load MySQL credentials
require_once '/var/www/login.php';

// Enable MySQL error reporting
mysqli_report(MYSQLI_REPORT_ERROR | MYSQLI_REPORT_STRICT);

// Establish connection
$conn = new mysqli($db_hostname, $db_username, $db_password, $db_database);
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

if ($_SERVER["REQUEST_METHOD"] === "POST") {
    $author = trim($_POST["author"] ?? "");
    $title = trim($_POST["title"] ?? "");
    $publisher = trim($_POST["publisher"] ?? "");
    $copyright = $_POST["copyright"] ?? "";

    if ($author === "" || $title === "" || $publisher === "" || $copyright === "") {
        echo "All fields are required.";
    } elseif (!preg_match('/^\d{4}-\d{2}-\d{2}$/', $copyright)) {
        echo "Copyright date must use YYYY-MM-DD format.";
    } else {
        // Prepare and bind SQL statement
        $stmt = $conn->prepare("INSERT INTO books (author, title, publisher, copyright) VALUES (?, ?, ?, ?)");
        $stmt->bind_param("ssss", $author, $title, $publisher, $copyright);

        if ($stmt->execute() === TRUE) {
            echo "New record created successfully";
        } else {
            echo "Error: " . $stmt->error;
        }
        $stmt->close();
    }
} else {
    echo "Please submit records using the cataloging form.";
}

// Close connection
$conn->close();
?>

<p><a href='index.html'>Return to Cataloging Page</a></p>
<p><a href='../mylibrary.html'>Return to Library Home Page</a></p>
</body>
</html>
>


We want to ensure that not just anybody can add information into our OPAC, so we need to make 
it secure. We do this by creating a username and password. NOTE when adding the password, the cursor will not move, but your input is being added.

sudo htpasswd -c /etc/apache2/.htpasswd username


We want to update our Apache configuration file to ensure that it requests our username and password before allowing anyone to add anythign to our catalog.

We edit this file through our text editor.

sudo edit /etc/apache2/apache2.conf

Add the following to the current file underneath the existing "Directory/var/www/" section

>
<Directory /var/www/html/cataloging/>
  Options Indexes FollowSymLinks
  AllowOverride AuthConfig
  Require all granted
</Directory>
>

Now we want to change back to our cataloging directory using the following commands and create a new file called .htaccess :

cd /var/www/html/cataloging

sudo edit .htaccess

Type the following information into our file:
>
AuthType Basic
AuthName "Authorization Required"
AuthUserFile /etc/apache2/.htpasswd
Require valid-user
>

 *Save and exit*
 
Next we verify our configuration file works by using the following command:

sudo apachect1 configtest

If we get a message that says Syntax OK  then we must restart Apache2 to ensure everything we have created is working together. Use the following commands to restart:

sudo systemctl restart apache2

sudo systemctl status apache2

The last thing we want to do before starting to use our catalog form is to limit file permissions and ownership to our user account. This is to ensure that only those who need access have access.

First, bring up the current permissions by using the following commands:

grep "www-data" /etc/passwd

www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin

Next we will use this command to change ownership:

sudo chown :www-data /var/www/html

The following command will set the setgid bit to /var/www/html.  This will mean that any file and directory created within /var/www/html will inherit ownership of the parent directory (in this case www-data)

 sudo find /var/www/html -type d -exec chmod g+s {} +


YAY! Time to catalog! 

Using our IP address, we can now open our catalog using this web address:

http://IP_ADDRESS/cataloging/index.html

 or
 our PHP page:
 
 http://IP_ADDRESS/cataloging/insert.php

You should be promted to add your username and password. 
You should get an error message if you add the wrong username or password.

IF it does not work, revisit the section on adding the username and password and restarting Apache2.

Get Cataloging!




