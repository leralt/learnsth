# git版本控制工具：2021.3.1

### 本地常用命令

`git init` ：将文件夹初始化为仓库`[repository]`

`git add`  `[filename]` ：添加追踪文件

`git commit` `-m "[description]"` ：提交文件到仓库

`git status`：仓库当前状态

`git diff`：查看修改内容

`git diff HEAD -- [filename]`：查看工作区和版本库里面最新版本的区别

`git log`：查看提交日志

`git reset--hard [commit id]/HEAD^`：`HEAD`总是表示当前版本，`^`表示上一个版本，`HEAD~99`表示往上99个版本

`git reflog`：记录每一次操作，可以查看回滚前的`commit id`，再用上一条命令恢复

`git checkout -- [filename]`：用版本库里的文件替换工作区的

`git reset HEAD [filename]`：可以把暂存区的修改撤销掉

`git rm`：删除文件

### 远程仓库命令

`git remote add origin git@server-name:path/repo-name.git`  `(like git@gitee.com:bytepro/new.git)`：关联远程仓库

​				`origin`：远程库名字

`git push -u origin master`：把本地的`master`分支内容推送的远程新的`master`分支，把本地的`master`分支和远程的`master`分支关联起来

`git push origin master`：把本地的`master`分支内容推送的远程新的`master`分支

`git remote -v`：查看远程库信息

`git remote rm [repo_name]`：删除远程仓库

`git clone git@server-name:path/repo-name.git`：克隆远程仓库到当前目录

​				也可以使用`https`协议：`git clone https://github.com/username/reponame.git`

`git pull`

### 分支管理

`git branch`：列出所有分支，当前分支前面会标一个`*`号

​				`-d [branch_name]`：删除该分支

`git checkout [branch_name]`：切换分支

​				` -b [branch_name]`：创建并切换到该分支

`git merge [branch_name]`：合并指定分支到当前分支

​				`--no-ff`参数，表示禁用`Fast forward`，可以保留分支历史

`git switch`：切换分支

​				`-c [branch_name]`：创建并切换到该分支

### 标签管理

`git tag v1.0.0`：给当前分支`commit`打上标签

​				`git tag v0.0.9 [commit id]`

​				`-d [tagname]`：删除标签

`git tag`：查看标签

`git show [tagname]`查看标签信息

`git push origin [tagname]`可以推送一个本地标签；

`git push origin --tags`可以推送全部未推送过的本地标签；

`git tag -d [tagname]`可以删除一个本地标签；

`git push origin :refs/tags/[tagname]`可以删除一个远程标签

### 自己搭建git服务器

##### 一、安装`git`

在`ubuntu`下

```shell
sudo apt-get install git
```

在`debain`下

```shell
sudo yum install git
```

##### 二、创建`git`用户

```shell
sudo adduser git
```

##### 三、创建证书登陆

把用户公钥`id_rsa.pub`导入到`/home/git/.ssh/authorized_keys`文件中，一行一个

##### 四、初始化`git`仓库

选定一个目录为`git`仓库，比如`/srv/example.git`，在`/srv`输命令

```shell
sudo git init --bare example.git
```

`git`就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的`git`仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的`git`仓库通常都以`.git`结尾。然后，把`owner`改为`git`

```shell
sudo chown -R git:git sample.git
```

##### 五、禁用`shell`登录

编辑`/etc/passwd`，找到`git`的一行，类似

```
git:x:1001:1001:,,,:/home/git:/bin/bash
```

改为

```
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
```

这样，`git`用户可以正常通过ssh使用git，但无法登录shell，因为我们为`git`用户指定的`git-shell`每次一登录就自动退出

##### 六、克隆远程仓库

通过`git clone`命令克隆远程仓库

```shell
git clone git@server_ip:/srv/example.git    
正克隆到 'example'...
warning: 您似乎克隆了一个空仓库。
```

这样就可以愉快的工作啦！

