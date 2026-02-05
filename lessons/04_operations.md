# Lesson 4: Operations

This lesson covers essential day-to-day operations: Checking health, Backing up data, and Restoring data.

## 1. Monitoring Health (`mongostat`)
The `mongostat` tool provides a quick overview of your database's status (inserts, queries, updates, deletes per second).

Run it via `docker exec`:
```bash
docker exec -it mongodb mongostat -u root -p examplepassword --authenticationDatabase admin
```

You will see a live updating table of statistics. Press `Ctrl+C` to exit.

## 2. Server Status
For a detailed JSON report of server health (connections, memory, etc.), use the `db.serverStatus()` command inside the shell:

```bash
docker exec -it mongodb mongosh -u root -p examplepassword --eval "printjson(db.serverStatus())"
```

## 3. Backing Up Data (`mongodump`)
You should regularly back up your data. `mongodump` creates a binary export of your database.

To back up all databases to a folder named `/dump` inside the container:
```bash
docker exec mongodb mongodump -u root -p examplepassword --out /dump
```

**Extracting the backup to your host:**
The backup currently exists *inside* the container. To copy it to your host machine:

```bash
docker cp mongodb:/dump ./my_backup
```

You now have a `my_backup` folder on your computer containing the BSON files.

## 4. Restoring Data (`mongorestore`)
To restore data from a backup:

1.  Copy the backup data into the container (if it's not already there):
    ```bash
    docker cp ./my_backup mongodb:/restore_source
    ```

2.  Run `mongorestore`:
    ```bash
    docker exec mongodb mongorestore -u root -p examplepassword /restore_source
    ```

This will replay the data from the BSON files into your running database.
