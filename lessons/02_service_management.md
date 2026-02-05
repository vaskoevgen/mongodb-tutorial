# Lesson 2: Service Management

In a traditional Ubuntu installation, you use `systemctl` to manage the MongoDB service. In our Docker environment, we use the Docker CLI to achieve the same results.

## Starting the Service (`systemctl start`)
To start the MongoDB container in the background (detached mode):

```bash
docker-compose up -d
```

This is equivalent to `sudo systemctl start mongod` but also creates the container if it doesn't exist.

## Checking Service Status (`systemctl status`)
To verify the service is active and running:

```bash
docker ps
```

Look for the `mongodb` container in the list. The `STATUS` column should say `Up X minutes`.
If you don't see it, it might have crashed. Check all containers (including stopped ones) with `docker ps -a`.

## Checking Logs (`journalctl`)
To view the standard output logs (similar to looking at `/var/log/mongodb/mongod.log`):

```bash
docker-compose logs -f mongodb
```

- `-f`: Follows the log output in real-time.
- Press `Ctrl+C` to exit.

## Stopping and Restarting
- **Restart** (`systemctl restart mongod`):
  ```bash
  docker-compose restart mongodb
  ```

- **Stop** (`systemctl stop mongod`):
  ```bash
  docker-compose stop mongodb
  ```

- **Stop and Remove** (Clean up container resources):
  ```bash
  docker-compose down
  ```
  *Note: This preserves the named volume `mongo_data`, so your data is safe.*

## Verifying Network Binding
MongoDB listens on port 27017. In Docker, we mapped this port to your host. You can verify it's listening with:

```bash
docker port mongodb
```
Output: `27017/tcp -> 0.0.0.0:27017`
