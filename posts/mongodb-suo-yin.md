---
title: ' MongoDB: 索引'
date: 2020-04-28 15:38:24
tags: [MongoDB]
published: true
hideInList: false
feature: http://404j-images.test.upcdn.net/coverImage/mongo_index.svg
isTop: false
---
索引可以提升文档的查询速度，首先模拟不建立索引和建立索引之后的查询速度对比。
- 首先插入测试数据
```js
test.js:
for(let i = 0; i < 100000; i++) {
	db.person.insert({ name: 'test'+i ,age: NumberInt(i) })
}
```
```shell
mongo test test.js
```
- 未建立索引，执行查询语句
```js
> db.person.find({name: "test30000"}).explain("executionStats")

        "executionStats" : {
                "executionSuccess" : true,
                "nReturned" : 1,
                "executionTimeMillis" : 45,
                "totalKeysExamined" : 0,
                "totalDocsExamined" : 100000,
                "executionStages" : {
                        "stage" : "COLLSCAN",
                        "filter" : {
                                "name" : {
                                        "$eq" : "test30000"
                                }
                        },
                        "nReturned" : 1,
                        "executionTimeMillisEstimate" : 42,
                        "works" : 100002,
                        "advanced" : 1,
                        "needTime" : 100000,
                        "needYield" : 0,
                        "saveState" : 781,
                        "restoreState" : 781,
                        "isEOF" : 1,
                        "invalidates" : 0,
                        "direction" : "forward",
                        "docsExamined" : 100000
                }
        }
}
```
> 可以看到，未建立索引执行查询条件未`name`的查询时候，执行时间为`"executionTimeMillis" : 45`, 扫描方式为`"stage" : "COLLSCAN"`，即全表扫描
- 建立索引，然后执行查询
```js
> db.person.createIndex( { name: 1 } )

        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 1,
        "numIndexesAfter" : 2,
        "ok" : 1
}
```
```js
>  db.person.find({name: "test30000"}).explain("executionStats")

        "executionStats" : {
                "executionSuccess" : true,
                "nReturned" : 1,
                "executionTimeMillis" : 18,
                "totalKeysExamined" : 1,
                "totalDocsExamined" : 1,
                "executionStages" : {
                        "stage" : "FETCH",
                        "nReturned" : 1,
                        "executionTimeMillisEstimate" : 0,
                        "works" : 2,
                        "advanced" : 1,
                        "needTime" : 0,
                        "needYield" : 0,
                        "saveState" : 0,
                        "restoreState" : 0,
                        "isEOF" : 1,
                        "invalidates" : 0,
                        "docsExamined" : 1,
                        "alreadyHasObj" : 0,
                        "inputStage" : {
                                "stage" : "IXSCAN",
                                "nReturned" : 1,
                                "executionTimeMillisEstimate" : 0,
                                "works" : 2,
                                "advanced" : 1,
                                "needTime" : 0,
                                "needYield" : 0,
                                "saveState" : 0,
                                "restoreState" : 0,
                                "isEOF" : 1,
                                "invalidates" : 0,
                                "keyPattern" : {
                                        "name" : 1
                                },
                                "indexName" : "name_1",
                                "isMultiKey" : false,
                                "multiKeyPaths" : {
                                        "name" : [ ]
                                },
                                "isUnique" : false,
                                "isSparse" : false,
                                "isPartial" : false,
                                "indexVersion" : 2,
                                "direction" : "forward",
                                "indexBounds" : {
                                        "name" : [
                                                "[\"test30000\", \"test30000\"]"
                                        ]
                                },
                                "keysExamined" : 1,
                                "seeks" : 1,
                                "dupsTested" : 0,
                                "dupsDropped" : 0,
                                "seenInvalidated" : 0
                        }
                }
        }
}
```
> 建立索引后，执行时间为`"executionTimeMillis" : 18`, 扫描方式为`IXSCAN`，即从索引中查找。

通过对比可以看出，建立索引可以提升查询的速度。接下来记录为何索引可以提升文档查询的速度。
--- 
### 索引的原理
当执行插入语句的时候，每个文档经过数据库的存储引擎持久化后，会有一个位置信息，通过这个位置信息，就能从存储引擎里读出该文档。下表大致描述：
| 位置 | 文档 |
| ----------------------------- |  ----------------------------- |
| pos0 | {"name" : "leo", "score" : 38} |  
| pos1 | {"name" : "yanina", "score" : 24} |  
| pos2 | {"name" : "mars", "score" : 23} |  
| pos3 | {"name" : "dale", "score" : 22} |  
| pos4 | {"name" : "baby", "score" : 2} |  
...
此时如果要执行查询语句`db.person.find({score: 23})`，则需要全盘扫描`person` 集合，当满足条件`score: 23`时，得到文档的位置信息`pos2`，进而取出相关文档
当给字段`score`添加索引后（`db.person.createIndex( { score: 1 } )`），MongoDB会额外存储一份按`score`字段升序排序的索引数据，索引结构类似如下:
| age | 位置 |
| ----------------------------- |  ----------------------------- |
| 2 | pos4 |  
| 22 | pos3 |  
| 23 | pos2 |  
| 24 | pos1 |  
| 38 | pos0 |  
...
简单的说，索引就是将文档按照某个（或某些）字段顺序组织起来，以便能根据该字段高效的查询。此时执行查询语句`db.person.find({score: 23})`，只会扫描部分文档，得到位置信息则可取出文档。
或者来张图片更加易懂？
![](http://404j.fun/post-images/1588065623029.svg)

---
参考文档：
[MongoDB索引原理](https://mongoing.com/archives/2797)