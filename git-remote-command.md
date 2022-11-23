
# git 远程仓库


### 创建远程公共仓库
```
#### 裸仓库 作为公共仓库 没有工作区
git init --bare  [repo.git]
```


### 远程仓库操作
```
# 列出已经存在的远程仓库
$ git remote

# 列出远程仓库的详细信息，在别名后面列出URL地址
$ git remote -v
$ git remote --verbose

# 添加远程仓库  将本地仓库与远程仓库绑定
$ git remote add <远程仓库的别名> <远程仓库的URL地址>

# 修改远程仓库的别名
$ git remote rename <原远程仓库的别名> <新的别名>

# 删除指定名称的远程仓库
$ git remote remove <远程仓库的别名>

# 修改远程仓库的 URL 地址
$ git remote set-url <远程仓库的别名> <新的远程仓库URL地址>

# 为已存在的远程仓库，添加仓库链接
# 可以同时同步多个仓库
$ git remote set-url --add <远程仓库的别名> <新的远程仓库URL地址>

# 修改push时，远程仓库的地址
$ git remote set-url --push <远程仓库的别名> <远程仓库URL地址>
```


### 克隆远程仓库
```
#### 克隆一个远程仓库
git clone <远程仓库的网址> 

# 指定本地仓库的目录
$ git clone <远程仓库的网址> <本地目录>

# -b 指定要克隆的分支，默认是master分支
$ git clone <远程仓库的网址> -b <分支名称> <本地目录>

# -o 自定义远程仓库名称
 git clone -o <自定义远程仓库名称>

```

Git 的 clone 命令会自动将远程仓库命名为 origin，拉取它的所有数据， 创建一个指向它的 master 分支的指针，并且在本地将其命名为 origin/master。 

Git 也会给你一个与 origin 的 master 分支在指向同一个地方的本地 master 分支。

<B>
“master” 是当你运行 git init 时默认的起始分支名字，原因仅仅是它的广泛使用。
“origin” 是当你运行 git clone 时默认的远程仓库名字。 
远程仓库名字 “origin” 与分支名字 “master”，在 Git 中并没有任何特别的含义。
如果你运行 git clone -o booyah，那么你默认的远程分支名字将会是 booyah/master。
</B>


### 远程分支


#### 上游分支


.git/config 配置
```
[remote "ludius-git"]
        url = git@github.com:songhuaner/ludius-git.git
        fetch = +refs/heads/*:refs/remotes/ludius-git/*
[branch "master"]
        remote = ludius-git
        merge = refs/heads/main
```
#### 查看跟踪分支信息
本地分支与远程分支的联系

```
$ git branch -vv
```
![Alt text](./images/git-branch-vv.png)

#### 创建远程分支与本地分支的联系

##### 存在本地分支， 存在远程分支
```
$ git branch --set-upstream-to=<远程分支> <本地分支>

$ git branch -u <远程分支> <本地分支>
```

##### 存在远程分支，本地分支不存在
```
# 创建本地分支，并且追踪远程分支
git branch --track <本地分支> <远程分支>

# 创建本地分支， 切换新建的本地分支为当前分支， 当前分支追踪远程分支
git checkout -b <本地分支> <远程分支>

# 创建与远程分支同名的本地分支，切换新建的同名本地分支未当前分支，当前分支追踪远程分支
git checkout --track <远程分支>

git checkout -t <远程分支>
```
##### 存在本地分支， 远程分支不存在

###### 将本地分支推送到远程仓库， 当远程分支不存在时，新建与本地分支同名的远程分支
###### 此时远程分支与本地分支之间没有联系
```
# 提交到远程分支
$ git push <远程仓库名> <本地分支名>
# 关联本地分支与远程分支
$ git branch --set-upstream-to=<远程分支> <本地分支>

```

###### 合并上述操作
``` 
$ git push -u <远程仓库名> <本地分支名>
```

#### 删除远程分支
```
$ git branch -d -r <分支名称>
```

#### 推送远程分支 git push 

##### 将本地分支的更新，推送到远程分支 
##### 如果远程分支不存在，则会新建
```
$ git push <远程仓库名> <本地分支名>:<远程分支名>
```

##### 省略远程分支名
##### 如果远程分支存在，则表示将本地分支推送与之存在”追踪关系”的远程分支(通常两者同名)
##### 如果远程分支不存在，则会被新建。此时，新建的远程分支未与本地分支建立关联
```
$ git push <远程仓库名> <本地分支名>

# 关联远程分支和本地分支
$ git branch --set-upstream-to=<远程分支> <本地分支>
```


##### 省略 本地分支
##### 表示删除指定的远程分支，等同于推送一个空的本地分支到远程分支

```
$ git push <远程仓库名> :<远程分支名>

$ git push origin :master

$ git push origin --delete master

```


##### 省略 本地分支与远程分支
##### 如果当前分支与远程分支之间存在追踪关系，则本地分支和远程分支都可以省略。
```
$ git push origin
```


##### 省略 远程仓
##### 根据配置项push.default 定义的动作执行
```
$ git push
```
###### 配置项push.default

 如果git push命令没有指定推送的源分支和目标分支，那么git会采用push.default定义的动作。不同的值适用于不同的工作流程模式。

```
nothing: 直接push会出错，需要显式的指出推送的远程分支，例如:git push origin master；

current: 推送时只会推送当前所在的分支到远程同名分支，如果远程分支不存在相应的同名分支，则创建该分支；

upstream: 推送当前分支到它的upstream分支上，这个模式只适用于推送到与拉取数据相同的仓库;

simple: 在中央仓库工作流程模式下，只能推送到与本地分支名一致的upstream分支中，
如果推送的远程仓库和拉取数据的远程仓库不一致，那么该模式会像current模式一样进行操作。
因为该选项对于新手来说是最安全的，所以在git 2.0中，simple是push.default的默认值配置项(2.0以前的默认配置项是matching)；

matching: 推送本地和远程都存在的同名分支
```


##### 在多个远程分支中，选择需要推送的远程分支
```
$ git push -u origin master
```

##### 强制推送
```
$ giit push --force
$ giit push --f
```

#### 拉取远程分支 git fetch  
1. git fetch 

 这将更新git remote 中所有的远程仓库(repository) 所包含分支的最新commit-id, 将其记录到.git/FETCH_HEAD文件中

2. git fetch remote_repository

 这将更新名称为remote_repository 的远程repository上的所有branch的最新commit-id，将其记录。 

3. git fetch remote_repository remote_branch_name

 这将更新名称为remote_repository 的远程repository上的分支： remote_branch_name

4. git fetch remote_repository remote_branch_name:local_branch_name 

这将更新名称为remote_repository 的远程repository上的分支： remote_branch_name ，并在本地创建local_branch_name 本地分支保存远端分支的所有数据。

FETCH_HEAD： 是一个版本链接，记录在本地的一个文件中，指向目前已经从远程仓库取下来的分支的末端版本。

#### git pull 远程分支与本地分支同步