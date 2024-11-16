# Creating a Redis docker container
#
- [Docker](https://docs.docker.com/get-docker/) installed on your system
- [Docker Compose](https://docs.docker.com/compose/install/) installed


## There are two docker images for redis,	
 * redis/redis-stack contains both Redis Stack server and Redis Insight. This container is best for local development because you can use the embedded Redis Insight to visualize your data.

 * redis/redis-stack-server provides Redis Stack server only. This container is best for production deployment
#
## To create a Redis docker container run below command
```bash
sudo docker run -d --name redis-stack -p 6379:6379 -p 8001:8001 -v redis-data:/data redis/redis-stack
```
# Docker Compose
Ensure that you have the `docker-compose.yml` file which is mentioned in this repository
## Usage

### 1. Start Redis

To start Redis and its dependencies, use the following command:

```bash
$ sudo docker compose up -d
```

### 2. Verify Containers

To check if the containers are up and running, use:

```bash
$ sudo docker compose ps
```
### Expected Output

After running the `sudo docker compose ps` command, you should see output similar to the following:

## Stopping and Cleaning Up

To stop all containers:

```bash
sudo docker compose down
```
This command will gracefully stop all running containers.

## To connect to the redis-stack run the command
```bash
sudo docker exec -it redis-stack redis-cli
```
 * After Exwcuting the above command , Insert some data
#
 * Sample Data
```redis
HSET user:1000 name "John Doe"
HSET user:1000 age 30
HSET user:1000 country "USA"
```
 * To view the data
```redis
HGETALL user:1000
```
## After inserting data and exited from redis, We must save the data manually
```bash
sudo docker exec -it redis-stack redis-cli SAVE
```
 * Once executing the above command the output will be "OK"
 * It means the data we insert will be saved 
#
#############################################################################################################################################

