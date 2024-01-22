# Mongodb-Community-Configuration

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

Install MongoDB Community Edition 

Follow these steps to install MongoDB Community Edition using the apt package manager. 

Import the public key used by the package management system 

From a terminal, install gnupg and curl if they are not already available: 

sudo apt-get install gnupg curl 

Create a list file for MongoDB 

Create the list file /etc/apt/sources.list.d/mongodb-org-7.0.list for your version of Ubuntu. 

Create the /etc/apt/sources.list.d/mongodb-org-7.0.list file for Ubuntu 22.04 (Jammy): 

echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list 

Create the /etc/apt/sources.list.d/mongodb-org-7.0.list file for Ubuntu 20.04 (Focal): 

echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list 

Reload local package database 

Issue the following command to reload the local package database: 

sudo apt-get update 

Install the MongoDB packages 

You can install either the latest stable version of MongoDB or a specific version of MongoDB. 

To install the latest stable version, issue the following 

sudo apt-get install -y mongodb-org 

Installation is completed. 

Start MongoDB 

You can start the mongod process by issuing the following command: 

sudo  systemctl start mongod 

If you receive an error similar to the following when starting mongod: 

Failed to start mongod.service: Unit mongod.service not found. 

Run the following command first: 

sudo  systemctl daemon-reload 

Then run the start command above again. 

Verify that MongoDB has started successfully. 

sudo  systemctl status mongod 

You can optionally ensure that MongoDB will start following a system reboot by issuing the following command: 

sudo  systemctl enable mongod 

Stop MongoDB. 

As needed, you can stop the mongod process by issuing the following command: 

sudo  systemctl stop mongod 

Restart MongoDB. 

You can restart the mongod process by issuing the following command: 

sudo  systemctl restart mongod 

You can follow the state of the process for errors or important messages by watching the output in the /var/log/mongodb/mongod.log file. 

Begin using MongoDB. 

Start a mongosh  session on the same host machine as the mongod. You can run mongosh  without any command-line options to connect to a mongod that is running on your localhost with default port 27017. 

mongosh 

Uninstall MongoDB Community Edition 

To completely remove MongoDB from a system, you must remove the MongoDB applications themselves, the configuration files, and any directories containing data and logs. The following section guides you through the necessary steps. 

Stop MongoDB. 

Stop the mongod process by issuing the following command: 

sudo service mongod stop 

Remove Packages. 

Remove any MongoDB packages that you had previously installed. 

sudo apt-get purge "mongodb-org*" 

Remove Data Directories. 

Remove MongoDB databases and log files. 

sudo rm -r /var/log/mongodb 

sudo rm -r /var/lib/mongodb
