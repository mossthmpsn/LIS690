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

