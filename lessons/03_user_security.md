# Lesson 3: User Security & RBAC

Security is critical for any database deployment. This lesson covers Authentication and Role-Based Access Control (RBAC), adapting the "Securing MongoDB" section of the guide.

## 1. The Admin User (Already Configured)
In the original guide, you manually create an administrative user. In our Docker setup, we accomplished this via environment variables in `docker-compose.yaml`:

```yaml
environment:
  MONGO_INITDB_ROOT_USERNAME: root
  MONGO_INITDB_ROOT_PASSWORD: examplepassword
```

This creates a user with `root` privileges (effectively `userAdminAnyDatabase` + `readWriteAnyDatabase` + others).

## 2. Connecting as Admin
To perform administrative tasks, you must connect as this root user. 

Inside the container:
```bash
docker exec -it mongodb mongosh -u root -p examplepassword
```

You are now in the MongoDB shell (`test>`).

## 3. Creating Application-Specific Users
It is a best practice **not** to use the root user for your applications (like your backend API). Instead, create a user with limited permissions (e.g., only `readWrite` access to a specific database).

### Step-by-Step:
1.  Switch to the application database (it will be created if it doesn't exist):
    ```javascript
    use ecommerce_db
    ```

2.  Create the user:
    ```javascript
    db.createUser({
      user: "app_user",
      pwd: "secure_password_123",
      roles: [ { role: "readWrite", db: "ecommerce_db" } ]
    })
    ```

3.  Verify the user exists:
    ```javascript
    show users
    ```

## 4. Testing Application Access
Exit the root shell (`exit`) and try connecting as the new limited user:

```bash
docker exec -it mongodb mongosh "mongodb://app_user:secure_password_123@localhost:27017/ecommerce_db"
```

If you can connect and insert data into `ecommerce_db` but not other databases, your RBAC is working correctly.

## 5. Network Restrictions
By default, your Docker container maps port `27017` to `0.0.0.0` (all interfaces) on your host. 
To restrict access to localhost only (similar to `bindIp: 127.0.0.1`), update your `docker-compose.yaml`:

```yaml
ports:
  - "127.0.0.1:27017:27017"
```

This ensures only services on your host machine can access the database, blocking external internet traffic.
