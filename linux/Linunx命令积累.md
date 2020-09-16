### Linunx命令积累

#### 1.查询一个安装包是否安装的的命令为：

rpm -qa | grep mysql


#### 2.重启网络服务

service network restart

#### 3.安装docker
yum -y install docker
#### 4.安装vim
yum -y install vim
#### 5.安装jdk8
- step1:搜索，寻找jdk8的名字

yum search jdk
- step2:

yum -y install java-1.8.0-openjdk

#### 6.下载solr

yum -y install wget

wget https://mirrors.tuna.tsinghua.edu.cn/apache/lucene/solr/7.7.3/solr-7.7.3.tgz

#### 7.防火墙相关
查看防火墙状态：firewall-cmd --state

停止防火墙：systemctl stop firewalld.service

禁用防火墙：systemctl disable firewalld.service

#### 8.复制/移动/删除

移动
cp -c file1 file2

mv file1/ /file2

修改文件名字
mv abc.text cba.txt
 
 





