# RabbitMQ dockerized

## Usage
Open [http://localhost:15672/](http://localhost:15672/)

```
open http://localhost:15672/
```

and use the login

```
username: rabbitmq
password: rabbitmq
```

---------

```
docker run -d --name some-rabbit \
-v /data/rabbitmq:/var/lib/rabbitmq \
-p 15672:15672 \
-p 5672:5672 \
daocloud.io/rabbitmq:3-management
```