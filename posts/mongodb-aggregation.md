---
title: 'MongoDB: Aggregation 简单使用'
date: 2020-05-13 18:09:17
tags: [MongoDB]
published: true
hideInList: false
feature: http://404j-images.test.upcdn.net/coverImage/mongodb-aggregation.jpg
isTop: false
---
> MongoDB的`aggretation pipeline`(聚合管道)查询：把查询分为多个 `stage`(阶段)，每个阶段的输入只依赖于上个阶段的输出。接下来记录一些基本的管道操作符用法。

### 导入样例数据
```json
person.json:
{"_id":{"$oid":"5ebfef770855d7b7e372f0f4"},"name":"Tom","age":25,"gender":1,"hobbies":["basketball","run","swim","eat"]}
{"_id":{"$oid":"5ebfefb10855d7b7e372f10d"},"name":"Jay","age":20,"gender":1,"hobbies":["football","swim","sing"]}
{"_id":{"$oid":"5ebfeffa0855d7b7e372f11a"},"name":"Hellen","age":15,"gender":0,"hobbies":["yoga","ride","sing"]}
{"_id":{"$oid":"5ebff02b0855d7b7e372f120"},"name":"Jack","age":20,"gender":1,"hobbies":["eat","basketball","sing"]}
{"_id":{"$oid":"5ebff0620855d7b7e372f12d"},"name":"Lucy","age":17,"gender":0,"hobbies":["sing","ride","program"]}

sales.json:
{"_id":{"$oid":"5ec0b2274bac27ea3ba384e1"},"item":"abc","price":{"$numberDecimal":"10"},"quantity":2,"date":{"$date":"2014-03-01T08:00:00Z"}}
{"_id":{"$oid":"5ec0b2274bac27ea3ba384e2"},"item":"jkl","price":{"$numberDecimal":"20"},"quantity":1,"date":{"$date":"2014-03-01T09:00:00Z"}}
{"_id":{"$oid":"5ec0b2274bac27ea3ba384e3"},"item":"xyz","price":{"$numberDecimal":"5"},"quantity":10,"date":{"$date":"2014-03-15T09:00:00Z"}}
{"_id":{"$oid":"5ec0b2274bac27ea3ba384e4"},"item":"xyz","price":{"$numberDecimal":"5"},"quantity":20,"date":{"$date":"2014-04-04T11:21:39.736Z"}}
{"_id":{"$oid":"5ec0b2274bac27ea3ba384e5"},"item":"abc","price":{"$numberDecimal":"10"},"quantity":10,"date":{"$date":"2014-04-04T21:23:13.331Z"}}
{"_id":{"$oid":"5ec0b2274bac27ea3ba384e6"},"item":"def","price":{"$numberDecimal":"7.5"},"quantity":5,"date":{"$date":"2015-06-04T05:08:13Z"}}
{"_id":{"$oid":"5ec0b2274bac27ea3ba384e7"},"item":"def","price":{"$numberDecimal":"7.5"},"quantity":10,"date":{"$date":"2015-09-10T08:43:00Z"}}
{"_id":{"$oid":"5ec0b2274bac27ea3ba384e8"},"item":"abc","price":{"$numberDecimal":"10"},"quantity":5,"date":{"$date":"2016-02-06T20:20:13Z"}}
```

