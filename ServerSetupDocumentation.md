# What is a LAMP stack
- A LAMP stack is a stack used for web development (specifically for databases), here consisting of Apache, PHP, MySQL, and our OS, Linux. Apache is used for an HTTP server, PHP is our programming language used within the server, and MySQL is our database.  

# Installation Steps

- Update systems before each installation: sudo apt update
	sudo apt upgrade
	sudo apt autoremove
	sudo apt clean

- Apache

	- Use apt search to find the right version, piping the results: apt search apache2 | head
	- Locate the desired packages: apt show apache2
	- Install: sudo apt install apache2
- PHP
	- Install PHP and libapache2-mod-php: sudo apt install php libapache2-mod-php
	- Connect PHP to Apache server: sudo systemctl restart apache2
	- Confirm installed version: php -v
	- Check status: systemctl status apache2
- MySQL
	- Install MySQL Community Server package:  sudo apt install mysql-server
	- Confirm version number: mysql --version
	- If incorrect, confirm versions available: apt policy mysql-server
	- Check if running and enabled: systemctl status mysql 
	- Run security checks and ensure secure configuration: sudo mysql_secure_installation
		- Yes for all prompts, password validation poliy at 0

# Configuration Changes

- Apache
	- Installed w3m for the command line browser
- PHP
	- Default Apache to the file index.php by editing the dir.conf file (our Directory Index)
- MySQL
	- Created new user
	- Created new database "opacdb"
	- Granted full access to new user

# Verifying Components

- Apache
	- Ran w3m 10.128.0.5 to verify if apache2 is running on my VM
	- Ran systemctl status apache2 before using PHP to check if it is running
- PHP
	- Created file info.php within /var/www/html to create website with system information under my VM's public IP address
- MySQL
	- Created a table titled "books" with author, title, copyright year information
	- Installed PHP support for MySQL
	- Authenticated PHP support by creating login.php file within directory /var/www
	- Created PHP file for website above
	- Tested PHP symtax to see if HTML is outputted

# Challenges

- My main challenge was the fact that I was using the wrong version of Ubuntu. I also had issues accessing the HTTP simply because I didn't understand the directions. Otherwise, everything ran fairly smoothly.


