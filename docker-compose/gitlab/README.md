## 使用步骤
1. 修改 `docker-compose.yml ` 文件内容。
2. 在 `docker-compose.yml ` 文件所在目录下执行如下命令：
    ```
    docker-compose up -d
    ```
[link](https://github.com/sameersbn/docker-gitlab)

## 常见问题
- 重启失败，删除 docker 容器（docker ps）,在重新启动一个实例（前提是容器数据有映射到宿主机）。
