# Lesson 5: Scaling Concepts

As your application grows, a single MongoDB server might not be enough. This lesson introduces the two main scaling strategies described in the tutorial: **Replica Sets** and **Sharding**.

> **Note**: These are advanced configurations. A simple Docker Compose setup usually runs a "Standalone" instance. Running a Replica Set in Docker requires a more complex configuration (multiple containers networking together).

## 1. High Availability: Replica Sets
A **Replica Set** is a group of `mongod` processes that maintain the same data set.

- **Primary Node**: Receives all write operations.
- **Secondary Nodes**: Replicate operations from the primary.
- **Automatic Failover**: If the primary crashes, secondaries elect a new primary.

In production, you should always run a Replica Set (minimum 3 nodes) to prevent data loss.

### How to Enable in Docker (Conceptual)
To run a replica set in Docker, you would:
1.  Start MongoDB with the `--replSet rs0` argument.
2.  Connect to the shell and run `rs.initiate()`.

## 2. Horizontal Scaling: Sharding
**Sharding** distributes data across multiple machines.

- **When to use**: When your data exceeds the storage or I/O capacity of a single server.
- **Shard Key**: You choose a field (like `customer_id`) to determine how data is split.

Sharding is complex and requires:
- **Config Servers**: Store metadata.
- **Mongos Routers**: Direct client queries to the correct shard.
- **Shard Nodes**: Store the actual data.

## 3. Managed Services vs. Self-Hosted
The guide recommends considering **Managed Services** (like MongoDB Atlas or DigitalOcean Managed Databases) when scaling becomes difficult.

- **Self-Hosted (Docker)**: Cheaper, full control, but you manage backups, scaling, and failover manually.
- **Managed**: Higher cost, but upgrades, backups, and scaling are automated.

For learning and development, Docker is perfect. For large-scale production, a managed service is often safer unless you have a dedicated DevOps team.
