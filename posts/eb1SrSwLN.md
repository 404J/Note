---
title: 'Linux 命令之文件操作'
date: 2020-04-02 09:03:34
tags: [Linux]
published: true
hideInList: false
feature: http://404j-images.test.upcdn.net/coverImage/hannah-joshua-46T6nVjRc2w-unsplash.jpg
isTop: false
---
*记录下常用或者不常用，会用或者不会用的 Linux 文件操作命令*
---
#### 新建
```
mkdir <folderName> // 新建文件夹
touch <fileName> // 新建文件
ln -s <source> <dest> // 创建文件软连接(穿个软连接)
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
- 本地复制
```
cp -r <source> <dest> // r --> 复制文件夹
```
- 远程复制
```
scp -r <localFile> <remoteUserName>@<remoteIp>:<remoteFolder> // 本地复制到远程，r --> 复制文件夹
scp -r <remoteUserName>@<remoteIp>:<remoteFolder> <localFile> // 远程复制到本地，r --> 复制文件夹
```
---
#### 移到 or 重命名
```
mv <source> <dest> // 移动 or 重命名 文件或文件夹
```
---
#### 归档 or 解压缩
- 压缩
```
tar -cvf <dest.tar> <source> // 归档成*.tar包, c --> 建立档案; v --> 显示过程; f --> 使用档案名字，必填且最后一个参数
tar -czvf <dest.tar.gz> <source> // 压缩成*.tar.gz文件, z --> 有gzip属性
tar -cjvf <dest.tar.bz2> <source> // 压缩成*.tar.bz2文件, j --> 有bz2属性
zip <dest.zip> <source> // 压缩成*.zip文件
```
- 解压
```
tar -xvf <dest.tar> // 展开*.tar包, x --> 解压
tar -xzvf <dest.tar.gz> // 解压*.tar.gz文件
tar -xjvf <dest.tar.gz> // 解压*.tar.bz2文件
unzip <dest.zip> // 解压*.zip文件
```
> 🐖：[tar vs zip vs gz](https://itsfoss.com/tar-vs-zip-vs-gz/)
> ---
#### 查找
```
find <targetFolders> -name <targetName> -type <b/d/c/p/l/f> // type -->块设备、目录、字符设备、管道、符号链接、普通文件
locate <targetName>
whereis <targetName>
```
