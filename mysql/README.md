
#### docker compose run

```
docker-compose up -d
```
#### docker run

```
docker run --name some-mysql -v /my/own/datadir:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d daocloud.io/mysql
```