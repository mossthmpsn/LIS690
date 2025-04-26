# Omeka Writeup
1. I updated my Ubuntu per usual
2. I created a new user and database in MySQL
	- I first switched to the root MySQL user, using *sudo su* then *mysql -u root*
	- I then created user 'omeka'@'localhost', and created database 'omeka'
	- I then granted all privileges to 'omeka'@'localhost' to user 'omeka'
3. I installed omeka-3.1.2.zip from the site using wget, unzipped the file, and renamed the directory to 'omeka' using mv
4. In the db.ini file, I changed the host name to 'localhost', username to 'omeka', I inserted my password, and changed database name to 'omeka'
	- This is the only place I had a real issue; I put in 'root' as the host name, which caused some errors, but I looked back at the file and fixed the host
5. I used chown -R to switch the ownership of the user and owner to www-data
6. I rebooted Apache and MySQL
7. I set up the site using my link, and went into the admin section and added my first item
