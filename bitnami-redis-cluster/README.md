#



## 步驟

1. 取電腦IP
```
ifconfig ens33 | grep 'inet' | cut -d: -f2 | awk '{print $2}'
```

2. 設到.env中
```
DOCKER_COMPOSE_MY_IPV4_IP=192.168.48.128
```

3. 跑docker compose
```
docker-compose up -d
```

4. 檢查cluster node
```
redis-cli -p 6379 -a bitnami -c
redis-cli -p 6380 -a bitnami -c
redis-cli -p 6381 -a bitnami -c
redis-cli -p 6382 -a bitnami -c
redis-cli -p 6383 -a bitnami -c
redis-cli -p 6384 -a bitnami -c
```

5. 把noaddr的node補上去
```
docker inspect -f '{{.Name}} - {{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $(docker ps -q)

redis-cli -p 6380 -a pwd123 -c

cluster meet 172.21.0.2 6379 
cluster meet 172.21.0.3 6379 
cluster meet 172.21.0.4 6379 
```

