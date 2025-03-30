# Installing Apache
- web (HTTP) server: software that makes websites available in your browsers
	- can add more complexity than just file access
- elements of a website
	- clients: connects to a server with the specified protocol, and makes a request for a resource using the URL-path
	- server: sends response w/ status code and what you should do w/ the response
	- URL: Uniform Resource Locators - which specify a protocol (e.g. http), a servername (e.g. www.apache.org), a URL-path (e.g. /docs/current/getting-started.html), and possibly a query string (e.g. ?arg=value) used to pass additional arguments to the server
	- hostname: preferably point to DNS
	- config files: HTTP server stored in text files, configuration files in the case of Apache
	- directive: a keyword followed by one or more arguments that set its value
	- static content: things like HTML files, image files, CSS files, and other files that reside in the filesystem
	- dynamic content: anything that is generated at request time, and may change from one request to another
	- log files: best place to start when troubleshooting
- installation
	- first update the machine: sudo apt update
	sudo apt -y upgrade
	- search for apache and pipe the results using head: apt search apache2 | head
	- the specific program is called apache2: apt show apache2
	- install: sudo apt install apache2
- basic checks
	- check if apache is enabled and running: systemctl status apache2
		- look for "enabled" and "active" in the code
-creating a webpage
	- can use either command line web browser or graphical web browser
	- install w3m, a command line browser: sudo apt install w3m
		- for e-links: sudo apt install elinks
	- visit default site using loopback IP address: w3m 127.0.0.1
	- find VM IP address: ip a
	- to use private VM IP address, run: w3m [insert IP address] 
	- for a graphical webpage, go to Compute Engine, VM instances, copy the external IP address and paste
- The default page described above provides the location of the document root at /var/www/html. The document root may reside at a different location on a different Linux operating system, so it's important to verify that location
- rename the document root: cd /var/www/html/
sudo mv index.html index.html.original
sudo nano index.html
- <html>
<head>
<title>My first web page using Apache</title>
</head>
<body>

<h1>Welcome</h1>

<p>Welcome to my web site.
I created this site using the Apache HTTP server.</p>

</body>
</html>

# PHP
- PHP: a server-side programming language (has to be installed on the server--specifically the web server for system/web admins)
- Installing PHP
	- input: sudo apt install php libapache2-mod-php
	sudo systemctl restart apache2
	- confirm the installed version: php -v
	- check status: systemctl status apache2
- Check Install
	- check if it's installed w Apache by checking web doc root: cd /var/www/html/
	sudo nano info.php
	- put this in the file: <?php
	phpinfo();
	?>
	- visit file using: http://[insert public IP address for VM]/info.php
	- delete the site: sudo rm /var/www/html/info.php
- Basic Configurations
	- edit dir.conf file to default to index.php: cd /etc/apache2/mods-enabled/
	sudo cp dir.conf dir.conf.bak
	sudo nano dir.conf
	- change file, putting index.php to the front: DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
	- check configuration: apachectl configtest
	- check status: sudo systemctl reload apache2
	sudo systemctl restart apache2
	systemctl status apache2
- Create an index.php file
	- create and open an index.php file: cd /var/www/html/
	sudo nano index.php
	- create a simple browser detector: <!DOCTYPE html>
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
    if (stripos($user_agent, 'Edge') !== false) {
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
    } elseif (stripos($user_agent, 'Mac') !== false || stripos($user_agent, 'Macintosh') !== false) {
        $os = 'Mac';
    } elseif (stripos($user_agent, 'Linux') !== false) {
        $os = 'Linux';
    } elseif (stripos($user_agent, 'iOS') !== false || stripos($user_agent, 'iPhone') !== false || stripos($user_agent, 'iPad') !== false) {
        $os = 'iOS';
    } elseif (stripos($user_agent, 'Android') !== false) {
        $os = 'Android';
    } else {
        $os = 'Unknown OS';
    }

    // Output Result
    echo "<p>Your browser is <strong>$browser</strong> and your operating system is <strong>$os</strong>.</p>";
    ?>

</body>
</html>
	- save file, visit site: http://[insert public IP address]/


# MySQL

- update machines
- install metapackage for mySQL: sudo apt install mysql-server
- confirm version number: mysql --version
- check if running: systemctl status mysql 
- perform security check: sudo mysql_secure_installation
- follow these to configure: Validate passwords: Y
Password validation policy: 0 (zero) for LOW
Remove anonymous users: Y
Disallow root login remotely: Y
Remove test database and access to it: Y
Reload privilege tables now: Y
- in order to show databases: show databases;
- to quit: \q
- creating and setting up account
	- log back into mysql server: sudo mysql -u root
	- create new user: create user 'opacuser'@'localhost' identified by 'XXXXXXXXX';
	- creating a practice database: mysql> create database opacdb default character set utf8mb4 collate utf8mb4_unicode_ci;
	mysql> show databases;
	mysql> grant all privileges on opacdb.* to 'opacuser'@'localhost' with grant option;
- logging in as user and creating tables
	- make prompt for client more informative: nano .bashrc
	- add this to end of doc: export MYSQL_PS1="[\d]> "
	- save and exit: source ~/.bashrc
	- require password: mysql -u opacuser -p
	- switch to new database: mysql> show databases;
	mysql> use opacdb;
	- creating new example table: mysql> create table books (
        id int unsigned not null auto_increment,
        author varchar(150) not null,
        title varchar(150) not null,
        copyright year(4) not null,
        primary key (ID)
	);
	- confirm table was created: mysql> show tables;
	mysql> describe books;
- adding records into table
	- example: mysql> insert into books (author, title, copyright) values
	('Jennifer Egan', 'The Candy House', '2022'),
	('Imbolo Mbue', 'How Beautiful We Were', '2021'),
	('Lydia Millet', 'A Children\'s Bible', '2020'),
	('Julia Phillips', 'Disappearing Earth', '2019');
	- view all records: mysql> select * from books;
	- in order to view specific aspects: view [---] from books
- installing php and mysql support: sudo apt install php-mysql php-mysqli
	- then restart apache and mysql: sudo systemctl restart apache2
	sudo systemctl restart mysql
	- allow file to be read by apache but nothing else: cd /var/www
	sudo touch login.php
	sudo chmod 640 login.php
	sudo chown :www-data login.php
	ls -l login.php
	sudo nano login.php
	- add credentials: <?php // login.php
	$db_hostname = "localhost";
	$db_database = "opacdb";
	$db_username = "opacuser";
	$db_password = "XXXXXXXXX";
	?>
	- create php file for website: cd /var/www/html
	sudo nano opac.php
	- copy text, save file: <!DOCTYPE html>
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
	- test php syntax: sudo php -f /var/www/login.php
	sudo php -f /var/www/html/opac.php



