<h1 align="center">Mongodb-Community-Configuration</h1>

| [Installation](#install-mongodb-community-edition) | [Start MongoDB](#start-mongodb) | [Using MongoDB](#begin-using-mongodb) | [Creating a User](#creating-a-user) | [Enable Authentication](#enable-authentication)|
| :--: | :--: | :--: | :--: | :--: |

| [Access with Authentication](#access-mongodb-shell-with-authentication) | [Connection with Host](#enable-connections-with-a-hostname-or-host-ip) |[Uninstall MongoDB](#uninstall-mongodb-community-edition) |
| :--: | :--: | :--: |

## Install MongoDB Community Edition on Ubuntu 

### Considerations 

#### Platform Support 

MongoDB 7.0 Community Edition supports the following 64-bit Ubuntu LTS (long-term support) releases on x86_64 architecture: 

`22.04 LTS ("Jammy")`

`20.04 LTS ("Focal")`

MongoDB only supports the 64-bit versions of these platforms. To determine which Ubuntu release your host is running, run the following command on the host's terminal: 

```nginx
cat /etc/lsb-release
```

## Install MongoDB Community Edition 

Follow these steps to install MongoDB Community Edition using the apt package manager. 

### Import the public key used by the package management system 

From a terminal, install gnupg and curl if they are not already available: 

```nginx
sudo apt-get install gnupg curl
```

To import the MongoDB public GPG key from https://pgp.mongodb.com/server-7.0.asc, run the following command:

```nginx
curl -fsSL https://pgp.mongodb.com/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor
```

### Create a list file for MongoDB 

Create the list file `/etc/apt/sources.list.d/mongodb-org-7.0.list` for your version of Ubuntu. 

Two options: 
1. **Ubuntu 22.04 (Jammy)**
2. **Ubuntu 20.04 (Focal)**

Create the `/etc/apt/sources.list.d/mongodb-org-7.0.list` file for **`Ubuntu 22.04 (Jammy)`**: 

```nginx
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```

Create the `/etc/apt/sources.list.d/mongodb-org-7.0.list` file for **`Ubuntu 20.04 (Focal)`**: 

```nginx
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```

### Reload local package database 

Issue the following command to reload the local package database: 

```nginx
sudo apt-get update
```

### Install the MongoDB packages 

You can install either the latest stable version of MongoDB or a specific version of MongoDB. 

To install the latest stable version, issue the following 

```nginx
sudo apt-get install -y mongodb-org
```

**Installation is completed.** 

## Start MongoDB 

You can start the mongod process by issuing the following command: 

```nginx
sudo  systemctl start mongod
```

If you receive an error similar to the following when starting mongod: 

*Failed to start mongod.service: Unit mongod.service not found.*

Run the following command first: 

```nginx
sudo  systemctl daemon-reload
```

Then run the start command above again. 

### Verify that MongoDB has started successfully. 

```nginx
sudo  systemctl status mongod
```

You can optionally ensure that MongoDB will start following a system reboot by issuing the following command: 

```nginx
sudo  systemctl enable mongod
```

### Stop MongoDB

As needed, you can stop the mongod process by issuing the following command: 

```nginx
sudo  systemctl stop mongod
```

### Restart MongoDB

You can restart the mongod process by issuing the following command: 

```nginx
sudo  systemctl restart mongod
```

You can follow the state of the process for errors or important messages by watching the output in the `/var/log/mongodb/mongod.log` file. 

## Begin using MongoDB

Start a `mongosh` session on the same host machine as the mongod. You can run mongosh  without any command-line options to connect to a mongod that is running on your **`localhost`** with default **`port 27017`**. 

```nginx
mongosh
```

If all goes well, you will get a MongoDB shell of your localhost. You can perform [MogoDB Shell Operatioins](https://github.com/ipavanteja/mongodb-import-export?tab=readme-ov-file#mongodb-shell-commands)

## Connection

If your MongoDB Server is running locally, you can use the connection string "mongodb://localhost:<port>" where <port> is the port number you configured your server to listen for incoming connections. The default port is `27017`

```nginx
mongodb://localhost:27017
```

### Create a Database

- Switch to the Admin Database:
In the MongoDB shell, switch to the admin database:

```nginx
use admin
```

- Create a New Database:
To create a new database, you can use the use command followed by the desired database name. For example, let's create a database named mydatabase:

```nginx
use mydatabase
```
If the database does not exist, MongoDB will create it for you. Note that the database won't appear in the list of databases until you insert some data into it.

- Or, you can directly [Restore the DB from your Atlas Server](https://github.com/ipavanteja/mongodb-import-export?tab=readme-ov-file#1-export-data-from-mongodb-atlas)

## Creating a User

With access control enabled, users are required to identify themselves. You have to grant a user one or more roles. A role grants a user privileges to perform certain actions on MongoDB resources.

- Create a user for a specific DB (**DB Owner**)

   - Connect to mongo shell
   ```nginx
   mongosh
   ```
   - Switch to your DB
   ```nginx
   use yourDB
   ```
   - Use the `db.createUser()` method to create **DB Owner** user
   ```javascript
   db.createUser(
      {
         user: "YourUserName",
         pwd: "YourPassword",
         roles: [ { role: "dbOwner", db: "yourDB" } ]
      }
   )
   ```
   - Verify User Creation
   ```nginx
   db.getUsers()
   ```
   This command will display a list of users in your database, including the newly created user.

## Enable Authentication
Enabling authentication in MongoDB adds a layer of security to your database by requiring users to authenticate with valid credentials before accessing the database.

- Edit MongoDB Configuration
```nginx
sudo nano /etc/mongod.conf
```

Look for the security section in the configuration file. If it doesn't exist, add the following lines to enable authentication:
```conf
security:
  authorization: enabled
```
Save the changes and exit the text editor.

- Restart MongoDB
After making changes to the configuration, restart the MongoDB service to apply the new settings
```nginx
sudo systemctl restart mongod
```

### Access MongoDB Shell with Authentication
- Authenticate during Connection
```nginx
mongosh -u <userName> --authenticationDatabase <databaseName> -p
```
After pressing `enter` it will ask for the password. Type the password of that user and press `enter` and you will be logged in successfully.

## Enable connections with a hostname or host IP
To enable connections with a hostname or host IP in MongoDB, you need to configure MongoDB to bind to the appropriate network interfaces and provide the necessary authentication details

- Edit MongoDB Configuration
```nginx
sudo nano /etc/mongod.conf
```

Look for the net section in the configuration file. If it doesn't exist, add the following lines to specify the network interfaces and their bind addresses

```conf
net:
  bindIp: hostname_or_ip
```

Replace hostname_or_ip with the actual hostname or IP address you want to allow connections from. If you want to allow connections from any IP address, you can use **`0.0.0.0`**

```conf
net:
  bindIp: 0.0.0.0
```
Save the changes and exit the text editor.

- Restart MongoDB
```nginx
sudo systemctl restart mongod
```

## Connection with Host IP and Authentication
MongoDB connection string with a username, password, and host IP

```nginx
mongodb://username:password@host_ip:port
```

Replace the following placeholders with your actual values:

- **`username`**: Your MongoDB username
- **`password`**: Your MongoDB password
- **`host_ip`**: The IP address of your MongoDB server
- **`port`**: The MongoDB server port (default is **`27017`**)


## Uninstall MongoDB Community Edition

To completely remove MongoDB from a system, you must remove the MongoDB applications themselves, the configuration files, and any directories containing data and logs. The following section guides you through the necessary steps. 

### Stop MongoDB

Stop the mongod process by issuing the following command: 

```nginx
sudo service mongod stop
```

### Remove Packages

Remove any MongoDB packages that you had previously installed. 

```nginx
sudo apt-get purge "mongodb-org*"
```

### Remove Data Directories

Remove MongoDB databases and log files. 

```nginx
sudo rm -r /var/log/mongodb
```
```nginx
sudo rm -r /var/lib/mongodb
```

### Remove MongoDB Configuration Files

Remove MongoDB configuration files, if any:

```nginx
sudo rm /etc/mongod.conf
```

### Update Package Cache

Update the package cache to ensure that the changes take effect:

```nginx
sudo apt-get update
```

### Optional: Remove MongoDB Tools

If you installed MongoDB tools like mongosh or mongodb-database-tools, you can remove them as well:

```nginx
sudo apt-get purge mongosh
sudo apt-get purge mongodb-database-tools
```

### Clean Up

Finally, you can clean up unnecessary packages and dependencies:

```nginx
sudo apt-get autoremove
```

[Go to Top](#mongodb-community-configuration)
