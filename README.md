# Creating MongoDB Docker Container

# * Install Docker 

## * update & upgrade the ubuntu, update the system with the command
```bash
sudo apt update && sudo apt upgrade 
```
## * install the docker software with the command below
```bash
sudo apt install docker -y
```
## * Installed latest version of docker with the command
```bash
curl -fsSL https://get.docker.com -o install-docker.sh
sudo sh install-docker.sh
```
## * check the docker with command
```bash
docker --version
```
# * Creating Mongodb Container
#
# There are two ways to run Mongodb Container
	- 1. command line
	- 2. with docker compose 
# 1.Command Line

## Run the mongodb container with below comand
* Use command this for the mongo:8.0 version
```bash
docker run -d \
    --name mongodb-c1 \
    -e MONGO_INITDB_ROOT_USERNAME=user_1 \
    -e MONGO_INITDB_ROOT_PASSWORD=mypassword \
    -v mongo-data-vol1:/data/db \
    -p 27017:27017 \
    mongo:8.0
```
* Use this command for the mongo:7.0 version
```bash
docker run -d \
    --name mongodb-c1 \
    -e MONGO_INITDB_ROOT_USERNAME=user_1 \
    -e MONGO_INITDB_ROOT_PASSWORD=mypassword \
    -v mongo-data-vol2:/data/db \
    -p 27017:27017 \
    mongo:7.0
```
* Use this for the mongo:6.0
```bash
docker run -d \
    --name mongodb-c1 \
    -e MONGO_INITDB_ROOT_USERNAME=user_1 \
    -e MONGO_INITDB_ROOT_PASSWORD=mypassword \
    -v mongo-data-vol3:/data/db \
    -p 27017:27017 \
    mongo:6.0
```
# * Replace 'mypassword' with your custom password
## exaplaination of the command

	docker run: Command to create and run a new container.

	-d: Runs the container in the background

	--name mongodb-c1 : assigns a name to the container

    -p 27017:27017: allowing you to access MongoDB from your host machine with port 27017

    -e MONGO_INITDB_ROOT_USERNAME=user_1: Sets the root username.

    -e MONGO_INITDB_ROOT_PASSWORD=<mypassword>: sets the root password.

    -v mongo-data:/data/db: Mounts the specified host directory to the container’s /data/db directory. This is where MongoDB stores its data, 

    mongo: Specifies the image to use, in this case, the official MongoDB image from Docker Hub.
#
## After running the container with the command, check the container
```bash
sudo docker ps -a
```
	displays detailes of running containers 				
## To check the images
```bash 
sudo docker images
```
## To check the volumes
```bash
sudo docker volume ls
```
# 2. With Docker Compose
### Prerequisites

- [Docker](https://docs.docker.com/get-docker/) installed on your system
- [Docker Compose](https://docs.docker.com/compose/install/) installed

Ensure that Docker and Docker Compose are installed on your system before proceeding.

## Usage

### 1. Start Mongo

To start Mongodb , use the following command:

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
NAME         IMAGE         COMMAND                  SERVICE   CREATED          STATUS          PORTS
mongodb_c1   mongo:8.0.1   "docker-entrypoint.s…"   mongodb   23 seconds ago   Up 21 seconds   0.0.0.0:27017->27017/tcp, :::27017->27017/tcp
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

## Now connect to the running Mongodb Container with command
```bash
sudo docker exec -it mongodb-c1 mongosh -u "user_1" -p "<mypassword>" --authenticationDatabase "admin"
```
* If mongosh does not working replace mongosh with  `mongo`. 
# * Replace <mypassword> with your custom password 
#
	docker exec: Executes a command in a running container.

	-it: Allocates a pseudo-TTY and keeps terminal open, allowing you to interact with the MongoDB shell.

	mongodb-c1: The name of the running MongoDB container (make sure this matches the name you used when starting the container).

	mongosh: This is the command to start the MongoDB shell.

	-u "user_1": The username you are using to authenticate.

	-p "mypassword": The password for the specified user.()

	--authenticationDatabase "admin": This specifies the database against which the user is authenticated. This should be the database where the user was created
#
## After connecting to the mongodb, insert some data
```mongosh
use food
```
- switched to db food
```monsh 
db.createCollection("fruits")
```
- { ok: 1 }
```mongosh
db.fruits.insertMany([ {name: "apple", origin: "usa", price: 5}, {name: "orange", origin: "italy", price: 3}, {name: "mango", origin: "malaysia", price: 3} ])
```
#
## Check the data
```mongosh
use food
```
- switched to db food

```mongosh
db.fruits.find().pretty()
```
## To create the users inside the mongodb database
```mongosh
db.createUser({
    user: "new_user",
    pwd: "new_user_password",  // Use a strong password
    roles: [
    { role: "readWrite", db: "food" }  // This gives read and write access to `mydatabase`
    ]
    })   
```
## There are different roles we can give to the users like,
	* read			: read only access
	* readWrite		: read and write access
	* dbAdmin		: admin access to specific database
	* dbOwner		: owner access to specific database
	* userAdmin		: admin access over all users
	* readWriteAnyDatabase	: read write access over all databases
#
## To View users
```mongosh	
db.getUsers()
```
#
## To stop and Start the docker containers
```bash
sudo docker stop container-name 
	
sudo docker start container-name 
```
  
##############################################################################################################################################
