---
title: git submodule 简记
date: 2018-01-10 11:44:58
tags: []
categories: []
---

## 目录
>  
1、为项目添加 submodule   
2、检出


### 1、为项目添加 submodule  


```  
git submodule add http://foo.com/semi.fu/iBot.git submodules/ibotlib
```  

```  
fuxiaosongdeMac-mini:FengMapDemo fuxiaosong$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   .gitmodules
        new file:   submodules/ibotlib

```  

其中 “.gitmodules” 指定submodule的主要信息，包括子模块的路径和地址信息。  
其中 “new file:   submodules/ibotlib“ 并不是将 ibotlib 库的内容添加到当前的仓库里面，而是以 gitlink 方式添加了一个链接。  
.gitmodules 和 submodules/ibotlib 这两项是需要提交到父项目的远程仓库的。