## 1.新建文档

在 content/archives 目录下生成 post.md 文档：`hugo new archives/post.md`  

## 2.启动服务

```
hugo server
```

## 3.部署

```
# 删除 docs 目录
rm -rf docs

# 生成静态文件，保存到 docs 目录
hugo -d docs

# 复制 CNAME 文件
cp CNAME docs/
```