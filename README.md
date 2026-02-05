# MongoDB Docker Tutorial

This guide explains how to set up and run MongoDB using Docker and Docker Compose, simplifying the installation process compared to manual system installation.

Install MongoDB
https://www.digitalocean.com/community/tutorials/how-to-install-mongodb-on-ubuntu-20-04#troubleshooting-and-maintenance

## Detailed Lessons

This repository includes detailed lessons adapted from the DigitalOcean tutorial for Docker users:

- [Lesson 1: Introduction & Prerequisites](./lessons/01_intro_and_prerequisites.md)
- [Lesson 2: Service Management](./lessons/02_service_management.md)
- [Lesson 3: User Security & RBAC](./lessons/03_user_security.md)
- [Lesson 4: Operations (Backup, Restore, Monitor)](./lessons/04_operations.md)
- [Lesson 5: Scaling Concepts](./lessons/05_scaling.md)

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/) installed on your machine.
- [Docker Compose](https://docs.docker.com/compose/install/) installed (often included with Docker Desktop).

## Setup Overview

We use a `docker-compose.yaml` file to define the MongoDB service. This configuration handles:
- Downloading the official MongoDB image.
- Setting up an administrative user.
- Persisting data so it survives container restarts.
- Exposing the database port to your host machine.

## Configuration

The `docker-compose.yaml` file is pre-configured with the following default credentials. **For production use, you should change these.**

```yaml
environment:
  MONGO_INITDB_ROOT_USERNAME: root
  MONGO_INITDB_ROOT_PASSWORD: examplepassword
```

## Running MongoDB

1.  **Start the container**:
    Run the following command in the project directory:

    ```bash
    docker-compose up -d
    ```

    The `-d` flag runs the container in detached mode (in the background).

2.  **Verify it's running**:
    Check the running containers:

    ```bash
    docker ps
    ```
    You should see a container named `mongodb` running on port `27017`.

## Connecting to the Database

You can connect to the database securely using the credentials defined in the compose file.

### Option 1: Using the Container's CLI

You can execute commands directly inside the running container:

```bash
docker exec -it mongodb mongosh -u root -p examplepassword
```

*(Note: older mongo images use `mongo` instead of `mongosh`)*

### Option 2: Using a GUI (Internal or External)

Connect via any MongoDB client (like MongoDB Compass) using:
- **Connection String**: `mongodb://root:examplepassword@localhost:27017/`

## Stopping the Service

To stop the MongoDB container:

```bash
docker-compose down
```

To stop and **remove the data volume** (WARNING: this deletes your database data):

```bash
docker-compose down -v
```
