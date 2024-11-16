## PgVecotr-infrastructure

## Check Docker is Installed in your system or not
```bash
docker --version
```
## If docker is not installed install the docker using below
### For Windows

- [Docker](https://docs.docker.com/desktop/install/windows-install/)

## For ubuntu
```bash
sudo snap install docker
```

## Install PgVector container using Docker(Command)

To install the pgvector extension in a PostgreSQL Docker container, you can use the following command:
```bash
sudo docker run \
  --name pgvector \
  -e POSTGRES_PASSWORD=pg \
  -e POSTGRES_DB=data \
  -v pgvect:/var/lib/postgresql/data \
  -p 5432:5432 \
  -d pgvector/pgvector:pg17 
```

- Replace Password with the required password

docker run: This is the command that tells Docker to create and start a new container.
--name pgvector: This option gives the container a specific name, "pgvector," making it easier to manage later (like stopping or removing it).<br>-e POSTGRES_PASSWORD=pg: This sets an environment variable inside the container that defines the password for the PostgreSQL user. Here, it is set to "pg".<br>
-e POSTGRES_DB=data: This option creates a new database named "data" when the PostgreSQL server starts.<br>
-v pgvect:/var/lib/postgresql/data: This mounts a volume named "pgvect" to the container. It allows PostgreSQL data to persist even if the container is stopped or removed. The data will be stored on your host machine.<br>
-p 5432:5432: This maps port 5432 on your local machine (the host) to port 5432 on the container. It allows you to connect to PostgreSQL from your local machine using this port.<br>
-d pgvector/pgvector:pg17: The -d flag runs the container in detached mode, meaning it runs in the background. The last part specifies the Docker image to use, which is pgvector/pgvector with the tag pg17, indicating it's based on PostgreSQL version 17.

## To access the running Docker container and connect to the pgvector database

```
   docker exec -it pgvector psql -U postgres -h 192.168.0.104 -p 5432 -d data
```
Replace 192.168.0.104 with your local host<br>
Enter the password of pgvector database then you login into database

## After login into pgvector database create a Extention
```
  CREATE EXTENSION vector;
```
The command CREATE EXTENSION vector; is used to enable the pgvector extension in PostgreSQL

## create a database in pgvector
```
  create database ram;
```
## list all the databases
```
   \l
```
## switch to another database
```
   \c ram(database-Name);
```
## Insert table in ram Database
### Create a table in a ram database using the command
```
    CREATE TABLE embeddings (
    id SERIAL PRIMARY KEY,
    vector VECTOR(3) 
   );
```
## Insert data into table by using following command
``` 
  INSERT INTO embeddings (vector) 
  VALUES 
    ('[0.1, 0.2, 0.3]'), 
    ('[0.4, 0.5, 0.6]'), 
    ('[0.7, 0.8, 0.9]');
```
## to see the Table Information
```
  SELECT * FROM embeddings;
```
## output
```
  id |    vector
 ----+---------------
  1 | [0.1,0.2,0.3]
  2 | [0.4,0.5,0.6]
  3 | [0.7,0.8,0.9]
```
## we can add users to the databse
```
  CREATE USER new_username WITH PASSWORD 'your_password';
```
Replace new_username with the desired username.<br>
Replace your_password with a secure password for that user.

## Grant permissions to the users
```
  GRANT ALL PRIVILEGES ON DATABASE data TO new_username;
  
  This command allows the user to perform all actions within the specified database
```
```
  GRANT privilege_list ON table_name TO user_name;

  if you want to grant permissions on a specific table
```
## Verify User Creation
```
  SELECT * FROM pg_user WHERE usename = 'new_username';
```
## You can create a pgvector container using either a Docker command or Docker Compose

## Install PgVector container using Docker-compose file
Note:<br>
   docker-compose extention will be .yaml (or) .yml

## Run the Docker Compose Command:
```
 docker-compose up -d

 If you want to run it in detached mode (in the background), use the -d flag:
```
## To access the running Docker container and connect to the pgvector database

```
   docker exec -it pgvector psql -U postgres -h 192.168.0.104 -p 5432 -d data
```
Replace 192.168.0.104 with your local host<br>
Enter the password of pgvector database then you login into database

## To stop the services that are running
```
 docker-compose stop
```
## If you want to stop and remove all containers defined in the file, use
```
 docker-compose down
```








