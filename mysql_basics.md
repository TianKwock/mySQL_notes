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
sudo apt install mysql-server -y
sudo systemctl status mysql
sudo systemctl start mysql
sudo systemctl enable mysql
```
These lines do the following:
- Installs MySQL
- Checks the MySQL service status
- Starts MySQL service
- Enables MySQL service (starts on boot)

```
sudo mysql
```
This will open a MySQL as root

# Basic MySQL Commands

### Working with Databases
```
SHOW DATABASES; # List all databses
CREATE DATABASE {name};
USE {name};
DROP DATABASE {name};
```
- List all databases
- 



