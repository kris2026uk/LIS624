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
it secure.
