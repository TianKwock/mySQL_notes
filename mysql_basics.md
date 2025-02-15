# General Notes

- DMBS (Database Management System) is a software that manages databases (e.g. MySQL).
- RDBMS (Relational Databse Management System) is a DBMS that uses a relational model (tables, keys, etc).
  - Vs. non-relational (e.g NoSQL)
- While MySQL commands do not have to be in all caps, it's best practice for readibility.
- In this doc, {name} will be a placeholder for database/table names. 
- Useful Resources:
  - https://dev.mysql.com/doc/
  - https://www.w3schools.com/MySQL/default.asp

# Installing & Managing MySQL

### Installation & Service Management (Linux)
```
sudo apt install mysql-server -y  # Installs MySQL
sudo systemctl status mysql  # Checks the MySQL service status
sudo systemctl start mysql  # Starts MySQL service
sudo systemctl enable mysql  # Enables MySQL service (starts on boot)
```
The < -y > tag answers 'yes' to prompts automatically. 

```
sudo mysql  # Opens a MySQL shell as root
```

# Basic MySQL Commands

### Working with Databases
```
SHOW DATABASES;  # List all databses
CREATE DATABASE {name};  # Creates a new database
USE {name};  # Sets the active database 
DROP DATABASE {name};  # Deletes a database
```
To work with a database, you set it as your active database. 

### Working with Tables

Syntax for creating a table: 
```
CREATE TABLE {table_name} (
    {column1_name} {DATA_TYPE} {CONSTRAINTS},
    {column2_name} {DATA_TYPE} {CONSTRAINTS},
    ...
    {columnN_name} {DATA_TYPE} {CONSTRAINTS}
);
```
Breakdown of components:
- CREATE TABLE - command to create a new table
- {table_name} - name of the table
- {column_name} - name of the column
- {DATA_TYPE} - type of data (e.g. INT, VARCHAR(255), DATE)
- {CONSTRAINTS} - Opetional rules for columns (e.g. PRIMARY KEY, NOT NULL, UNIQUE, AUTO_INCREMENT)
- Take note of where commas should be and that there is a semi-colon at the end. 

Example:
```
CREATE TABLE Users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE
);
```
Breakdown:
- AUTO_INCREMENT - ensures that each time a new row is inserted, the id value automatically increases.
- PRIMARY KEY - ensures that the id column must have unique values for each row and cannot contain NULL values.
  - Only one primary key per table.
- VARCHAR(50) - Variable-length string with a max length of 50 characters.
- NOT NULL - The username column cannot have a NULL value, meaning every user must have a username.
- UNIQUE - No two users can have the same username.

```
DESCRIBE {name};  # Displays table structure
```

### Inserting Data
```
INSERT INTO {name} VALUES {values});  # Inserts a row into the table
```
Note that the values must be in order.

Example:
```
INSERT INTO Users VALUES (1, 'Tian');
```
It is generally best practice to specify columns, like so:
```
INSERT INTO Users (id, username) VALUES (2, 'Caleb');
```




