## 1、初始化仓库
```
git init
```
第一次使用要设置用户和邮箱：
```java
git config --global user.name "gugibv"
git config --global user.email gugibv@163.com

git config --global user.name "zhoujing"
git config --global user.email zhoujing@sunline.cn

git config --global user.name        // 查看用户名是否配置成功
git config --global user.email       // 查看邮箱是否配置

# 其他查看配置相关
git config --global --list           // 查看全局设置相关参数列表
git config --local --list            // 查看本地设置相关参数列表
git config --system --list           // 查看系统配置参数列表
git config --list                    // 查看所有Git的配置(全局+本地+系统)
git config --global color.ui true    // 显示git相关颜色
```
创建ssh:
```
$ ssh-keygen -t rsa -C "gugibv@163.com"
```
将路径下的id_rsa.pub添加到github网站上

## 2、克隆远程库

```java
git remote add origin git@github.com:帐号名/仓库名.git
git remote set-url origin [url]                              // 修改远程库
git remote -v                                                // 查看远程库地址

git clone git@github.com:git帐号名/仓库名.git                 // 将远程库拉克隆到本地
git clone -b mvp3.0_dev  git@github.com:git帐号名/仓库名.git  // 指定分支克隆
```

## 3、将文件添加到仓库

<div align="center"> <img src="pics/工作区与暂存区.png" width="300"/> </div>

```java
git add 文件名 // 将工作区的某个文件添加到暂存区   
git add -u    // 添加所有被tracked文件中被修改或删除的文件信息到暂存区，不处理untracked的文件
git add -A    // 添加所有被tracked文件中被修改或删除的文件信息到暂存区，包括untracked的文件
git add .     // 将当前工作区的所有文件都加入暂存区
git add -i    // 进入交互界面模式，按需添加文件到缓存区
```
## 4、将暂存区文件提交到本地仓库

```java
git commit -m "提交说明"    // 将暂存区内容提交到本地仓库
git commit -a -m "提交说明" // 跳过缓存区操作，直接把工作区内容提交到本地仓库

git commit --amend         // 修改提交注释
git rebase -i 版本号        // 可以合并多个未 push 的commit为一次（将要合并的commit修改为s）
```
## 5、查看仓库状态
```java
git status

// 忽略文件的改动，但是不加入.gitignore 文件中
// 这样可以达到仅在本地目录中忽略，不影响其他团队成员的工作
git update-index --assume-unchanged 文件名

//查看忽略列表
git ls-files -v | grep '^h\ '

// 上一个命令的逆操作，重新追踪文件改动。
git update-index --no-assume-unchanged 文件名
```
## 6、比较文件异同
```java
git diff               // 工作区与暂存区的差异
git diff --cached      // 工作区与暂存区的差异（更详细）
    
git diff 分支名         // 工作区与某分支的差异，远程分支这样写：remotes/origin/分支名
git diff HEAD          // 工作区与HEAD指针指向的内容差异
git diff 提交id   	 // 文件路径 # 工作区某文件当前版本与历史版本的差异

git diff 版本TAG       // 查看从某个版本后都改动内容
git diff 分支A 分支B   // 比较从分支A和分支B的差异(也支持比较两个TAG)
git diff 分支A...分支  // 比较两分支在分开后各自的改动

# 另外：如果只想统计哪些文件被改动，多少行被改动，可以添加 --stat 参数
```
## 7、查看历史记录java
```java
git log                 // 查看所有commit记录(SHA-A校验和，作者名称，邮箱，提交时间，提交说明)
git log --oneline       // 让提交记录以精简的一行输出
git log -n4 --oneline   // 查看最近4次提交
git log --all --graph   // 查看所有分支历史
git log fileName        // 查看某文件的修改记录，找背锅专用
    
gitk 打开图形化界面
```
## 8、代码回滚
```java
git checkout -- filepathname   // 撤消本地还没有commit 的修改（操作工作区）

// 撤消add 的内容
git status                      // 先看一下add 中的文件 
git reset HEAD                  // 如果后面什么都不跟的话 就是上一次add 里面的全部撤销了 
git reset HEAD XXX/XXX/XXX.java // 就是对某个文件进行撤销了    
    
git reset HEAD^   // 将暂存区恢复成上次提交的版本（操作暂存区）
git reset HEAD^^  // 将暂存区恢复成上上次提交的版本，就是多个^，以此类推或用~次数
  
git reset --soft HEAD^  // 撤消commit 但是还没有push 的内容，缓存区和工作区不变
// soft：只是改变HEAD指针指向，缓存区和工作区不变；
// mixed：修改HEAD指针指向，暂存区内容丢失，工作区不变；
// hard： 修改HEAD指针指向，暂存区内容丢失，工作区恢复以前状态；
// 例如：git reset --hard HEAD 暂存区内容丢失，工作区恢复以前状态  
    
git reflog               // 查看版本号
git reset --hard 版本号   // 通过查看版本号来恢复       
```
## 9、同步远程仓库
```java
git fetch origin master                            // 将远程库更新拉取到本地
// 从远程获取最新的到本地，首先从远程的origin的master主分支下载最新的版本到origin/master分支上，
// 然后比较本地的master分支和origin/master分支的差别，最后进行合并。
// git fetch比git pull更加安全    
    
git push -u origin master                           // 将本地推到远程库  
													// 禁止向集成分支使用push -f 
													// 禁止向集成分支执行变更历史的操作    
git branch -av                                      // 查看推送同步记录  
```
## 10、删除文件
```java
git rm 文件名              // 删除文件

git mv readme readme.md   // 重命令文件
```
## 11、分支管理
```java
git branch           // 查看本地分支
git branch -a        // 查看所有分支包括本地分支和远程分支
git branch -r        // 查看远程分支

git checkout -b dev  // -b表示创建并切换分支，上面一条命令相当于下面的二条：
git branch dev       // 创建分支
git checkout dev     // 切换分支（切换到主分支：git checkout master）
    
git merge dev        // 用于合并指定分支到当前分支
git merge -h         // 查看合并帮助信息

git branch -d dev    // 删除本地分支
git push origin --delete dev // 删除远程分支   

git log --graph --pretty=oneline --abbrev-commit  // 查看分支合并图
```

