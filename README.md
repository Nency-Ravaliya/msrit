# Step 1: Start the MongoDB Shell (mongosh)

Open a terminal and run:
 
```
mongosh
```

# Step 2: Create a New Database

MongoDB creates a database automatically when you switch to a non-existent database and insert data. Use the use command:

```
use mydatabase
```

Replace mydatabase with your preferred database name.

# Step 3: Create a User

Create a user with a username, password, and assigned roles:

```
db.createUser({
  user: "myuser",
  pwd: "mypassword",
  roles: [{ role: "readWrite", db: "education1" }]
})
```

- Replace "myuser" with your desired username.
- Replace "mypassword" with a secure password.
- The readWrite role allows the user to read and write data in mydatabase.

# Step 4: Exit the Shell

```
exit
```

# Step 5: Enable Authentication (Optional but Recommended)

Edit the MongoDB configuration file to enable authentication:
```
sudo nano /etc/mongod.conf
```
Find the security section and add:

```
security:
  authorization: enabled
```

Save the file (Ctrl + X, then Y, then Enter) and restart MongoDB:

```
sudo systemctl restart mongod
```

# Step 6: Connect with the New User

Now, connect to MongoDB using authentication:

```
mongosh -u myuser -p mypassword --authenticationDatabase mydatabase
```

This ensures that the user has proper access.

You're all set! Let me know if you run into any issues.

---

# Create Multiple Databases and Users with a Script

```
// Specify the target database name
const dbName = "myDatabase";
// Switch to the target database
const targetDB = db.getSiblingDB(dbName);

for (let i = 1; i <= 10; i++) {
  const username = "user" + i;
  const password = "password" + i;  // Customize passwords as needed
  targetDB.createUser({
    user: username,
    pwd: password,
    roles: [{ role: "readWrite", db: dbName }]
  });
  print("Created user: " + username);
}
```

run this:

```
mongosh --file createUsers.js
```