### $match(筛选)
> 在person中查询age大于等于20,name为Tom或者Jay的记录
```shell
db.person.aggregate([
    {
        $match: {
            name: {
                $in: [ "Tom", "Jay" ]
            },
            age: {
                $gte: 20
            }
        }
    }
])
```
结果：
```js
{
        "_id" : ObjectId("5ebfef770855d7b7e372f0f4"),
        "name" : "Tom",
        "age" : 25,
        "gender" : 1,
        "hobbies" : [
                "basketball",
                "run",
                "swim",
                "eat"
        ]
},
{
        "_id" : ObjectId("5ebfefb10855d7b7e372f10d"),
        "name" : "Jay",
        "age" : 20,
        "gender" : 1,
        "hobbies" : [
                "football",
                "swim",
                "sing"
        ]
}
```
---
### $project(投影)
> 对name为Lucy的person文档进行重新组合
```shell
db.person.aggregate([
    {
        $match: {
            name: 'Lucy'
        }
    },
    {
        $project: {
            _id: 0,
            name: 1,
            age: 1,
            newArray: [ "$name", "$age" ],
            nameFirstWord: {
                $substr: [ "$name", 0, 1 ]
            },
            ageAddOne: {
                $add: ["$age", 1]
            },
            renameHobbies_interest: "$hobbies",
            subDocument_nameInfo: {
                name: "$name"
            }
        }
    }
])
```
结果：
```js
{
        "name" : "Lucy",
        "age" : 17,
        "newArray" : [
                "Lucy",
                17
        ],
        "nameFirstWord" : "L",
        "ageAddOne" : 18,
        "renameHobbies_interest" : [
                "sing",
                "ride",
                "program"
        ],
        "subDocument_nameInfo" : {
                "name" : "Lucy"
        }
}
```
---
### $limit(文档数量限制)
> 取出两个person数据
```shell
db.person.aggregate([
    {
        $limit: 2
    }
])
```
结果：
```js
{
        "_id" : ObjectId("5ebfef770855d7b7e372f0f4"),
        "name" : "Tom",
        "age" : 25,
        "gender" : 1,
        "hobbies" : [
                "basketball",
                "run",
                "swim",
                "eat"
        ]
},
{
        "_id" : ObjectId("5ebfefb10855d7b7e372f10d"),
        "name" : "Jay",
        "age" : 20,
        "gender" : 1,
        "hobbies" : [
                "football",
                "swim",
                "sing"
        ]
}
```
---
### $skip(跳过管道文档)
> 从第4个开始取出person数据
```shell
db.person.aggregate([
    {
        $skip: 3
    }
])
```
结果：
```js
{
        "_id" : ObjectId("5ebff02b0855d7b7e372f120"),
        "name" : "Jack",
        "age" : 20,
        "gender" : 1,
        "hobbies" : [
                "eat",
                "basketball",
                "sing"
        ]
},
{
        "_id" : ObjectId("5ebff0620855d7b7e372f12d"),
        "name" : "Lucy",
        "age" : 17,
        "gender" : 0,
        "hobbies" : [
                "sing",
                "ride",
                "program"
        ]
}
```
---
### $sort(排序)
> 分页取出person数据：按age降序，第2页，每页2个
```shell
db.person.aggregate([
    {
        $sort: {
            age: -1
        }
    },
    {
        $skip: 2
    },
    {
        $limit: 2
    }
])
```
结果：
```js
{
        "_id" : ObjectId("5ebff02b0855d7b7e372f120"),
        "name" : "Jack",
        "age" : 20,
        "gender" : 1,
        "hobbies" : [
                "eat",
                "basketball",
                "sing"
        ]
},
{
        "_id" : ObjectId("5ebff0620855d7b7e372f12d"),
        "name" : "Lucy",
        "age" : 17,
        "gender" : 0,
        "hobbies" : [
                "sing",
                "ride",
                "program"
        ]
}
```
---
### $unwind(展开数组)
> 对person中name为Jay的数据的hobbies进行展开
```shell
db.person.aggregate([
    {
        $match: {
            name: "Jay"
        }
    },
    {
        $unwind: "$hobbies"
    }
])
```
结果：
```js
{
        "_id" : ObjectId("5ebfefb10855d7b7e372f10d"),
        "name" : "Jay",
        "age" : 20,
        "gender" : 1,
        "hobbies" : "football"
},
{
        "_id" : ObjectId("5ebfefb10855d7b7e372f10d"),
        "name" : "Jay",
        "age" : 20,
        "gender" : 1,
        "hobbies" : "swim"
},
{
        "_id" : ObjectId("5ebfefb10855d7b7e372f10d"),
        "name" : "Jay",
        "age" : 20,
        "gender" : 1,
        "hobbies" : "sing"
}
```
---
### $group(分组)
> 对sales进行统计分析

 - 计算总的销售额
