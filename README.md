# Apache/age-infrastructure

## Prerequisites

1. Docker installed on your system

2. Docker compose installed

## Install Apache/age container using Docker(Command)

To install the Apache/age Docker container, you can use the following command:
```bash
docker run \
    --name Apache  \
    -p 5432:5432 \
    -e POSTGRES_PASSWORD=postgresPW \
    -v vol:/var/lib/postgresql/data \
    -e POSTGRES_DB=postgresDB \
    -d \
    apache/age
```
`-docker run:`
       This is the command used to create and start a new container from a specified image.

`--name Apache:`
      This option assigns the name "Apache" to the container. Naming your containers makes them easier to manage.

`-p 5455:5432:`
       This flag maps port 5432 inside the container (the default port for PostgreSQL) to port 5455 on the host machine. This means that you can access the PostgreSQL service running in the container via localhost:5455 on your host.

`-v pgvect:/var/lib/postgresql/data:`
          This option mounts a volume named pgvect at the specified path inside the container. This is where PostgreSQL stores its data, allowing persistent storage even if the container is stopped or removed.

`-e POSTGRES_PASSWORD=postgresPW:`
             This environment variable sets the password for the PostgreSQL user (the default user is "postgres"). In this case, it is set to "postgresPW".

`-e POSTGRES_DB=postgresDB:`
      This environment variable creates a new database named "postgresDB" upon initialization of the PostgreSQL server in the container.

`-d:`
     This flag runs the container in detached mode, meaning it runs in the background and does not block your terminal.
     
`apache/age:`
       This specifies the Docker image to use for creating the container. In this case, it's an image named "apache/age", which likely includes both Apache and PostgreSQL components

## To access the running Docker container and connect to the pgvector database
```bash
  docker exec -it Apache psql -U postgres -h 192.168.0.104 -p 5432 -d postgresDB
```
Replace 192.168.0.104 with your local host<br>
Enter the password of pgvector database then you login into database

## After login into Apache/age database create a Extention
```psql
 CREATE EXTENSION age;
LOAD 'age';
SET search_path = ag_catalog, "$user", public;
```

## create a database in Apache/age
```psql
  create database ram;
```
## list all the databases
```psql
   \l
```
## switch to another database
```psql
   \c ram(database-Name);
```
## Insert table in ram Database
### Create a graph in a ram database using the command
```psql
    SELECT create_graph('my_graph');
```
## Insert Data into graph 
```psql
 SELECT * FROM cypher('my_graph', $$
CREATE (a:Person {name: "Alice", age: 30}),
       (b:Person {name: "Bob", age: 25}),
       (c:Person {name: "Charlie", age: 35}),
       (a)-[:FRIENDS_WITH]->(b),
       (b)-[:FRIENDS_WITH]->(c)
$$
) AS (result agtype);
```
## Querying the Data
## Retrieve All Nodes:
```psql
 SELECT * FROM cypher('my_graph', $$
MATCH (n)
RETURN n
$$
) AS (node agtype);
```
## output
```psql
                                                node
--------------------------------------------------------------------------------------------------
 {"id": 844424930131969, "label": "Person", "properties": {"age": 30, "name": "Alice"}}::vertex
 {"id": 844424930131970, "label": "Person", "properties": {"age": 25, "name": "Bob"}}::vertex
 {"id": 844424930131971, "label": "Person", "properties": {"age": 35, "name": "Charlie"}}::vertex
(3 rows)
```
## Retrieve Specific Nodes:
```psql
 SELECT * FROM cypher('my_graph', $$
MATCH (p:Person {name: "Alice"})
RETURN p
$$
) AS (person agtype);
```
## output
```psql
                                               person
------------------------------------------------------------------------------------------------
 {"id": 844424930131969, "label": "Person", "properties": {"age": 30, "name": "Alice"}}::vertex
(1 row)
```

## Retrieve Nodes with Relationships:
```psql
 SELECT * FROM cypher('my_graph', $$
MATCH (a)-[r]->(b)
RETURN a, r, b
$$) AS (node_a agtype, relationship agtype, node_b agtype);
```
## Retrieve All Relationships:
```psql 
 SELECT * FROM cypher('my_graph', $$
MATCH ()-[r]->()
RETURN r
$$
) AS (relationship agtype);
```
## Delete a Graph
```psql
 drop_graph(graph_name, cascade);
```
## adding users 
## Adding Users as Nodes
 You can add users by creating nodes with the label User and including properties such as name, email, phone, and address. Here’s how to do it:

## adding single users
```psql
 SELECT * FROM cypher('my_graph', $$
CREATE (:User {name: "Alice", email: "alice@example.com", phone: "1234567890", address: "Wonderland"})
$$
) AS (result agtype);
```
## add multiple users
```psql
 SELECT * FROM cypher('my_graph', $$
CREATE 
    (:User {name: "Alice", email: "alice@example.com", phone: "1234567890", address: "Wonderland"}),
    (:User {name: "Bob", email: "bob@example.com", phone: "0987654321", address: "Builderland"})
$$
) AS (result agtype);
```
## Verifying Inserted Users
```bash
 SELECT * FROM cypher('my_graph', $$
MATCH (u:User)
RETURN u
$$
) AS (result agtype);
```
## output
```psql
                                                                               result

------------------------------------------------------------------------------------------------------------------------------------------------------------------
 {"id": 1407374883553281, "label": "User", "properties": {"name": "Alice", "email": "alice@example.com", "phone": "1234567890", "address": "Wonderland"}}::vertex
 {"id": 1407374883553282, "label": "User", "properties": {"name": "Alice", "email": "alice@example.com", "phone": "1234567890", "address": "Wonderland"}}::vertex
 {"id": 1407374883553283, "label": "User", "properties": {"name": "Bob", "email": "bob@example.com", "phone": "0987654321", "address": "Builderland"}}::vertex
(3 rows)
```
## Instructions to Grant Permissions to users
## Grant USAGE on the ag_catalog Schema
```psql
 GRANT USAGE ON SCHEMA ag_catalog TO username;
```
Replace username with the actual username
## Grant Privileges on Tables in the ag_catalog Schema
```psql
 GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA ag_catalog TO username;
```
##  Grant USAGE on Your Graph Schema
```psql
  GRANT USAGE ON SCHEMA test TO username;
GRANT ALL PRIVILEGES ON SCHEMA test TO username;
```
## Grant Privileges on Specific Tables
```psql
 GRANT ALL PRIVILEGES ON TABLE test._ag_label_vertex TO username;
```
## You can create a Apache/age container using either a Docker command or Docker Compose

## Install Apache/age container using Docker-compose file
Note:<br>
   docker-compose extention will be .yaml (or) .yml

## Run the Docker Compose Command:
```
 docker-compose up -d

 If you want to run it in detached mode (in the background), use the -d flag:
```
## To access the running Docker container and connect to the Apache/age database

```
   docker exec -it Apache psql -U postgres -h 192.168.0.104 -p 5432 -d postgresDB
```
Replace 192.168.0.104 with your local host<br>
Enter the password of Apache/age database then you login into database

## To stop the services that are running
```
 docker-compose stop
```
## If you want to stop and remove all containers defined in the file, use
```
 docker-compose down
```

