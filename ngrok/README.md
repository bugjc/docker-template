## BUILD IMAGE
```
git clone https://github.com/hteen/docker-ngrok.git
cd docker-ngrok
docker build -t hteen/ngrok .
```
## RUN
you must mount your folder (E.g /data/ngrok) to container /myfiles
if it is the first run, it will generate the binaries file and CA in your floder /data/ngrok
```
docker run -idt --restart=always --name ngrok-server \
-v /data/ngrok:/myfiles \
-p 8082:80 \
-p 4432:443 \
-p 4443:4443 \
-e DOMAIN='tunnel.bugjc.com' hteen/ngrok /bin/bash /server.sh

```