```shell
db.sales.aggregate([
    {
        $group: {
            _id: null,
            totalSaleAmount: {
                $sum: {
                    $multiply: [ "$price", "$quantity" ]
                }
            }
        }
    }
])
```
结果：
```js
{ "_id" : null, "totalSaleAmount" : NumberDecimal("452.5") }
```
- 根据item进行分组，统计每组的总的销售额，降序展示销售额大于100的数据
```shell
db.sales.aggregate([
    {
        $group: {
            _id: "$item",
            saleAmount: {
                $sum: {
                    $multiply: [ "$price", "$quantity" ]
                }
            }
        }
    },
    {
        $match: {
            saleAmount: {
                $gt: 100
            }
        }
    },
    {
        $sort: {
            saleAmount: -1
        }
    }
])
```
结果：
```js
{
        "_id" : "abc",
        "saleAmount" : NumberDecimal("170")
},
{
        "_id" : "xyz",
        "saleAmount" : NumberDecimal("150")
},
{
        "_id" : "def",
        "saleAmount" : NumberDecimal("112.5")
}
```
- 2015-01-01以后，根据日期（YYYY-MM-dd）分组，计算出每个分组的销售总额，平均销售数，文档条数。按销售总额升序输出
```shell
db.sales.aggregate([
    {
        $match: {
            date: {
                $gt: new ISODate("2015-01-01")
            }
        }
    },
    {
        $group: {
            _id: {
                $dateToString: {
                    format: "%Y-%m-%d",
                    date: "$date"
                }
            },
            totalSaleAmount: {
                $sum: {
                    $multiply: [ "$price", "$quantity" ]
                }
            },
            averageQuantity: {
                $avg: "$quantity"
            },
            count: {
                $sum: 1
            }
        }
    },
    {
        $sort: {
            totalSaleAmount: 1
        }
    }
])
```
结果：
```js
{
        "_id" : "2015-06-04",
        "totalSaleAmount" : NumberDecimal("37.5"),
        "averageQuantity" : 5,
        "count" : 1
},
{
        "_id" : "2016-02-06",
        "totalSaleAmount" : NumberDecimal("50"),
        "averageQuantity" : 5,
        "count" : 1
},
{
        "_id" : "2015-09-10",
        "totalSaleAmount" : NumberDecimal("75.0"),
        "averageQuantity" : 10,
        "count" : 1
}
```
- 按日期（YYYY）分组，统计每年一共的item类型（包含不去重和去重的结果）
```shell
db.sales.aggregate([
    {
        $group: {
            _id: {
                $dateToString: {
                    format: "%Y",
                    date: "$date"
                }
            },
            hasDupItems: {
                $push: "$item"
            },
            uniqueItems: {
                $addToSet: "$item"
            }
        }
    }
])
```
结果：
```js
{
        "_id" : "2016",
        "hasDupItems" : [
                "abc"
        ],
        "uniqueItems" : [
                "abc"
        ]
},
{
        "_id" : "2014",
        "hasDupItems" : [
                "abc",
                "jkl",
                "xyz",
                "xyz",
                "abc"
        ],
        "uniqueItems" : [
                "abc",
                "jkl",
                "xyz"
        ]
},
{
        "_id" : "2015",
        "hasDupItems" : [
                "def",
                "def"
        ],
        "uniqueItems" : [
                "def"
        ]
}
```
*OVER~*

参考文档：
    [MongoDB 聚合管道（Aggregation Pipeline）](https://blog.51cto.com/shanyou/1345924)
    [Official documents](https://docs.mongodb.com/manual/core/aggregation-pipeline/)

