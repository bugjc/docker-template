## 方式一
#### 1、BUILD IMAGE
```
git clone https://github.com/hteen/docker-ngrok.git
cd docker-ngrok
docker build -t hteen/ngrok .
```
#### 2、RUN
you must mount your folder (E.g /data/ngrok) to container /myfiles
if it is the first run, it will generate the binaries file and CA in your floder /data/ngrok
```
docker run -idt --restart=always --name ngrok-server \
-v /data/ngrok:/myfiles \
-p 8082:80 \
-p 4432:443 \
-p 4443:4443 \
-e DOMAIN='tunnel.bugjc.com' hteen/ngrok /bin/sh /server.sh

```

#### 3、配置nginx转发
```
server {
     listen       80;
     server_name  tunnel.bugjc.com *.tunnel.bugjc.com;
     location / {
             proxy_redirect off;
             proxy_set_header Host $host;
             proxy_set_header X-Real-IP $remote_addr;
             proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
             proxy_pass http://10.66.156.5:8082;
     }
 }
 server {
     listen       443;
     server_name  tunnel.bugjc.com *.tunnel.bugjc.com;
     location / {
             proxy_redirect off;
             proxy_set_header Host $host;
             proxy_set_header X-Real-IP $remote_addr;
             proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
             proxy_pass http://10.66.156.5:4432;
     }
 }
```


## 方式二
运行docker-compose-ssl-nginx.yml


## 客户端配置
创建一个ngrok.cfg配置文件：
```
server_addr: "tunnel.bugjc.com:4443"
trust_host_root_certs: false
```