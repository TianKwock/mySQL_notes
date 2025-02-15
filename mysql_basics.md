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

### Retrieving Data
```
SELECT * FROM {name};  # Selects all columns
SELECT {field} from {name];  # Selects only the specified column
SELECT * FROM {name} WHERE {field} = {value};  # Filers rows where the field matches the value
```
Example:
```
SELECT * FROM Users WHERE id = 1;
```
This will only show me rows where the id = 1. 

More Filtering:
```
SELECT * FROM {name} WHERE {field} = {value} OR {field} = {value};
SELECT {field} from {name} WHERE {field} < {value};  # Retrieves rows meeting the condition
SELECT {field} FROM {name} WHERE NOT {field} = {value};  # Excludes fields with that value
```
Example:
```
SELECT * FROM Users WHERE username = 'Tian' OR username = 'Caleb';
SELECT username from Users WHERE id < 20;
SELECT username FROM Users WHERE NOT id = 2;
```

### Updating & Deleting Data
```
UPDATE {name} SET {field} = {value} WHERE {other field} = {other value};  # Modifies data according to a condition
DELETE FROM {name} WHERE {field} = {value};  # Deletes a specfic row
```
Example:
```
UPDATE Users SET username = 'Tian' WHERE id = 1;  # For rows where the id = 1, make the username 'Tian"
DELETE FROM Users WHERE id = 2;  # Deletes rows with id = 2
```

### Sorting Data
```
SELECT * FROM {name] ORDER BY {field} ASC;  # Orders results in ascending order
SELECT * FROM {name] ORDER BY {field} DESC;  # Orders results in descending order
```

### Altering Tables
```
ALTER TABLE {name} ADD {new field} {DATA_TYPE};  # Adds a new column
UPDATE {name} SET {new field} = {value} WHERE {field} = {value};  # Updates field values
```
Example:
```
ALTER TABLE Users ADD beard BOOLEAN;
UPDATE Users SET beard = FALSE WHERE username = 'Tian';
```
Tian does not have a beard, so set that beard boolean to false. 

# User Management

### Creating & Granting Permissions

Creating an admin:
```
CREATE USER {user}@{machine} IDENTIFIED BY {password};
GRANT ALL PRIVILEGES ON *.* TO {user}@{machine} WITH GRANT OPTION;
FLUSH PRIVILEGES;
```
Breakdown:
- ALL PRIVILEGES - exactly what it sounds like, this user can do anything with the database
- WITH GRANT OPTION - this user can grant the same privileges to other users
- FLUSH PRIVILEGES - command to tell MySQL to reload the grant tables (storing user privileges) so your changes take effect

Example:
```
CREATE USER 'admin'@'localhost' IDENTIFIED BY 'SecurePass123!';
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

# Backing Up & Restoring Databases
```
mysqldump -u {user} -p {name} > backup.sql  # backup the database
mysql -u {user} -p {name} < backup.sql  # restore the database
```
Breakdown:
- mysqldump - command-line utility to create backups of MySQL databases. It will generate a .sql file that contains the SQL statements necessary to recreate the database.
- < -p > - tag to prompt for password of the user.

Example:
```
mysqldump -u root -p Users > backup.sql
mysqldump -u root -p Users < backup.sql
```

# Security Considerations

### Securing MySQL Installation
```
mysql_secure_installation  # Runs security configuration
```
This does the following:
- Set the root password (if current password is not set or weak) 
- Remove anonymous users (MySQL allows anonymous users by default)
- Disable remote root login (local machine only)
- Remove test databse (which anyone can access)
- Reload privilege tables (to apply changes immediately)

### Disabling Remote Root Login
```
UPDATE mysql.user SET Hosts='localhost' WHERE User='root';
FLUSH PRIVILEGES;
```
Breakdown:
- mysql.user - This is a built-in table in MySQL that stores details about all MySQL users, including usernames, authentication methods, privileges, host access, etc.
- SET Host='localhost' - the Host from which the user can connect to MySQL.

### Enforcing Strong Passwords
```
INSTALL PLUGIN validate_password SONAME 'validate_password.so';
```
Breakdown:
- INSTALL PLUGIN - Installs a plugin
- validate_password - name of the plugin that enforces strong password policies
- SONAME 'validate_password.so' - specifies the object file that contains the plugin's implementation.

To check if plugin is installed:
```
SELECT * FROM information_schema.plugins WHERE PLUGIN_NAME = 'validate_password';
```

To uninstall the plugin (WARNING: will remove password requirements)
```
UNINSTALL PLUGIN validate_password;
```

To check current configuration:
```
SHOW VARIABLES LIKE 'validate_password%';
```

#### Changing password policy settings: 

Set Password Policy Level:
```
SET GLOBAL validate_password_policy = '{LEVEL}';  # LOW, MEDIUM, or STRONG
```
LOW - enforces password length
MEDIUM - requires length, uppercase, lowercase, numbers, and special characters by default
STRONG - includes dictionary check 

Change Minimum Password Length:
```
SET GLOBAL validate_password_length = {length];
```

Require at least X uppercase and lowercase letters:
```
SET GLOBAL validate_password_mixed_case_count = {min};
```

Require at least X digits:
```
SET GLOBAL validate_password_number_count = {min};
```

Require at least X special characters:
```
SET GLOBAL validate_password_special_char_count = {min};
```

### Restricting Access to MySQL Directory:
```
sudo chmod -R 750 /var/lib/mysql/
```

### Enabling logging & auditing:
In bash:
```
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```
Add:
```
log_error = /var/log/mysql/error.log
slow_query_log = 1
slow_query_log_file = /var/log/mysql/slow.log
long_query_time = 2
```
Restart the service:
```
sudo systemctl restart mysql
```

# Troubleshooting

Checking service status:
```
systemctl status mysql  # Check if running
sudo systemctl restart mysql  # Restart the service (optional)
```
Checking Open Ports
```
sudo ss -tulpn | grep mysql
``` 
Checking Config File for Errors
```
mysqld --verbose --help | grep -E "Default options|/etc"
```
#### Resetting MySQL Root Password 
Bash:
```
sudo systemctl stop mysql  # Stop the service 
sudo mysqld_safe --skip-grant-tables &  # Disables MySQL authentication, which allows anyone to log in as any user without a password
sudo mysql -u root  # Log in as root
```
SQL:
```
UPDATE mysql.user SET authentication_string=PASSWORD('{new password}') WHERE User='root';
FLUSH PRIVILEGES;
```
Bash:
```
sudo systemctl restart mysql
```
















