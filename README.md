# TimescaleDB-infrastructure

## Prerequisites

1. Docker installed on your system

2. Docker compose installed

## Install TimescaleDB container using Docker(Command)

To install the TimescaleDB Docker container, you can use the following command:
```bash
docker run -d \
    --name time1 \
    -p 5432:5432 \
    -v timescale:/var/lib/postgresql/data \
    -e POSTGRES_PASSWORD=pas \
    -e POSTGRES_DB=mydatabase \
    timescale/timescaledb:2.17.1-pg15
```
`Detached Mode:`
    The -d option runs the container in the background, allowing you to continue using the terminal.<br>

`Container Naming:`
    --name time1 gives the container a specific name ("time1") for easier identification.<br>

`Port Mapping:`
    -p 5432:5432 maps port 5432 of the host to port 5432 of the container, enabling access to the PostgreSQL database from outside the container.<br>

`Data Persistence:`
    -v timescale:/var/lib/postgresql/data mounts a volume named "timescale" to store database data persistently, ensuring that data is not lost when the container stops or is removed.<br>

`Environment Variables:`
    -e POSTGRES_PASSWORD=pas sets the password for the default PostgreSQL user.<br>
    -e POSTGRES_DB=mydatabase initializes a new database named "mydatabase" upon container creation.<br>
    
`Image Specification:`
    The command uses the Docker image timescale/timescaledb:2.17.1-pg15, which is a specific version of TimescaleDB based on PostgreSQL 15.<br>

## connect to the container
```bash
 docker exec -it time1 psql -U postgres -p 5432 -d mydatabase
```
## To Show all the databases
```psql
 \l
```

## Create new database using the command
```psql
  create database DataBase_Name;
```
### create database Rami;
```psql
 create database Rami;
```

## Connect to New Database
```psql
 \c Rami
```
## Creating a  table in the Rami Database
```psql
   CREATE TABLE conditions (
    time TIMESTAMPTZ NOT NULL,
    location TEXT NOT NULL,
    device TEXT NOT NULL,
    temperature DOUBLE PRECISION NULL,
    humidity DOUBLE PRECISION NULL
  );
```

### Convert the Table to a Hypertable
### SELECT create_hypertable('conditions', 'time');
### Insert Data Into table
```bash
 INSERT INTO conditions(time, location, device, temperature, humidity) 
VALUES (NOW(), 'office', 'sensor1', 70.0, 50.0);
```
### insert multiple values in to table
```psql
 INSERT INTO conditions(time, location, temperature, humidity)
 VALUES 
    (NOW(), 'office', 70.0, 50.0),
    (NOW() - INTERVAL '1 hour', 'warehouse', 65.0, 55.0),
    (NOW() - INTERVAL '2 hours', 'office', 68.0, 52.0);
```
## View The data in the table
```bash
  select * from Conditions;
```
## output
```psql
            time              | location  | temperature | humidity
 -------------------------------+-----------+-------------+----------
 2024-11-01 07:35:03.202894+00 | office    |          70 |       50
 2024-11-01 07:35:12.415689+00 | office    |          70 |       50
 2024-11-01 06:35:12.415689+00 | warehouse |          65 |       55
 2024-11-01 05:35:12.415689+00 | office    |          68 |       52
```
## Create a User for database
```psql
 CREATE USER user1 WITH PASSWORD 'abc'
```
user1- username<br>
abc- user password

## allow the user to connect to a specific database and perform operations, you can use:
```psql
 GRANT CONNECT ON DATABASE Rami TO user;
```
Rami - database name<br>
user - username
 
## You can grant specific privileges on tables. Common privileges include:
```psql
  SELECT: Allows reading data.
  INSERT: Allows inserting data.
  UPDATE: Allows updating data.
  DELETE: Allows deleting data.
```
## granting read and write access to a table named conditions:
```psql
 GRANT SELECT ON TABLE conditions TO user
```
user- user name

## You can create a TimescaleDB container using either a Docker command or Docker Compose
### Install TimescaleDB container using Docker-compose file
```psql
 Note:
 docker-compose extention will be .yaml (or) .yml
```

## Run the Docker Compose Command:
```psql
  docker-compose up -d
```
 * If you want to run it in detached mode (in the background), use the -d flag:

## connect to the container
```bash
 docker exec -it time1 psql -U postgres -p 5432 -d mydatabase
```
## To stop the services that are running

```psql
   docker-compose stop
```


## To create a TimescaleDB-ha container using following command
```bash
docker run -d \
    --name time1 \
    -p 5432:5432 \
    -v timescale:/var/lib/postgresql/data \
    -e POSTGRES_PASSWORD=pas \
    -v timescale:/var/lib/postgresql/data \
    -e POSTGRES_DB=mydatabase \
    timescale/timescaledb-ha:2.17.1-pg15
```
Do the same process like above


