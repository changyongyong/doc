
# 开启kafka环境：

技术文档
https://kafka.apache.org/documentation/#gettingStarted

## 在此文件夹中打开终端，先开启的是zookeeper服务：
```
bin/zookeeper-server-start.sh config/zookeeper.properties
```

## 然后再打开一个终端，这次开启的是kafka服务：
```
bin/kafka-server-start.sh config/server.properties
```
