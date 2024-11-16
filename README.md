# mysql-infrastructure

## Prerequisites

1. Docker installed on your system

2. Docker compose installed

## Install mysql container using Docker(Command)

To install the mysql Docker container, you can use the following command:
```bash
docker run -d \
    --name my1 \
    -p 3306:3306 \
    -v ram:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=my \
    -e MYSQL_DATABASE=my_database \
    mysql:8.0
```                                                                                                      
#

`docker run -d`:
    This tells Docker to run the container in detached mode (i.e., in the background).

`--name my1`:
    This gives the container a custom name, my1, so you can easily refer to it later instead of using the container ID.

`-p 3306:3306`:
    Maps port 3306 on your host machine to port 3306 on the container, which is the default MySQL port. This allows you to connect to MySQL from the host machine using the default port.

`-v ram:/var/lib/mysql`:
    Creates a named volume ram and mounts it to the MySQL data directory inside the container (/var/lib/mysql). This ensures that MySQL's data is stored persistently in the ram volume.
    If you're trying to store the data in RAM (temporary storage), this volume needs to be created with an appropriate configuration, such as a tmpfs mount.

`-e MYSQL_ROOT_PASSWORD=my`:
    Sets the MYSQL_ROOT_PASSWORD environment variable to my, which is the root user's password for MySQL. This is necessary to secure the database instance.

`-e MYSQL_DATABASE=my_database`:
    This environment variable tells MySQL to create a new database called my_database when the container is initialized.

`mysql:8.0`:
    Specifies that the container will use the official mysql image with version 8.0.
## To access the running Docker container and connect to the pgvector database
### To access the terminal inside your container, you can use the following command

```bash
  docker exec -it my1 bash
```
## Connecting to the MySQL Server Container Locally
```bash
    mysql -u root -p
```
## Here you check the default database created or not
## list all the databases
```mysql
  show databases;
```
## output
```mysql
   +--------------------+
   | Database           |
   +--------------------+
   | information_schema |
   | my_database        |
   | mysql              |
   | performance_schema |
   | sys                |
   +--------------------+
```
 * Here we see the database(my_database)
## Creating a database
```mysql
   create database Database_Name;
```
## Switch to another database using following command
```mysql
  use my_database;
```
## Insert table in my_database Database
## create table
```mysql
  CREATE TABLE Persons (PersonID INT NOT NULL AUTO_INCREMENT,LastName VARCHAR(255),FirstName VARCHAR(255),Address VARCHAR(255),City VARCHAR(255),PRIMARY KEY (PersonID));
```
## Insert data into table
```mysql
   INSERT INTO Persons (PersonID, LastName, FirstName,Address,City) VALUES ('1', 'Reddy', 'Rami','1-210','hyd');<br>
   INSERT INTO Persons (PersonID, LastName, FirstName,Address,City) VALUES ('2', 'Reddy', 'Rami','1-211','hyd');
```
## see the table information
```mysql
   select * from Persons;
```
## output
```mysql
   +----------+----------+-----------+---------+------+
   | PersonID | LastName | FirstName | Address | City |
   +----------+----------+-----------+---------+------+
   |        1 | Reddy    | Rami      | 1-210   | hyd  |
   |        2 | Reddy    | Rami      | 1-211   | hyd  |
```
## Creating users in database using command
```mysql

   create user username@localhost Identified by "Password"
```
## Creating three users
```mysql
  1.create user user1@localhost Identified by "Password"<br>
  2.create user user2@localhost Identified by "Password"<br>
  3.create user user3@localhost Identified by "Password"

```
## list all the users
```mysql
 select user from MySQL.user;
```
## output
```mysql
  +------------------+
  | user             |
  +------------------+
  |
  | root             |
  | user1            |
  | user2            |
  | user3            |
  +------------------+
```
## Grant all the permissions to the user1
```mysql
  GRANT ALL PRIVILEGES ON books.authors  TO 'user1'@'localhost';
```

## You can create a mysql container using either a Docker command or Docker Compose
### Install mysql container using Docker-compose file
#
Note:
docker-compose extention will be .yaml (or) .yml
#
## Run the Docker Compose Command:
```mysql
  docker-compose up -d
```
 * If you want to run it in detached mode (in the background), use the -d flag:

## To access the running Docker container and connect to the pgvector database
### To access the terminal inside your container, you can use the following command
```mysql
  docker exec -it my1 bash
```
## Connecting to the MySQL Server Container Locally
```mysql
    mysql -u root -p
```
* Enter the password of mysql database then you login into database
## To stop the services that are running
```mysql
   docker-compose stop
