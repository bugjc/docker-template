
#### RUN

```
docker run -p 3306:3306 --name mysql -v conf.d:/etc/mysql/conf.d -v data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=javadev -d mysql:latest
```

