---
title: 'MongoDB: 基础命令'
date: 2020-03-17 17:31:51
tags: [MongoDB]
published: true
hideInList: false
feature: 
isTop: false
---
### **mongo**
```

```

### **mongodump**
```
mongodump -h [ip]:[port] -d [db name] -u [userName] -p [pwd] -o ./
```

### **mongorestore**
```
mongorestore -d [db name] -j 1 D:\databases\smarttransit_channel.tar\smarttransit_channel
```

### **mongoimport**
```
mongoimport --db smarttransit --collection cardbins --file target.json
```