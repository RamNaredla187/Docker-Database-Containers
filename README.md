# PostgresDB-infrastructure

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/) installed on your system
- [Docker Compose](https://docs.docker.com/compose/install/) installed

Ensure that Docker and Docker Compose are installed on your system before proceeding.

## Creating a volume:

`docker volume create pgdata15`

# Run PostgreSQL containers with volume:

```
`docker run --name postgres15 \
    -e POSTGRES_PASSWORD=mysecretpassword \
    -p 5432:5432 \
    -v pgdata15:/var/lib/postgresql/data \
    -d postgres:15-alpine`
- Replace the password you want to setup.

```
You can stop and delete this container as follows:

`docker stop postgres15`

`docker rm postgres15`

# After Running the Installation Command:
- A Docker container named postgres15 has been started on port 5432(HTTP).
- postgresdb is configured with the default username postgres and the password you mentioned.
- Data, logs, importfiles, and plugins are persisted using Docker volume (pgdata15), ensuring that your data remains intact across container restarts and upgrades.

## Connect to PostgreSQL

You can connect to each PostgreSQL instance using the following commands:

`docker exec -it <container name> psql -U <data base name>`

# PostgresDB Setup with Docker Compose:

-- Create a new user

`CREATE USER <user name > WITH PASSWORD <your password>;`

# Login as user

`docker exec -it <container name> psql -U <username> -d <data base name>`

# PostgreSQL with Docker-compose:


## you have to add pgvector extension to your PostgreSQL container by using an initialization script that installs pgvector when the container is first started.

> Create a file named "init-pgvector.sql"

> This SQL command installs the pgvector extension when the database initializes `CREATE EXTENSION IF NOT EXISTS vector;`


```

version: '3.8'

services:
  db:
    image: postgres:15  
    container_name: newcontainer  
    restart: always  # Restart policy
    environment:
      POSTGRES_DB: ${POSTGRES_DB}
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./init-pgvector.sql:/docker-entrypoint-initdb.d/init-pgvector.sql  # Initialization script for pgvector

volumes:
  postgres_data:

```
# To avoid exposing credentials directly in your docker-compose.yml file, you can use environment variables or a .env file:

- open .env file and include the following:

```

POSTGRES_PASSWORD: <your password>
POSTGRES_USER: <username>
POSTGRES_DB: <Database name to access>

```








