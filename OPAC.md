# OPAC

## These are instructions for how to create ones own OPAC. An OPAC is an "Online Public Access Catalog".

An OPAC uses the LAMP technologies to create an accessible database that can be searched.

By using these instructions along with the LAMP instructions, one can create their own online accessible database. This can be very useful for a library that may not have the resources available to house their own servers and in-house database.

Our OPAC is going to be created using a MySQL database along with creating an HTML website and using PHP as our search page.

You can view the MySQL github file I created to view the initial download and setting up of our OPAC database "books". These instructions map out how to download MySQL and create a new database

There are many ways to make changes to our current database within MySQL. Databases will have information constantly being updated or removed, and so learning the correct commands is essential to having a working database.

Learning MySQL will only come with practice. 

1. The first thing we want to do is make an update to our database file.

Within our mySQL we want change our database to have a different format of copyright date. 

In mysql use the following commands to ensure you are in the correct database:

-u opacuser -p
use opacdb;
alter table books add publication_date date; 
update books set publication_date = str_to-date(concat(copyright, '-01-01'), '%Y-%m-%d');
alter table books drop column copyright;
alter table books change publication_date copyright date not null;

The above commands added a new column to our table, then made sure it was the correct format, then dropped the original copyright column and replaced it with our new formated column and renamed it.

We can make other additions and changes through mysql to our database. 
You will want to exit out of MySQL before continuing on with our OPAC instructions.

Now that our database is updated! 

2. Second, we want to make our database available and searchable on the web.

The first thing we do to do this is create an HTML page.

Use the following commands to create a new file in the VM:

**cd var/www/html**

**sudo edit mylibrary.html**

Type the following HTML code into your text editor and once your done save and exit. The document below explains what our OPAC is, how one can search it, and the form for it to be created on the website. This file will link to our next search.php file to enable us to search our database!

>
>
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>MySQL Server Example</title>
    </head>
<body>

    <h1>A Basic OPAC</h1>

    <p>In the form below, <b>optionally</b> enter text in the search field.
    Your search query will search by author, title, or publisher.
    Capitalization is usually not necessary on default case-insensitive MySQL collations.
    It's okay to enter partial information, like part of an author's, title's, or publisher's name.</p>

    <p>You can leave the search field empty and only enter dates.
    Regardless, both start and end dates are required for all searches.
    You can use the date fields to limit results, too.
    I added some extra records, which you can view to know what you can query:</p>

    <p><a href="opac.php">OPAC</a></p>

    <p>This is very much a toy, stripped down
    <a href="https://en.wikipedia.org/wiki/Online_public_access_catalog">OPAC</a>.
    The records are basic.
    Not only do they not conform to <a href="https://www.loc.gov/marc/">MARC</a>,
    they don't even conform to something as simple as <a href="https://www.dublincore.org/">Dublin Core</a>.</p>

    <p>I also don't provide options to select different fields, like author, title, or publisher fields.
    Instead the search field below searches key bibliographic fields (author, title, publisher) in our <b>books</b> table.</p>

    <p>The key idea is to get a sense of how an OPAC works, though.</p>

    <h2>My Basic Library OPAC</h2>

    <form method="post" action="search.php">
        <label for="search">Search Terms (optional):</label>
        <input type="text" name="search" id="search">
        
        <br>
        
        <label for="start_date">Start Date:</label>
        <input type="date" name="start_date" id="start_date" required>
        
        <br>
        
        <label for="end_date">End Date:</label>
        <input type="date" name="end_date" id="end_date" required>
        
        <br>
        
        <input type="submit" value="Search">
    </form>

</body>
</html>
>


2. The next step is to create our search.php file.

To create a new file use the following commands:

**cd var/www/html**

**sudo edit search.php**

You will want to type in the following code into this file so that our search site will connect with our database and pull our search results. **NOTE** The file must be typed correctly to work. If not, it will not connect and will not work.

>
>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Search Results</title>
<style>
    table {
        border-collapse: collapse;
        width: 100%;
    }
    th, td {
        border: 1px solid black;
        padding: 8px;
        text-align: left;
    }
</style>
</head>
<body>

    <h1>Search Results</h1>

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

    if ($_SERVER["REQUEST_METHOD"] == "POST") {
        $search = trim($_POST['search']);
        $start_date = $_POST['start_date'];
        $end_date = $_POST['end_date'];

        // Prepared statement to prevent SQL injection
        $stmt = $conn->prepare("SELECT id, author, title, publisher, copyright FROM books 
                                WHERE (author LIKE ? OR title LIKE ? OR publisher LIKE ?) 
                                AND copyright BETWEEN ? AND ?");

        // Use wildcard search
        $search_param = "%$search%";
        $stmt->bind_param("sssss", $search_param, $search_param, $search_param, $start_date, $end_date);
        $stmt->execute();
        $result = $stmt->get_result();

        if ($result->num_rows > 0) {
            echo "<table>";
            echo "<tr><th>ID</th><th>Author</th><th>Title</th><th>Publisher</th><th>Copyright</th></tr>";

            while ($row = $result->fetch_assoc()) {
                echo "<tr>";
                echo "<td>" . htmlspecialchars($row["id"]) . "</td>";
                echo "<td>" . htmlspecialchars($row["author"]) . "</td>";
                echo "<td>" . htmlspecialchars($row["title"]) . "</td>";
                echo "<td>" . htmlspecialchars($row["publisher"]) . "</td>";
                echo "<td>" . htmlspecialchars($row["copyright"]) . "</td>";
                echo "</tr>";
            }

            echo "</table>";
        } else {
            echo "<p>No results found.</p>";
        }

        $stmt->close();
    }

    $conn->close();
    ?>

    <p><a href="mylibrary.html">Return to search page</a></p>

</body>
</html>
>

## **Lastly! Test your work by doing a search!**

http://IPADDRESS/mylibrary.html

If you are able to do a search, and bring up search results, you have successfully created your first OPAC!

