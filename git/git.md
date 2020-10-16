# 一、Git概述
### 1.1.Git和代码托管中心
##### 代码托管中心的任务：维护远程库

- 局域网环境下：GitLab服务器
- 外网：GitHub,码云
### 1.2.Git的结构
##### 工作区
- 写代码
- 工作区提交到暂存区：git add
##### 暂存区
- 临时存储
- 暂存区提交到本地库：git commit
##### 本地库
- 历史版本
    
# 二、Git的命令行操作
### 2.1.本地库操作
#### 2.1.1本地初始化
- 命令：git init
- 结果：
```
需要通过 ls -lA才能查看得到;
会产生一个默认隐藏.git的文件夹;
.git/ 下的内容：
    config
    description
    HEAD
    hooks/
    info/
    objects/
    refs/
```
- 注意：.git目录存放的是本地库相关的子目录和文件，不要删除，也不要胡乱修改

#### 2.1.2设置签名
- 作用：
```
区分不同开发人员身份;
和登录的账号密码一点关系也没有
```
- 命令：
```
项目级别：
git config user.name xxxx
git config user.email xxxx
//通过 cat .git/config可以查看
系统级别
git config --global user.name xxxx
git config --global user.email xxxx
//在.gitconfig中可以查看 
```
#### 2.1.3基本操作
- git status
```
on branch master
No commits yet
Untracked files
```
- git rm --cahced fileName：从暂存区删除文件
- git add fileName
- git commit -m "提交的信息" fileName 或者 git commit + 提交记录的信息 
-  git checkout -- fileName：取消修改   
-  新建文件
```
通过git add把新建的文件加入暂存区
```
-  修改文件
```
git add把修改提交到暂存区
git commit -a直接做提交的操作
```

#### 2.1.4版本的前进和后退
- 查看历史记录的命令
```
git log --pretty=online
git log --online
git reflog
```
- 历史记录的本质
```
一个HEAD的指针
基于HEAD指针回退和前进
```
- 前进后退的方式
```
基于索引值(需要配合 git reflog):
    git reset --hard 索引值(hash值)
基于~：
    只能后退
    git reset --hard HEAD~n
基于^：
    只能往后
    git reser --hard HEAD^
```
- reset的参数
```
--soft
    移动本地库指针
--mixed
    移动本地库指针
    重置暂存区
--hard  
    在本地库移动HEAD指针
    重置暂存区
    重置工作区
```
#### 2.1.5文件删除和找回
- 提交本地库后的删除
```
rm fileName
git add fileName
git commit -m "" fileName 
```
- 添加到暂存区的删除文件找回

#### 2.1.6 stash
```
stash可遇到的情况：
正在dev分支开发新功能，做到一半时有人过来反馈一个bug，让马上解决，但是新功能做到了一半你又不想提交，这时就可以使用git stash命令先把当前进度保存起来，然后切换到另一个分支去修改bug，修改完提交后，再切回dev分支，使用git stash pop来恢复之前的进度继续开发新功能。
```
git stash
```
作用：
    保存当前工作的进度，会把暂存区和工作区的改动保存起来。执行完这个命令后，通过git status查看，会发现工作去是“干净的”
命令：
    git stash save 'massage'

```
git stash list
``` 
作用：
    显示保存进度的列表
```
git stash pop [--index][stash_id]
```
git stash pop :恢复最新的进度到工作区。git默认会把工作区和暂存区的改动都恢复到工作区。
git stash pop --index: 恢复最新的进度到工作区和暂存区
git stash pop stash@{1}:恢复指定的进度到工作区。stash_id是通过git stash list命令得到的 
注意：通过git stash pop命令恢复进度后，会删除当前进度
```
git stash apply [--index][stash_id]
```
除了不删除恢复进度以外，其余和git stash pop命令一样
```
git stash drop [stash_id]
```
删除一个存储的进度
```
git stash clear
```
删除所有的进度
```
#### 2.1.6 reset
```
作用：可以让HEAD这个指针指向其他的地方
本质：移动HEAD以及它所指向的brach
三种模式：
    soft:
        保留工作目录，并把重置HEAD所带来的新差异放进暂存区
    mixed：
        保留工作目录，并清空暂存区
    hard:
        重置 HEAD 和branch的同时，重置stage区和工作目录里的内容;
        stage区和工作目录里的内容会被完全重置为和HEAD的新位置相同的内容
        没有commit的修改会被全部擦掉
```

### 2.2.分支操作
#### 2.2.1 Rebase
```
一个分支中的修改整合到另一个分支的办法有两种：merge 和 rebase
```


# ?、Idea的Git工具
- 让idea中的项目被git管理
```
setting->version control->git->path->填写xx/xx/git.exe
VCS-->Import into VC -->create git repository
```
- 提交到本地仓库
```
git-->add
git-->commit
```
- 本地库push到远程库
```
step1:创建github远程库
step2:将本地库push到远程库
    需要登录github账户：
        方式1：setting->version control->gitHub
        方式2：
```

- idea中的一些选项对应的命令
```
git-->show history==git reflog
git-->repository-->remotes
git-->repository-->push
```
- 开发人员clone
- Local Changes
- 冲突
```
什么是冲突
    多个人操作同一个文件...
    cancel/merge/rebase
如何解决：
    提交之前，要先push
避免冲突的建议：
    修改文件之前，先进行一次update操作
    修改完成-之后，及时commit，不要在本地停留过长时间
```
- idea 遇到冲突后 merge 和 rebase的区别
- idea中的update project:https://blog.csdn.net/qq_39198749/article/details/102929779
- 遇到的问题
```
Q1:
Local changes were not restored：
    stash

Q2:不好的操作
修改代码-->commit本地库-->pull==>可能会冲突

Q3:测试分支
创建branch1，并修改一行，commit/push
master修改一行，commit/push
master再修改一行，commit/push
操作：
    切换到branch1，再reset current branch to here 到master分支最新的地方
    这个有四种reset：
        soft:
        mixed:
        hard:
        keep:
        
Q4:Commit的内容有错误的
1.修改错误内容，再次commit一次
2.使用 git reset
Q5:IDEA里versionControl中(GIT工具)的log中的不同颜色是什么意思啊？
1.黄色代表HEAD, 绿色表示的是你本地分支, 紫色是远程分支
2.如果你看到一个标志是黄绿蓝, 表示当前HEAD和你远程还有你本地
3.黄色只是表示HEAD的位置,没其它含意
4.如果你看到一个提交只有紫色,表示你本地没有这个分支
5.如果你看到一个提交只有绿色,表示这只是你本地的分支提交
6.如果你看到一个是紫色和绿色,表示这个提交是远程分支并且你本地也有这个分支.
```

