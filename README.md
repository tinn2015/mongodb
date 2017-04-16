
### mongod的配置

mkdir data,log,conf,bin

```
cp mongod&mongo -> bin
vim conf :
    port = 12345
    dbpath = data
    logpath = log/mongod.log
    //linux下启动一个后台进程，win下无效
    fork = true
```
### mongod启动

```
./bin/mongod -f conf/mongod.conf
```
### 使用mongo连接mongodb服务器
```
./bin/mongo 127.0.0.1:12345/mydb
```
