

## [docker redis](https://hub.docker.com/r/bitnami/redis/)


## 第一次运行时设置服务器密码
```
docker run --name redis -e REDIS_PASSWORD=password123 bitnami/redis:latest
```

或
```
version: '2'

services:
  redis:
    image: 'bitnami/redis:latest'
    ports:
      - '6379:6379'
    environment:
      - REDIS_PASSWORD=password123
    volumes:
      - '/Users/qingyang/Desktop/aoki/docker/volume/redis:/bitnami'
```

## RUN
```
docker-compose up -d
```