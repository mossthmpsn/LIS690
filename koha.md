# Koha Installation
1. I first put in place a new VM under the name koha-main
	- series e2 medium 2vCPU 4 GB memory Ubuntu 22.04 20 GB disk size
	- allowed HTTP traffic, w/ network tag koha-8080
2. I then put a firewall rule in place
	- named koha-opac, target tag koha-8080, 0.0.0.0/0 in source IPv4 ranges, TCP port 8080
3. Updated Ubuntu using update and upgrade, used *apt remove* and *apt clean* to remove unnecessary files and packages
4. Installed *gnupg2* and *apt-transport-https*, then rebooted system
5. Logged into root user using *sudo su*
6. Added Koha repo to server, along with installing the GPG key in the *trusted.gpg.d* directory
	- Inspecting the key didn't work a couple times, but I input the previous commands again and it worked after a few tries
7. Updated to connect to repo, checked package info, then installed *koha-common*
8. Created backup of *koha-sites.conf* within the /etc/koha directory
9. Edited koha-sites.conf file, changing INTRAPORT 80 to INTRAPORT 8080
10. Installed *mysql-server*, set password, and enabled *rewrite* and *cgi* functionality within Apache2 using a2enmod
11. Created database for Koha using *koha-create --create-db bibliolib*
12. Edited *ports.conf*, adding "Listen 8080" after "Listen 80"
	- restart Apache
13. Disable Apache2 default setup
	- a2dissite 000-default
14. Enable traffic compression
	- a2enmod deflate
15. Enable bibliolib site
	- a2ensite bibliolib
16. Reload and restart Apache2
17. Find admin login through this file *nano /etc/koha/sites/bibliolib/koha-conf.xml*
	- This was another issue I faced; I accidentally added text to this file, causing the configuration to fail
18. Finished installation at http://35.193.186.85:80


