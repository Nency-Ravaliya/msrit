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

### Script to Drop All Collections in a Database

```
// Switch to the target database
const userDB = db.getSiblingDB("userDatabase");

// Retrieve all collection names and drop each one
userDB.getCollectionNames().forEach(function(collectionName) {
  print("Dropping collection: " + collectionName);
  userDB.getCollection(collectionName).drop();
});
```

run this command:
```
mongosh --file dropCollections.js
```

### How to drop all the tables from particular 10 users from one database

```
// Specify the target database
const dbName = "myDatabase";
const targetDB = db.getSiblingDB(dbName);

// List of 10 usernames whose collections you want to drop.
// Adjust these names according to your naming convention.
const users = [
  "user1", "user2", "user3", "user4", "user5",
  "user6", "user7", "user8", "user9", "user10"
];

// Get all collection names in the database
const collections = targetDB.getCollectionNames();

collections.forEach(function(collName) {
  // Check if the collection name starts with any of the user names followed by an underscore.
  for (let i = 0; i < users.length; i++) {
    if (collName.startsWith(users[i] + "_")) {
      print("Dropping collection: " + collName);
      targetDB.getCollection(collName).drop();
      // Once dropped, break out of the inner loop
      break;
    }
  }
});
```

Run this command:

```
mongosh --file dropUserCollections.js
```
