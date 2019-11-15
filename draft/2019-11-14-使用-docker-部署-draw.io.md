draw.io 官方提供了 docker 镜像，可以很方便的部署：

```
docker run -it --rm --name="draw" -p 8080:8080 -p 8443:8443 jgraph/draw.io
```

提供了以下环境变量：

* LETS_ENCRYPT_ENABLED: Enables Let's Encrypt certificate instead of self-signed; default false
* PUBLIC_DNS: DNS domain to be used as certificate "CN" record; default draw.example.com
* ORGANISATION_UNIT: Organisation unit to be used as certificate "OU" record; default Cloud Native Application
* ORGANISATION: Organisation name to be used as certificate "O" record; default example inc
* CITY: City name to be used as certificate "L" record; default Paris
* STATE: State name to be used as certificate "ST" record; default Paris
* COUNTRY_CODE: Country code to be used as certificate "C" record; default FR
* KEYSTORE_PASS: ".keystore"/.jks" store password; default V3ry1nS3cur3P4ssw0rd
* KEY_PASS: Private key password; default <ref:KEYSTORE_PASS>

官方还提供了基于 Electron 构建的桌面版本：https://github.com/jgraph/drawio-desktop

https://github.com/jgraph/drawio
https://github.com/jgraph/docker-drawio
https://github.com/jgraph/drawio-desktop
