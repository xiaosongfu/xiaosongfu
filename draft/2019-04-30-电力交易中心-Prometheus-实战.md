
```
docker run --name prometheus-server -d -p 9090:9090 \
  -v /root/prometheus/prometheus-2.9.1.linux-amd64/my-prometheus.yml:/etc/prometheus/prometheus.yml \
  prom/prometheus
```


```
docker run --name grafana-server -d -p 3000:3000 grafana/grafana
```
