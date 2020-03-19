---
title: 'MongoDB: 基础命令'
date: 2020-03-17 17:31:51
tags: [MongoDB]
published: true
hideInList: false
feature: 
isTop: false
---
### **mongo[🍃](https://docs.mongodb.com/manual/reference/program/mongo/)**
* run mongo shell with a connection string
```
mongo "mongodb://<username>:<password>@<host>:<port>/<db name>" <js file>
```
* run mongo shell with various command-line options
```
mongo -u <username> -p <password> --host <host> --port <port>  <js file>
```

### **mongodump[🍀](https://docs.mongodb.com/manual/reference/program/mongodump/)**
```
mongodump -h <ip>:<port> -d <db name> -u <userName> -p <pwd> -c <collection> -o <dump dir>
```
*Also can run mongodump shell with --uri*
```
mongodump --uri "mongodb://<username>:<password>@<host>:<port>/<db name>" [additional options]
```


### **mongorestore**
```

```

### **mongoexport**
```

```

### **mongoimport**
```

```