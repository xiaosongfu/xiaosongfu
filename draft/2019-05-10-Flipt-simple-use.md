

```
docker run -d --name flipt \
    -p 8080:8080 \
    -p 9000:9000 \
    -v $HOME/flipt:/var/opt/flipt \
    markphelps/flipt:latest
```



```
docker run -p 8080:80 -e SPEC_URL=https://api.example.com/openapi.json redocly/redoc
```