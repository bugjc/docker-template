

#### Create volume _vol-mssql_.
```
mkdir -p /home/mssql/mssql-vol/
docker volume create --opt type=none --opt device=/home/mssql/mssql-vol --opt o=bind --name=mssql-vol
```
or
```
docker volume create --name=mssql-vol
```
**The default path is _"/var/lib/docker/volumes/mssql-vol"_.**

#### Remove _vol-mssql_ volume.

```
docker volume rm vol-mssql
```

#### Remove _vol-mssql_ folder.

```
sudo rm -Rf /home/mssql/vol-mssql
```

