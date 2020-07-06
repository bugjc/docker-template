## docker run
```
docker run --name some-nginx -p 8080:80 -v /Users/qingyang/Desktop/aoki/github/docker-compose-templete/nginx/conf/nginx.conf:/etc/nginx/nginx.conf:ro -d daocloud.io/nginx
```

## docker compose run
```
docker-compose up -d
```