---
layout: post
title: MongoDB Quickstart
mathjax: false
related: false
comments: true
published: true
---


_Created: Oct, 2015_

_Last modified: Sep, 2016_


## General Concepts

A list of MongoDB glossaries can be found [here](https://docs.mongodb.com/manual/reference/glossary). But at its minimum, there are three terms: [database](https://docs.mongodb.com/manual/reference/glossary/#term-database), [collection](https://docs.mongodb.com/manual/reference/glossary/#term-database) (like the "table" in a [RBDMS](https://en.wikipedia.org/wiki/Relational_database_management_system)) and [document](https://docs.mongodb.com/manual/reference/glossary/#term-document) (the unit of a database, like the row in a RBDMS). For example: 

* A MongoDB server can have multiple databases, one database can have multiple collections, and a collection can have multiple documents. The the shell commands below to have a grasp of what it looks like. 

* MongodDB is schema-less (a feature of NoSQL), meaning that the documents in a collection don't have to have the same structure. 

Another feature of MongoDB is that a database or collection can be created on the fly, but its existence can only come into being when the first document under it is created. 

## Setting It Up

* Install MongoDB on Ubuntu: [doc from mongodb.org](http://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/)

* MongoDB failed to start on Ubuntu, "errno:98 Address already in use for socket: 0.0.0.0:27017 ": restart mongod on Ubuntu to release the occupied port: 

```
sudo service mongod restart
```

* MongoDB needs space of > 3GB by default, or it will fail to start. In that case (for small VMs), start it with "--smallfiles" option:

```
mongod --smallfiles --dbpath /path/to/database
```

## Quick-start MongoDB Shell Commands 

* To start MongoDB console/shell after the daemon mongod is started, do: 

```
mongo
```

* In the MongoDB shell, to list all databases, do: 

```
show dbs
```

* To create a new database or switch to another database named "datadump", do

```
use datadump
```

Note that if it is a new database "datadump", it will not be created until the first record is inserted below.

At this point, the shell variable "db" will be referenced to "datadump" (also see below). 

* To list all collections of the current database, do 

```
show collections
```

* To insert a document to the current database, do: 

```
db.mycollection1.insert({"title": "user1", "email": "user1@example.com"})
```

this will create a collection named "mycollection1" if it hasn't be created, and add a document to this collection. From this point, this collection can be referenced as "db.mycollection1" in the shell.  

* One can also create an array of records and insert like

```
newrec = [{"title": "user2", "email": "user2@example.com"}, {"title": "user3", "email": "user3@example.com"}]
db.mycollection1.insert(newrec)
```

* To list all documents in a collection, do 

```
db.mycollection1.find()
```

* To print the content of all documents in a clean JSON way

```
db.mycollection1.find().forEach(printjson)
```

* To remove all documents in a collection, do 

```
db.mycollection1.remove({})
```

A query is passed to the remove() function to match the target queries. See [query operator](https://docs.mongodb.com/master/reference/operator/) for details. 

Note that even all documents in a collection are removed, the collection still exists, and show up in the results of `show collections`.

* To drop a collection (even if it is non-empty), do 

```
db.mycollection1.drop()
```

This will decommission a collection from a database, making it disappear in the results of `show collections`.

* To drop the current database, do 

```
db.dropDatabase()
```

* Press Ctrl+"d" to quit the console. 

* A list of commonly used MongoDB shell commands is [here](http://docs.mongodb.org/master/reference/mongo-shell/).


## Using It with Python

MongoDB has a [Python driver](https://docs.mongodb.org/getting-started/python/) called [PyMongo](https://docs.mongodb.org/ecosystem/drivers/python/). First initiate a Virtualenv container: 

```
virtualenv pymongo_env
. ./pymongo_env/bin/activate
pip install --upgrade pip
```

And install dependencies and pymongo

```
sudo apt-get install python-dev libffi-dev libssl-dev 
pip install requests[security]
pip install pymongo
```

Otherwise, there will be "InsecurePlatformWarning" after installing pymongo (see [this post](http://stackoverflow.com/q/29134512)).  

Refer to [PyMongo documentation](https://docs.mongodb.org/getting-started/python/client/) and [tutorials](http://api.mongodb.org/python/current/tutorial.html) on the usage.  

Note that PyMongo is a blocking MongoDB driver, which may be fine for basic use. If non-blocking CRUD are needed, check out [Motor](http://motor.readthedocs.org/en/stable/). 


## Secure Remote Access of MongoDB

MongoDB listens only to localhost by default. One basic scenario is that I need to access a database running on Server A and listening 27017 from another Server B. I find the SSH tunneling method in [this post](https://www.digitalocean.com/community/tutorials/how-to-securely-configure-a-production-mongodb-server) a simple solution that is also secure enough. It can be further supplemented by [autossh](http://www.harding.motd.ca/autossh/) to keep the SSH tunnel alive. The following assumes the MongoDB on Server A uses default settings. 

First install autossh 

```
sudo apt-get install autossh
```

Now generate SSH public-private key pairs on Server B

```
ssh-keygen
```

and concatenate the public key on Server B to "~/.ssh/authorized_keys" on Server A. I leave the key passphrase empty for convenience. 

Create an autossh tunnel on Server B:

```
autossh -M 0 -fN -L 4321:localhost:27017 UserA@ServerA
```

The command above will create a tunnel between the port 4321 of localhost on Server B to the port 27017 of localhost on Server A. Autossh has many powerful options that need further reading. If the public key of Server B is not at "~/.ssh/id_rsa.pub", specify the correct path with `-i path_to_pub_key_server_b` option. 

Now, one can access the database on Server A  from Server B with port 4321 of localhost.
