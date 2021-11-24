
## 1. 创建 MySQL容器
```
docker run --restart=always --name ea-mysql -p 3306:3306 -v /data/mysql:/var/lib/mysql -v /etc/mysql:/etc/mysql/conf.d -e TZ=Asia/Shanghai -e MYSQL_ROOT_PASSWORD=123456 -d  daocloud.io/mysql
```
- --restart 表示重启的策略
- -p 代表端口映射，格式为：宿主机映射端口:容器运行端口
- -e 代表添加环境变量，MYSQL_ROOT_PASSWORD是root用户的登陆密码
- -v 代表绑定挂载目录，格式为：宿主机目录:容器目录  
- - `/data/mysql:/var/lib/mysql` 表示将容器里面 `/var/lib/mysql` 的数据挂载到宿主机 `/data/mysql` 目录下；
- - `/etc/mysql:/etc/mysql/conf.d` 表示将容器目录为 `/etc/mysql/conf.d` 挂载到宿主机 `/etc/mysql` 目录，mysql 容器启动时会加载宿主机 `/etc/mysql` 目录下的所以 `.cnf` 为后缀的文件并覆盖容器里 `/etc/mysql/my.cnf` 文件中的配置。
    
#### 重启策略参数
- no，默认策略，在容器退出时不重启容器
- on-failure，在容器非正常退出时（退出状态非0），才会重启容器
- on-failure:3，在容器非正常退出时重启容器，最多重启3次
- always，在容器退出时总是重启容器
- unless-stopped，在容器退出时总是重启容器，但是不考虑在Docker守护进程启动时就已经停止了的容器

## 2. 进入 MySQL 容器,登陆 MySQL

```
# 进入 mysql 容器
docker exec -it ea-mysql /bin/bash

# 登录 mysql
mysql -u root -p
```

## 3. 配置 MySQL
在 `/etc/mysql` 目录下创建 `my.cnf` 文件并添加如下配置
```
[mysqld]
character-set-server=utf8
sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION
max_allowed_packet=4194304
default-time_zone = '+8:00'

[client]
default-character-set=utf8

[mysql]
default-character-set=utf8
```

## 4. 导入数据库
1. 上传 `rz schema.sql` 文件到宿主机 `/tmp` 目录中;
2. `docker ps` 查看 mysql `CONTAINER ID`;
3. 将宿主机文件拷贝到 docker 容器中 `docker cp /tmp/schema.sql [CONTAINER ID]:/tmp/schema.sql`
2. 进入 MySQL 容器,登陆 MySQL;
3. 执行 `source /tmp/schema.sql`

## 5. 常见问题

#### （1）客户端连接不上，Navicat 报 1251 或 SQLyog 报 2058 错误 或 linux mysql client 2059 错误。
解决方式：更改加密规则以及更新root用户密码。原因是客户端只支持旧版的加密。

```mysql
# 1.容器中登录 mysql,查看 mysql 的版本
mysql> status;

# 2.进行授权远程连接(注意mysql 8.0跟之前的授权方式不同)
mysql> GRANT ALL ON *.* TO 'root'@'%';
mysql> flush privileges;

# 3.更改加密规则
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER;

# 4.更新root用户密码
mysql> ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
mysql> flush privileges;
```
#### （2）sql_mode 问题。
##### 1. 先查询 sql_mode 的配置
```mysql
mysql> SELECT @@sql_mode;
```
##### 2. 去掉 `ONLY_FULL_GROUP_BY` 后在 mysql 配置文件 `[mysqld]` 下配置属性,例如：
```text
sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION
```
##### 3. mysql 中文乱码问题
首先通过 `show variables like '%char%';` 命令查看当前编码；
然后在配置文件 `my.cnf` 中配置如下编码格式：
```text
[mysqld]
character-set-server=utf8

[client]
default-character-set=utf8

[mysql]
default-character-set=utf8
```
最后通过 `service mysql restart` 命令重启即可。

##### 4. client 连接出现 error 2003
- 检查 mysql server 服务是否已启动；
- 检查 防火墙或云服务器的安全组是否允许 mysql 3306 端口的进出。


tbl.rename(columns = {'序号':'serial_number', '招股书':'zhaogushu', '公司财报':'financial_report', '行业分类':'industry_classification', '产品类型':'industry_type', '主营业务':'main_business'},inplace = True)

##### 5. Got a packet bigger than 'max_allowed_packet' bytes
业务数据转换成 byte 数组，然后存入数据库类型为 mediumblob 的字段中，由于单个 SQL STATEMENT 的大小超过 `max_allowed_packet` 而出现的问题。

解决方法：
1. 使用 `show VARIABLES like '%max_allowed_packet%';` 命令查看 `max_allowed_packet` 大小；
1. 在配置文件 `my.cnf` 中配置 `max_allowed_packet` ：

```
# 设置数据报大小为 4M
max_allowed_packet=4194304
```

##### 6. 写入时间与实际保存在 MySQL 的时间相差 8 小时
1. 在配置文件 `my.cnf` 中 `[mysqld]` 下配置 `default-time_zone` ：
```
default-time_zone = '+8:00'
```
