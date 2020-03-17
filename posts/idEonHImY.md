---
title: 'Git: 基础命令'
date: 2020-03-17 17:17:46
tags: [Git]
published: true
hideInList: false
feature: 
isTop: false
---
### **远端代码clone到本地**
```
git clone -b [branch name] [remote repository address]
```

### **本地新建分支**
```
git branch [branch name]
git checkout [branch name]
```

### **删除本地分支**
```
git branch -D [branch name]
```

### **删除远端分支**
```
git push origin --delete [branch name]
```

### **rebase流程**
```
先在自己的分支上执行，git fetch
然后在自己的分支上执行 git rebase origin/develop
有冲突解决冲突, 解决完冲突 git add -A
git rebase --continue
推送到远端 git push origin [remote branch name] -f
```

### **合并多个远程commit提交**
```

```

### **更改本地分支的名字**
```
git checkout [old name]
git branch -m [new name]
```

### **将本地分支推送到远端并关联**
```
git push --set-upstream origin [remote branch name]
```

### **将某次commit应用到某一分支**
```
git cherry-pick [commit id]
```

### **将远端分支拉取到本地**
```
git fetch && git checkout [remote branch name] 
```