# Dubbo学习笔记

## 一、dubbo框架【全面介绍】和【使用快速入门】

####  1、全面介绍
- ORM/MVC/RPC/SOA
- 传统mvc不同的service都会注册ioc中，在rpc中会把远端的service注册到本地ioc中
- 注意有两个dubbbo版本的依赖
```
com.alibaba.dubbo:2.6

org.apache.dubbo:2.7

使用的时候需要:【curator-framenwork:4.0.1】【curator-recipes:4.0.1】；不加这两个依赖会报错

还需要引入【zookeeper:3.4.13】x
```

- github中有两个dubbo管理台
```
master分支老版本：dubbo-admin/dubbo-monitor-simple
develop新版本
```

- 一些配置
```
负载均衡：默认轮询==>xml修改成轮询==>管理台修改成随机
集群容错：配置cluster
服务降级：admin配置==>动态配置==>
路由规则：服务级别之上，筛选服务
动态配置：服务级别，针对服务具体属性的
	|针对消费者过滤|动态参数名字&参数值|服务降级mock数据
标签路由：实现灰度发布、蓝绿发布
黑白名单
服务分组
分组聚合
本地存根
```

### 2、使用快速入门

### 3、dubbo核心成分：
	url===总线服务
	invoker==Dubbo的核心模型，代表一个执行体；代理对象
	invocation==调用的对象

## 二、Dubbo扩展机制

### 1.Java SPI
//加载这个接口的实现类
//指定想要加载的类名现类，需要全限定名的文件（META-INF/services/）指定想要加载的类名ServiceLoader.load(class)
缺点：全加载，耗性能
dubbo SPI可以缓存数据

### 2.Dubbo SPI
1）基本使用
引入包【dubbo-common】
@SPI加载接口中
ExtensionLoader.getExtension(class).getExtension("key");

2）应用
自动注入

AOP


## 三、dubbo配置文件的加载顺序
- -D > xml > properties
```
-Ddubbo.protocal.port=20880
dubbo.xml  <dubbo:protocal port="" />
dubbo.properties dubbo.protocal.port=20880
```

### 四、超时处理和覆盖规则

1、提供者设置
- 提供者全局配置
```xml
<dubbo:provider timeout="3000"></dubbo:provider>
```
- 接口设置
```xml
<dubbo:service timeout="3000">
    <dubbo:method name="方法名字" timeout=""></dubbo:method> 
</dubbo:service>
```

2、消费者设置
- 全局配置
```xml
<dubbo:consumer timeout=""></dubbo:consumer>
```
- 接口配置
```xml
<dubbo:reference timeout="">
    <dubbo:methond name="方法名字" timeout=""></dubbo:methond>
</dubbo:reference>
```

3、说明

- 顺序
消费者的method、提供者的method、消费者的reference、提供者的reference、消费者全局、提供者全局

- 注解版本

@Service(timeout="")、@Reference(timeout="")、方法？、全局？

- 注意

建议实际配置在服务端、修改的是服务端的消费者：原因，没有人比我自己了解我

几个地方配置的例子：
```xml
<dubbo:consumer check=""></dubbo:consumer>
<dubbo:reference check=""></dubbo:consumer>
<dubbo:metthod></dubbo:consumer> 启动不检查
注解方式：@Reference(check="")
```


