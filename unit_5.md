  GNU nano 6.2                                                                               unit_5.md                                                                                        
# Relational Databases
- new database: sudo mysql -u root
- create new database w/ privileges for opacuser: mysql> create database DinnerDB;
mysql> grant all privileges on DinnerDB.* to 'opacuser'@'localhost';
- creating a table: use DinnerDB;
create table Meals (
    meal_id int auto_increment primary key,
    meal_name varchar(100) not null,
    cuisine varchar(50),
    cooking_time int not null default 1 check (cooking_time > 0),
    vegetarian boolean
);
- values for future reference
        - meal_id = integer as *primary key*
        - meal_name = *variable-length string* up to 100 characters
        - cuisine = *variable-length string* up to 50 characters
        - cooking_time = integer that uses *check constraint* (if value <0, it will reject)
        - vegetarian = *Boolean* value (T/F)
- for ingredients: create table Ingredients (
    ingredient_id int auto_increment primary key,
    meal_id int,
    ingredient_name varchar(100) not null,
    quantity varchar(50),
    foreign key (meal_id) references Meals(meal_id) on delete cascade
);
- insert data into table: insert into Meals (meal_name, cuisine, cooking_time, vegetarian) values
    ('Spaghetti Bolognese', 'Italian', 45, FALSE),
    ('Vegetable Stir Fry', 'Chinese', 20, TRUE),
    ('Chicken Curry', 'Indian', 50, FALSE),
    ('Mushroom Risotto', 'Italian', 35, TRUE);
- for ingredients: insert into Ingredients (meal_id, ingredient_name, quantity) values
    (1, 'Spaghetti', '200g'),
    (1, 'Ground Beef', '250g'),
    (1, 'Tomato Sauce', '1 cup'),
    (2, 'Broccoli', '100g'),
    (2, 'Carrots', '50g'),
    (2, 'Soy Sauce', '2T'),
    (3, 'Chicken Breast', '300g'),
    (3, 'Curry Powder', '2T'),
    (3, 'Coconut Milk', '1 cup'),
    (4, 'Arborio Rice', '1 cup'),
    (4, 'Mushrooms', '1 cup'),
    (4, 'Parmesan Cheese', '1/2 cup');
- query data: select * from Meals;
- filter for vegetarian: select * from Meals where vegetarian = TRUE;
- in descending or ascending order: select * from Meals order by cooking_time desc;
select * from Meals order by cooking_time asc;
- rename the values: select Meals.meal_name as Meals,
    Ingredients.ingredient_name as Ingredients,
    Ingredients.quantity as Quantity
    from Meals
    join Ingredients on Meals.meal_id = Ingredients.meal_id;
- list ingredients and quantities: select ingredient_name as Ingredients,
    quantity as Quantity
    from Ingredients 
 Ingredients.quantity as Quantity
    from Meals
    join Ingredients on Meals.meal_id = Ingredients.meal_id;
- list ingredients and quantities: select ingredient_name as Ingredients,
    quantity as Quantity
    from Ingredients 
    where meal_id = (select meal_id from Meals where meal_name = 'Chicken Curry');
- provide count of meals by cuisine: select cuisine, count(*) as meal_count
    from Meals
    group by cuisine;
- meals less than 45 min: select meal_name, cooking_time
    from Meals 
    where cooking_time <= 45
    order by cooking_time asc;
- if you want to revoke the privileges of the user: sudo mysql -u root
mysql> show grants for 'opacuser'@'localhost';
mysql> revoke all privileges on DinnerDB.* from 'opacuser'@'localhost';
- track other users: select user, host from mysql.user;
- delete database: mysql> drop database DinnerDB;
- remove user: mysql> drop user 'sean'@'localhost';

# Creating Barebones OPAC
- This OPAC will only be relying on the "books" table from MySQL, which will mimic the way a real OPAC is able to navigate through a much more complex database
- create html page that has a form for entering queries, in the file "mylibrary.html": <!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>MySQL Server Example</title>
    </head>
<body>

    <h1>A Basic OPAC</h1>

    <p>In the form below, <b>optionally</b> enter text in the search field.
    Your search query will search by author, title, or publisher.
    Capitalization is not necessary.
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
    they don't even conform to something as simple as <a href="https://www.dublincore.org/">Dublin Core</a>.

    <p>I also don't provide options to select different fields, like author, title, or publisher fields.
    Instead the search field below searches all the fields (author, title, publisher) in our <b>books</b> table.</p>                                                                        
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

- PHP for the search script: <!DOCTYPE html>
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
    mysqli_report(MYSQLI_REPORT_ERROR | MYSQLI_REPORT_STRICT);<p>The key idea is to get a sense of how an OPAC works, though.</p>

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

- PHP for the search script: <!DOCTYPE html>
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
        $stmt = $conn->prepare("SELECT * FROM books 
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

- add records to the "books" table: mysql -u opacuser -p
insert into books
(author, title, publisher, copyright) values
('Emma Donoghue', 'Room', 'Little, Brown \& Company', '2010'),
('Zadie Smith', 'White Teeth', 'Hamish Hamilton', '2000');

# Creating a Barebones Cataloging Module

- cataloging module is a more user-friendly way to input values into an OPAC
- create index.html, which contains a form for inputting bibliographic data: cd /var/www/html
sudo mkdir cataloging
- use text editor to create file and add content: cd cataloging
sudo nano index.html
- add into index.html: <!DOCTYPE html>
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
        <input type="number" name="copyright" id="copyright" min="1000" max="2300" required>

        <input type="submit" value="Submit">
    </form>
</body>
</html>
- insert.php referenced in the html: <!DOCTYPE html>
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

// Prepare and bind SQL statement
$stmt = $conn->prepare("INSERT INTO books (author, title, publisher, copyright) VALUES (?, ?, ?, ?)");
$stmt->bind_param("ssss", $author, $title, $publisher, $copyright);

// Set parameters and execute statement
$author = $_POST["author"];
$title = $_POST["title"];
$publisher = $_POST["publisher"];
$copyright = $_POST["copyright"];

if ($stmt->execute() === TRUE) {
    echo "New record created successfully";
} else {
    echo "Error: " . $stmt->error;
}

// Close statement and connection
$stmt->close();
$conn->close();
?>

<p><a href='index.html'>Return to Cataloging Page</a></p>
<p><a href='../mylibrary.html'>Return to Library Home Page</a></p>
</body>
</html>

- configure authentication file w/ password: sudo htpasswd -c /etc/apache2/.htpasswd libcat
- use password to control access: sudo nano /etc/apache2/apache2.conf
        - edit this line to "all": AllowOverride None
- change to cataloging directory and create .htaccess: cd /var/www/html/cataloging
sudo nano .htaccess
        - add to .htaccess: AuthType Basic
AuthName "Authorization Required"
AuthUserFile /etc/apache2/.htpasswd
Require valid-user
- check that config file is ok: apachectl configtest
- restart and check status: sudo systemctl restart apache2
systemctl status apache2
- user account is www-data, details stored in /etc/passwd
- implement the following guidelines: Static files (like HTML, CSS, JS) might not need to be writable by the Apache server, so they could be owned by a different user (like your own user ac>
Directories where Apache needs to write data (like upload directories) or applications that need write access should be owned by www-data.
Configuration files (incl. files like login.php) should be readable by www-data but not writable, to prevent unauthorized modifications.
        - change group ownership:  sudo chown :www-data /var/www/html
        - set the setgid bit on /var/www/html:  sudo chmod -R g+s /var/www/html
