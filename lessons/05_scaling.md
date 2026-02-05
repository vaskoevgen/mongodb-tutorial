# Lesson 5: Scaling Concepts

As your application grows, a single MongoDB server might not be enough. This lesson introduces the two main scaling strategies described in the tutorial: **Replica Sets** and **Sharding**.

## 1. High Availability: Replica Sets
A **Replica Set** is a group of `mongod` processes that maintain the same data set.

- **Primary Node**: Receives all write operations.
- **Secondary Nodes**: Replicate operations from the primary.
- **Automatic Failover**: If the primary crashes, secondaries elect a new primary.

In production, you should always run a Replica Set (minimum 3 nodes) to prevent data loss.

### Practical Demo: Running a Replica Set
We have created a separate configuration file `docker-compose-replica.yaml` to demonstrate a 3-node cluster.

**1. Stop the existing single instance:**
```bash
docker-compose down
```

**2. Start the Replica Set:**
```bash
docker-compose -f docker-compose-replica.yaml up -d
```
This starts 3 containers: `mongo1`, `mongo2`, and `mongo3`.
*Note: The configuration includes a healthcheck that automatically initializes the replica set for you.*

**3. Check the Status:**
Wait about 30 seconds for them to start and elect a primary, then check the status:

```bash
docker exec -it mongo1 mongosh --eval "rs.status()"
```
You should see one member as `"stateStr": "PRIMARY"` and others as `"SECONDARY"`.

**4. Test Failover:**
Stop the primary node (assume `mongo1` is primary):
```bash
docker-compose -f docker-compose-replica.yaml stop mongo1
```
Check status on `mongo2`:
```bash
docker exec -it mongo2 mongosh --eval "rs.status()"
```
You will see that `mongo2` or `mongo3` has likely been promoted to PRIMARY.

**5. Clean Up:**
When finished, shut down the cluster to save resources:
```bash
docker-compose -f docker-compose-replica.yaml down -v
```

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
