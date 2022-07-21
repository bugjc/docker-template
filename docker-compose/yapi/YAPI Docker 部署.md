[项目地址](https://github.com/fjc0k/docker-YApi)

## 1. 先决条件
安装 [Docker](https://docs.docker.com/engine/install/centos/)[link](https://note.youdao.com/) 和 [Docker Compose](https://docs.docker.com/compose/install/)

## 2. 安装与访问
1. 克隆项目

```
git clone https://github.com/fjc0k/docker-YApi.git
```

2. 修改 docker-compose.ymal 文件的环境变量

```
version: '3'

services:
  yapi-web:
    image: jayfong/yapi:latest
    container_name: yapi-web
    ports:
      - 40001:3000
    environment:
      - YAPI_ADMIN_ACCOUNT=admin@ugiant.yapi
      - YAPI_ADMIN_PASSWORD=admin123456
      - YAPI_CLOSE_REGISTER=true
      - YAPI_DB_SERVERNAME=yapi-mongo
      - YAPI_DB_PORT=27017
      - YAPI_DB_DATABASE=yapi
      - YAPI_MAIL_ENABLE=false
      - YAPI_LDAP_LOGIN_ENABLE=false
      - YAPI_PLUGINS=[]
    depends_on:
      - yapi-mongo
    links:
      - yapi-mongo
    restart: unless-stopped
  yapi-mongo:
    image: mongo:latest
    container_name: yapi-mongo
    volumes:
      - ./data/db:/data/db
    expose:
      - 27017
    restart: unless-stopped
```

3. 启动服务

```
# 启动
docker-compose up -d
# 重启
docker-compose restart yapi-web
# 查看日志
docker-compose logs yapi-web
```

4. 访问

```
http://localhost:40001
```

## 3. 配置
在 docker-compose.ymal 文件中增加如下配置：
```
./config.json:/yapi/config.json
./config.js:/yapi/config.js
```


## 4. 升级

```
docker-compose pull yapi-web \
  && docker-compose down \
  && docker-compose up -d
```

## 5. 迁移
直接打包整个目录复制上传到新的服务器，然后启动即可。注意：最好原服务器的数据目录和 YAPI 同级目录，这样，迁移打包会更有效率。


```
└─docker-YApi
    ├─docker-compose.yml        // docker-compose 文件
    ├─data                      //数据存储文件目录
    └─...                      

```
