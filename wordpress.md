# Wordpress Write-Up

1. I first updated the machine using *sudo apt update*, then *sudo apt updgrade*
2. I checked the editions for my PHP, MySQL, and my Ubuntu, then restarted PHP and MySQL
3. I switched to the /var/www/html directory, then used wget to download the package for Wordpress
4. This is one of the few times I had an issue with the installation process; I had not installed the unzip program, so I had to find that before I could unzip the package
5. This was also different from other programs we used, because this involved unzipping a .zip file, which unloaded a lot of files into the system, which I hadn't seen before
6. I logged into the MySQL root in order to create a Wordpress database it could access
7. I then created a new user under the name 'wordpress' to log in to the new database
8. I input my username, database name, and password into the *wp-config.php* file after renaming it from *wp-config-sample.php*, along with editing the file to disable FTP uplaods
9. I did not change the name of my site
10. I changed the owner of the Wordpress database to www-data
11. From there, I went to the URL and went through with the install
12. This is another spot where I encountered some issues; I went to upload a new post to the site, and it froze, causing my VM to freeze as well. I was unable to restart it, so I waited until it sorted itself out.
