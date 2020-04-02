---
title: 'Linux 命令之文件操作'
date: 2020-04-02 09:03:34
tags: [Linux]
published: true
hideInList: false
feature: 
isTop: false
---
*记录下常用或者不常用，会用或者不会用的 Linux 文件操作命令*
---
## 目录
[dsf](#新建)
---
#### 新建
```
mkdir <folderName> // 新建文件夹
touch <fileName> // 新建文件
```
---
#### 查看
- 查看目录
```
ll or ls -l // 显示目录文件详细信息
du -h <fileName or folderName> // 查看文件或者文件夹的大小
pwd // 显示当前路径
```
- 查看文件内容
```
cat <fileName> // 查看文件的全部内容
head -<headLine> <fileName> // 查看文件前几行的内容
tail -n <startToTail> -f <fileName> // 显示文件后几行的内容
more <fileName> // 显示一屏文件内容。Space --> 下一屏内容; Enter --> 下一行内容; Q --> 退出
less <fileName> // 类似于more，值得研究下
🐖：more和less可以结合管道命令：history | more; ps -ef | more
```
- 查看文件状态
```
stat <fileName> // 查看文件详细信息
```
- 查看文件类型
```
file <fileName> // 查看文件类型
```
---
#### 删除
```
rm -f -i -r <fileName> // f --> force强制删除; i --> interactive交互式删除; r --> recursive递归删除文件夹
```
---
#### 复制

