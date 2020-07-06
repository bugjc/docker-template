
#### docker compose run

```
docker-compose up -d
```
#### docker run

```
docker run --name some-mysql -p 3306:3306 -v /my/own/datadir:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -d daocloud.io/mysql
```
