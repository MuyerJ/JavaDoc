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

### 三、客户端使用

```
#查看目录结构
ls path  

#启动client
./zkCli.sh start

#创建节点
create [-s] [-e] path data [acl]
	-s有序
	-e临时

#获取节点信息
get path

#更新节点信息
set path data

#监控节点信息
get path watch

```

### 四、选举机制

1）半数机制：集群中半数以上机器存活，集群可用；zk适合安装奇数台服务器
2）有一个leader，其他follower，leader是通过内部选举机制临时产生的
3）server的id和启动顺序比较重要
按启动顺序投票，投最大的id，半数以上的为leader
启动集群最好以后往前启动集群


### 五、面试

- 选举机制（paxos算法）
- 监听原理
- 部署方式、集群角色、集群最少个数
- 常用命令
```
ls
create
get
rmr
delete
set
```







