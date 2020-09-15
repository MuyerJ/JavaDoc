# Solr简介和安装



### 一、简介 

1.对于商品模糊查询的时候，%在前面是不走索引的

2.key:分词 value:list<id>

3.jieba分词

问题

​	1）有些不是分词没查到

​	2）分词太多-->字典树（又叫前缀树，四个属性）--->倒排索引算法

### 二、solr和lucence、solrCloud、ES的区别

1.solr和lucence

2.solr和solrCloud

3.solr和ES

### 三、安装

1.安装概述

需要配置jdk8

下载solr/Linux解压

solr以前是用tomcat作为容器；而在solr以后内部集成jetty

2.安装solr

1）下载solr

​	wget solr镜像url

2）解压solr

​	解压并移动到   /usr/local

​	tar -zxvf solr.tgz

​	mv solr /usr/local

3）启动solr	

​	进入bin目录（目录分析一下。。。）

​	./solr start 有可能启动问题，加一个参数 -force

​	./solr start -force

4）测试访问

​	http://ip:8983/solr/

5）安装存在的问题

​	无法启动（没有显示pid的值）：因为服务器配置太低，关闭暂时不用的服务

​	无法访问：

​		阿里云服务器==>因为你对外的8983的端口没有开发，登录到你阿里云后台去开发一下

​		VM==>因为你的虚拟机防火墙没有关闭



​	