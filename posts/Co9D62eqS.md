---
title: 'MongoDB: 基础命令'
date: 2020-03-17 17:31:51
tags: [MongoDB]
published: true
hideInList: false
feature: http://404j-images.test.upcdn.net/coverImage/image_1.png
isTop: false
---
### **mongo[🍃](https://docs.mongodb.com/manual/reference/program/mongo/)**
* run mongo shell with a connection string
```shell
mongo "mongodb://<username>:<password>@<host>:<port>/<db name>" <js file>
```
* run mongo shell with various command-line options
```shell
mongo -u <username> -p <password> --host <host> --port <port>  <js file>
```

---
### **mongodump[🍀](https://docs.mongodb.com/manual/reference/program/mongodump/)**
```shell
mongodump -h <ip>:<port> -d <db name> -u <userName> -p <pwd> -c <collection> -o <dump dir>
```
*Also can run mongodump shell with --uri*
```shell
mongodump --uri "mongodb://<username>:<password>@<host>:<port>/<db name>" [additional options]
```

---
### **mongorestore[🌴](https://docs.mongodb.com/manual/reference/program/mongorestore/)** 
```shell
mongorestore [options] [<directory>/<BSON file>]
```
*Part of options is same as [mongodump](#mongodump)*

---
### **mongoexport[☘️](https://docs.mongodb.com/manual/reference/program/mongoexport/)**
```shell
mongoexport -u <username> -p <password> --host <host> --port <port> -d <db name>  -c <collection> --out <json/csv file>
```
*Also can run mongoexport shell with --uri*

---
### **mongoimport[🍂](https://docs.mongodb.com/manual/reference/program/mongoimport/)**
```shell
mongoimport -u <username> -p <password> --host <host> --port <port> -d <db name>  -c <collection> --file <json/csv file>
```
*Also can run mongoimport shell with --uri*