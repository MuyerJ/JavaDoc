# zookeeper学习笔记

### 一、 应用场景
1） 订阅与发布
2）负载均衡
3）命名服务
4）分布式协调，通知，心跳

注意：

​	使用zk时，可以将所有的服务都注册成一个临时节点（临时节点是存储在内存中的）

​	我们判断一个服务是否可用只需要判断这个节点在zk集群中是否存在



### 二、Linux中下载和安装zk
1）下载zk
地址：https://apache.org/dist/zookeeper/stable

2）解压zk到 /usr/local下面

```
tar -zxvf zookeeper-3.4.14.gz -c /usr/local
cd /usr/local
mv zookeeper-3.4.14/ zookeeper
```

3）启动和停止

注意****：需要修改一下配置

```

//复制一份配置文件并且修改名字为“zoo.cfg”,这个是必须修改的，不然会报错
[conf]cp zoo_sample.cfg zoo.cfg   

//修改dataDir的地址，需要新建地址
dataDir=/tmp/zookeeper   ==>   dataDir=/usr/myzk1/zookeeper
mkdir -p /usr/myzk1/zookeeper     

//配置日志文件
dataLogDir=日志目录

```





