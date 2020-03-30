---
title: 'Mongodb 用户管理'
date: 2020-03-28 13:14:14
tags: [MongoDB]
published: true
hideInList: false
feature: 
isTop: false
---
**谨以此篇blog记录server的mongoDB被洗空😖~~~**

### 1. admin db创建超级管理员用户
- 首先进入MongoDB shell
```
mongo
```
- 此时默认进入的test数据库，切换到admin数据库。关于admin数据库在mongoDB中的作用，
stackoverflow上的一位网友讲的听清楚，在此引用一下：*The main purpose of this admin database is to store system collections and user authentication and authorization data, which includes the administrator and user's usernames, passwords, and roles. Access is limited to only to administrators, who have the ability to create, update, and delete users and assign roles.*
```
use admin
```
- 创建Administrator
```
db.createUser({
    user: <username>,
    pwd: <password>,
    roles: [{
        role: <role>, db: <targetDB> // role一般指定为"userAdminAnyDatabase"
    }]
})  
```
> 以下为mongoDB内置的role

| 分类                          | role(角色) | 简要说明 |
| ----------------------------- | ---------- | -------- |
| 数据库用户角色(DB User Roles) |    `read` `readWrite`        |  为某个数据库创建一个用户, 分配该数据库的读写权力        |
| 数据库管理员角色(DB Admin Roles) | `dbAdmin` `dbOwner` `userAdmin` | 拥有创建数据库, 和创建用户的权力 |
| 集群管理角色(Culster Administration Roles) | `clusterAdmin` `clusterManager` `clusterMonitor` `hostManager` | 管理员组, 针对整个系统进行管理 |
| 备份还原角色(Backup and Restoration Roles) | `backup` `restore` | 备份数据库, 还原数据库 |
| 所有数据库角色(All-Database Roles) | `readAnyDatabase` `readWriteAnyDatabase` `userAdminAnyDatabase` `dbAdminAnyDatabase` | 拥有对admin操作的权限 |
| Superuser Roles(超级管理员) | `root` `dbOwner` `userAdmin` `userAdminAnyDatabase` | 这几个角色角色提供了任何数据任何用户的任何权限的能力，拥有这个角色的用户可以在任何数据库上定义它们自己的权限 |

### 2. 重启mongo服务，开启认证
- 停止服务
```
service mongodb stop
```
- 修改配置文件
```
whereis mongodb.conf // 查找配置文件
auth = true // 配置文件mongodb.conf 去掉 auth = true 前面的注释
```
- 重启服务
```
service mongodb start
or
mongod -f mongodb.conf
```

### 3. 认证方式登录mongo
- 登录
```
mongo
use admin
db.auth(<userName>, <password>)
show users // 查看用户
```

### 4. 创建指定db 用户
- 认证登录admin后，切换至指定的db, 以管理员身份创建其他用户
```
use <targetDB>
db.createUser({
    user: <username>,
    pwd: <password>,
    roles: [{
        role: <role>, db: <targetDB> // role一般指定为"readWrite"
    }]
})
show users // 可以通过 _id确定用户所指定的数据库
```

### 5. 修改用户密码及删除用户
- 首先以管理员身份登录admin，然后进行修改一般用户密码操作
```
use <targetDB> // 切换到待修改用户所在的db, 否则 user note found error
db.changeUserPassword(<userName>, <password>)
```
- 删除一般用户操作
```
use <targetDB> // 切换到待删除用户所在的db, 否则 user note found error
db.dropUser(<userName>)
```