## 12、工作进度保存和恢复

```java
git stash                   // 保存当前工作进度，将工作区和暂存区恢复到修改之前
git stash save message      // 作用同上，message为此次进度保存的说明
git stash list              // 显示保存的工作进度列表，编号越小代表保存进度的时间越近
git stash apply stash@{num} // 恢复，num是可选项（与 pop 区别是可恢复多次）
git stash pop stash@{num}   // 恢复工作进度到工作区，此命令的stash@{num}是可选项    
git stash drop stash@{num}  // 删除一条保存的工作进度，此命令的stash@{num}是可选项
git stash clear             // 删除所有保存的工作进度
```



## 13、标签命令
```java
git tag 标签                 // 打标签命令，默认为HEAD
git tag                     // 显示所有标签
git tag 标签 版本号          // 给某个commit版本添加标签
git show 标签               // 显示某个标签的详细信息
```


## 14、统计代码提交量

```
git log --author="zhoujing" --pretty=tformat: --numstat | awk '{ add += $1; subs += $2; loc += $1 - $2 } END { printf "added lines: %s, removed lines: %s, total lines: %s\n", add, subs, loc }' -
```

## 15、github 搜索技巧

```java
关键词 in：readme                        // readme文件中包含搜索的关键词
关键词 stars>1000                        // star 数目大于1000
"xxx" filename:xxx.java                 // 根据文件名包含关键词搜索
blog easily start in:readme stars>1000  // 搜索博客搭建
```

## 16、git cherry-pick

举例来说，代码仓库有`master`和`feature`两个分支。

```mysql
a - b - c - d   Master
         \
           e - f - g Feature
```

现在将提交`f`应用到`master`分支。

```mysql
# 切换到 master 分支
$ git checkout master

# Cherry pick 操作
$ git cherry-pick f
```

上面的操作完成以后，代码库就变成了下面的样子。

```
 a - b - c - d - f   Master
         \
           e - f - g Feature
```

从上面可以看到，`master`分支的末尾增加了一个提交`f`。

`git cherry-pick`命令的参数，不一定是提交的哈希值，分支名也是可以的，表示转移该分支的最新提交。

```mysql
$ git cherry-pick feature
```

上面代码表示将`feature`分支的最近一次提交，转移到当前分支。