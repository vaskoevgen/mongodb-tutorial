# Lesson 1: Introduction & Prerequisites

## Overview
This tutorial series adapts the comprehensive "How To Install MongoDB on Ubuntu" guide for a Docker-based environment. By using Docker, we avoid many system-specific installation steps (like adding apt repositories) but still need to master key management tasks:

1.  **Service Management**: Starting, stopping, and verifying the database service.
2.  **User Security**: Creating administrative users and enforcing authentication.
3.  **Operations**: Backing up data and monitoring performance.
4.  **Scaling**: Understanding replica sets and sharding.

## Prerequisites
To follow along, you need:

1.  **Docker & Docker Compose**: Installed on your machine.
    *   [Install Docker](https://docs.docker.com/get-docker/)
2.  **Command Line Interface**: A terminal to run Docker commands and `mongosh`.

## The Docker Environment
In this tutorial, we use a `docker-compose.yaml` file to define our MongoDB service. This file acts as our "configuration," replacing many systemd service files you'd manage on a bare-metal server.

Key configuration in our `docker-compose.yaml`:
- **Image**: `mongo:latest` (Official MongoDB image)
- **Port**: `27017` (Default MongoDB port exposed to host)
- **Volume**: `mongo_data:/data/db` (Persistent storage for our database files)
