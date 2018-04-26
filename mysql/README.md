
#### RUN

```
docker volume create --opt type=none --opt device=/opt/mysql/conf.d --opt o=bind --name=conf.d
docker run -p 3306:3306 --name mysql -v conf.d:/etc/mysql/conf.d -v data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=javadev -d mysql:latest
```

