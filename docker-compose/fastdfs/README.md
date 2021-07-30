# 文件目录
```
├── docker-compose.yaml
├── nginx
│   └── nginx.conf
├── storage
│   └── data
└── tracker
│   └── conf
│       └── client.conf
└── store_path
```

## 使用步骤
1. 修改 `./tracker/conf/client.conf` 文件里面的`tracker_server` 属性值；
2. 修改 `./docker-compose.yaml` 文件里面的两个 `TRACKER_SERVER` 属性值；
3. 在 `docker-compose.yml ` 文件所在目录下执行如下命令：
    ```
    docker-compose up -d
    ```
[link](https://www.cnblogs.com/edoclin/p/14648832.html)