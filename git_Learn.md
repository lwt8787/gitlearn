# Git 常用语句

## <font>一、本地分支</font>
- $ git branch -D branch 删除本地分支 branch，-D代表delete
- $ git branch -dr <remote/branch> 删除远程分支，remote是远程主机的名字
- $ git config --global merge.tool meld 设置合并工具meld


## 二、远程分支
从gogs库下载并完全覆盖，再上传github
1. 确认是否连接了两个主机
- git remote -v 查看remote主机的连接情况，如果只连了一个，则需要连接另外一个
```bash 
github  git@github.com:lwt8787/gitlearn.git (fetch)
github  git@github.com:lwt8787/gitlearn.git (push)
gogs    branch1 (fetch)
gogs    branch1 (push)
```
2. 上述结果中gogs远程主机没有连接，需要连接一下：
```bash
$ git remote add gogs git@192.168.17.117:lwt/learngit.git

$ git remote -v
github  git@github.com:lwt8787/gitlearn.git (fetch)
github  git@github.com:lwt8787/gitlearn.git (push)
gogs    git@192.168.17.117:lwt/learngit.git (fetch)
gogs    git@192.168.17.117:lwt/learngit.git (push)
```
3. 然后将两个库的代码全部下载
```key
$ git fetch --all
Fetching github
Warning: Permanently added the RSA host key for IP address '13.229.188.59' to the list of known hosts.
From github.com:lwt8787/gitlearn
 * [new branch]      branch1       -> github/branch1
 * [new branch]      branch1Hotfix -> github/branch1Hotfix
 * [new branch]      master        -> github/master
Fetching gogs
remote: Enumerating objects: 10, done.
remote: Counting objects: 100% (10/10), done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 6 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (6/6), done.
From 192.168.17.117:lwt/learngit
 * [new branch]      branch1    -> gogs/branch1
 * [new branch]      master     -> gogs/master
```
4. 最后设定HEAD在哪个主机的哪个分支上，现在我们为了将gogs主机的代码覆盖到本地后再上传覆盖github主机的代码，所以要先将HEAD定位到gogs/master
```key
$ git reset --hard gogs/master
HEAD is now at 0797c04 Merge branch 'branch1' to Master
```
5. 最后强制上传到github上
```key
$ git push -f github master
Enumerating objects: 10, done.
Counting objects: 100% (10/10), done.
Delta compression using up to 2 threads
Compressing objects: 100% (5/5), done.
Writing objects: 100% (6/6), 637 bytes | 159.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0)
To github.com:lwt8787/gitlearn.git
   52f4f0e..0797c04  master -> master
```
如果github上有未删除的分支，则需要手动删除

## 三、打标签
- git tag [tagename] [校验码] 轻量标签
- git tag -a [tagename] [校验码] -m '注释' 带注释的标签

##
