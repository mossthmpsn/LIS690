# Introduction

An OPAC is a catalog used within libraries to access and add metadata on the resources within the library. It allows us to access the data put in a database for public access.

# Relational Database Structure

Relational databases break data into separate tables for efficiency, link these tables together to create subcategories, allow this data to be searched through these subcategories, and allow the user to control who gets to access the database. The relational database takes all the information from the OPAC, and puts it through this process in order for the front-end to be more efficient and easily navigable.

# Step-by-Step Setup

1. The PHP insert script is read by the index HTML which allows a connection between the database and the records through MySQL
2. With the connection from the PHP, it allows the records from MySQL to be navigated and added to the database
3. An HTML page is created with search.php, which contains a MySQL query that allows us to navigate a table we created in MySQL
4. These records would need to be stored in MARC and we would need many more tables to store all of the information
5. We need to configure password access, allow override through the password, require authentication; allow configuration files to be read, access to writing data, and static files need to be readable by www-data

# Key Details

Within tables, there are several key values within: the primary key (column/columns in the query), variable-length strings (values that indicate how long the contents of the columns can be), CHECK constraints (a limit on values within either the table or specific columns) and Boolean values.

# Using Documentation

There were no changes I made because I don't feel that I have a good enough grasp to tweak anything. I did reference the different commands when transcribing the code so that I could understand what each command was doing in the PHP. For example, I checked what parameters I was setting, where and how I was connecting, etc. 
