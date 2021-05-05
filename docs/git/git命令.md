# git命令

Git 全局设置:

```
git config --global user.name "大表哥"
git config --global user.email "wjw_595@126.com"
```

创建 git 仓库:

```
mkdir test
cd test
git init
touch README.md
git add README.md
git commit -m "first commit"
git remote add origin git@gitee.com:www.wjw595.com/test.git
git push -u origin master
```

已有仓库?

```
cd existing_git_repo
git remote add origin git@gitee.com:www.wjw595.com/test.git
git push -u origin master
```



## 创建分支

git status  //查看状态

创建分支

```
#创建分支
git checkout -b branchname
#查看分支
git branch -a  #带*的是当前的

#切换分支
$ git checkout -b zhanghanlun origin/zhanghanlun
#切换到origin/zhanghanlun分支命令本地分支为”zhanghanlun”
```

## 合并分支

```shell
$git status
#添加到暂存区
$git add .
#提交
$git commit -m "初始化"
```



+ 切到主分支

  ```shell 
  $git branch
  #切换到主分支
  $git checkout master 
  #branchname分支合并到主分支
  $git merge branchname 
  #本地master分支推送到push
  $git push 
  
  ```

  本地子分支推动到云端

  ```shell
  #origin 远端的意思  当前分支推送到远端 branchname分支中
  $git push -u origin branchname
  ```


## 忽略不必要的文件

gitignore不生效.gitignore只能忽略原来没有被跟踪的文件，因此跟踪过的文件是无法被忽略的。因此在网页上可以看到target等目录的存在。
解决方法就是先把本地缓存删除（改变成未track状态），然后再提交:

```
$git rm -r --cached .
$git add .
$git commit -m 'add .gitignore file'
```

