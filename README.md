## Creating COUCHDB docker container
#
- [Docker](https://docs.docker.com/get-docker/) installed on your system
- [Docker Compose](https://docs.docker.com/compose/install/) installed

## Installing and running and connecting to couchdb database

##  CouchDB database
	- image : couchdb:3.3.3
	- port : 5984
	- container_name : couchdb_c1
	- volume : couchdb_vol1:/opt/couchdb/data
##  Create a container for COUCHDB database
```bash
sudo docker run -d --name couchdb_c1 \
        -p 5984:5984 \
  	-e COUCHDB_USER=user_1 \
  	-e COUCHDB_PASSWORD=mypassword \
  	-v couchdb_vol1:/opt/couchdb/data \
  	--restart always \
  	couchdb:3.4.2
```

## * Explanation of Command Parameters:
	-d: Runs the container in detached mode (in the background).
	--name couchdb_c1: Names the container couchdb_c1.
	-p 5984:5984: Maps port 5984 on the host to port 5984 in the container, allowing external access to CouchDB.
	-e COUCHDB_USER=user_1 and -e COUCHDB_PASSWORD=mypassword: Sets the admin username and password for CouchDB.
	-v couchdb_vol1:/opt/couchdb/data: Mounts a volume couchdb_vol1 to persist CouchDB data on the host.
	--restart always: Configures the container to restart automatically if it stops.
* "/opt/couchdb/data" is the default storage location path in the container

## * It is not possible to create a default database for couchdb , while creating a database container.
# Docker Compose 
### Prerequisites

- [Docker](https://docs.docker.com/get-docker/) installed on your system
- [Docker Compose](https://docs.docker.com/compose/install/) installed

Ensure that Docker and Docker Compose are installed on your system before proceeding.

## Usage

### 1. Start Couchdb

To start Couchdb , use the following command:

```bash
sudo docker compose up -d
```
### 2. Verify Containers

To check if the containers are up and running, in use:

```bash
sudo docker compose ps
```
After running the `sudo docker compose ps` command, you should see the output similar to the data below
```bash
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                                           NAMES
657b9f170917   couchdb   "tini -- /docker-ent…"   8 minutes ago   Up 8 minutes   4369/tcp, 9100/tcp, 0.0.0.0:5984->5984/tcp, :::5984->5984/tcp   couchdb-couchdb-1
```
## Stopping and Cleaning Up

To stop all containers:

```bash
sudo docker compose down
```
This command will gracefully stop all running containers.
```bash
sudo docker volume remove $(sudo docker volume ls -q)
```
 * Warning:This command will remove all volumes which have been mounted so far

#
## To connect to the couchdb container
## This is only to test if data is being inserted and being stored securely,
## We can connect to database server through application, after checking with this process
```bash
suso docker exec -it container-name bash
```
 - Replace container-name with couchdb_c1

## Insert some data like creating a database or tables
## To create a database in couchdb through bash
```bash
	curl -u user_1:mypassword -X PUT http://localhost:5984/sample_db	
```
## To insert sample data in sample_db through bash
```bash
curl -u user_1:mypassword -X POST http://localhost:5984/sample_db -H "Content-Type: application/json" -d '{
    "name": "John Doe",
    "email": "john.doe@example.com",
    "age": 30,
    "address": {
    "street": "123 Main St",
    "city": "Anytown",
    "zipcode": "12345"
    }
    }'
```
## To check the data 
```bash
curl -u user_1:mypassword -X GET http://localhost:5984/sample_db/_all_docs?include_docs=true
```
#  Remove or stop the container and create another container with same volume mount
#  Check if the data exist or not
```bash
curl -u user_1:mypassword -X GET http://localhost:5984/sample_db/_all_docs?include_docs=true
```
############################################################################################################################################
