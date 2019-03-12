## 1. 先搭建 mysql

如果没有配置 AWS S3，构建日志文件这些都是保存在默认的 sqlite 数据库里面的，可能会有性能问题，所有建议单独部署 mysql 服务器来搭配使用。

Drone 支持 mysql 5.6及更高版本作为数据库引擎。

参考：https://docs.drone.io/administration/server/database/

```
docker run -d --name drone-mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=drone mysql:5.7
```

## 2. 部署 Central Drone Server

##### 2.1 创建共享密钥

```
$ openssl rand -hex 16
```

##### 2.2 开始部署

！！记得配置使用刚刚搭建的 mysql 数据库。

```
$ docker run -d \
  --name=drone \
  --volume=/var/lib/drone:/data \
  --env=DRONE_GIT_ALWAYS_AUTH=false \
  --env=DRONE_GITLAB_SERVER=http://git.ibdbots.com \
  --env=DRONE_GITLAB_CLIENT_ID=1eca36316a92c00967b69d57c55196885ed1f1de2f53189a4272d516d2d5675d \
  --env=DRONE_GITLAB_CLIENT_SECRET=b88318a113d215bebf90dcda7b541bce93718a68cb8e7e50505892f353720fd5 \
  --env=DRONE_RUNNER_CAPACITY=2 \
  --env=DRONE_SERVER_HOST=drone.xiaosongfu.com \
  --env=DRONE_SERVER_PROTO=https \
  --env=DRONE_TLS_AUTOCERT=true \
  --env=DRONE_RPC_SECRET=bea26a2221fd8090ea38720fc445eca6 \
  --env=DRONE_DATABASE_DRIVER=mysql \
  --env=DRONE_DATABASE_DATASOURCE=root:drone@tcp(localhost:3306)/drone?parseTime=true \
  --publish=80:80 \
  --publish=443:443 \
  --restart=always \
  --detach=true \
  drone/drone:1.0.0-rc.5
```

## 3. 部署 Agent

```
$ docker run -d \
  --name=drone-agent \
  --volume=/var/run/docker.sock:/var/run/docker.sock \
  --env=DRONE_RPC_SERVER=drone.xiaosongfu.com \
  --env=DRONE_RPC_SECRET=bea26a2221fd8090ea38720fc445eca6 \
  --env=DRONE_RUNNER_CAPACITY=2 \
  --env=DRONE_RUNNER_NAME=drone-agent-us \
  --restart=always \
  --detach=true \
  drone/agent:1.0.0-rc.5
```

## 4. 改进意见

构建日志文件这些都是保存在数据库里面的，建议保存到 AWS S3。可以使用 Minio 代替吗？



