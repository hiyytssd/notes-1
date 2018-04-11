# GitHub 简单使用入门

## 目录
* [设置名字和Email地址](#设置名字和email地址)
* [创建版本库](#创建版本库)
* [把文件提交到版本库](#把文件提交到版本库)
* [版本回退](#版本回退)
* [删除文件](#删除文件)
* [远程仓库](#远程仓库)
* [从远程库克隆](#从远程库克隆)
* [整个项目同步到GitHub](#整个项目同步到github)
* [获取并合并](#获取并合并)
* [参与GitHub上的项目](#参与github上的项目)
* [常见报错](#报错)
* [GitHub tips](#GitHub-tips)

### 设置名字和Email地址
``` git
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```
### 创建版本库
1. 新建目录（文件夹）
``` git
mkdir folder
```
2. 进入目录（文件夹）
```
cd folder
```
3. 显示当前目录
``` git
pwd
```
4. 初始化
``` git
git init
// git init 初始化命令把这个目录变成 Git 可以管理的仓库
// 成功后显示：Reinitialized existing Git repository in E:/git-test/.git/
```
5. 使用命令后可以看到 .git 目录，该目录默认是隐藏的
``` git
ls -ah
```
### 把文件提交到版本库
1. 编写一个 readme.txt 文件，放到 folder 文件夹下，内容如下：
``` git
Git is a version control system.
Git is free software.
```
2. 把文件添加到仓库
``` git
git add readme.txt
// 没有任何提示表示添加成功
// 提交多个文件 ： git add file2.txt file3.txt
```
3. 把文件提交到仓库
``` git
git commit -m "提交说明"
// 用命令 git commit 告诉 Git，把文件提交到仓库
// -m 后面输入的是本次提交的说明
// 在执行 git commit 之前，可以运行 git status 看看当前仓库的状态
// 使用 git diff HEAD -- readme.txt 查看具体修改了什么内容
// 使用 git log 命令显示从最近到最远的提交日志（一大串类似 3628164...882e1e0 的是 commit id（版本号））
// 使用 git checkout -- readme.txt （没有--，就变成了“切换到另一个分支”的命令），把 readme.txt 文件在工作区的修改全部撤销，让这个文件回到最近一次 git commit 或 git add 时的状态。
// 使用 git reset HEAD file 可以把暂存区的修改撤销掉（unstage），重新放回工作区，既可以回退版本，也可以把暂存区的修改回退到工作区。
```
### 版本回退
1. 回退到上一个版本
``` git
git reset --hard HEAD^
// 回退到上一个版本：在 Git 中，用 HEAD 表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，回退到上 100 个版本，就写成 HEAD~100
```
2. 查看回退版本后的内容
``` git
cat readme.txt
```
3. 回退到指定版本
```
git reset --hard 3628164
// 回退到最新版本（在命令行窗口没有关闭的条件下），3628164... 是 commit id，版本号没必要写全，前几位就可以了，Git会自动去找
// 如果已经关闭了命令行窗口，可以使用 git reflog 找到 commit id
```
### 删除文件
1. 删除文件
``` git
git rm test.txt
// git status 命令查看哪些文件被删除了
```
2. 确实删除
``` git
git commit
// 确实要从版本库中删除该文件，那就用命令 git rm 删掉
    ```
3. 如果删错了
``` git
git checkout -- test.txt
// 如果删错了
// git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。
```
### 远程仓库

1. 创建秘钥
``` git
ssh-keygen -t rsa -C "youremail@example.com"
// 在系统用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。
```
2. 登陆 GitHub，进入 settings -> SSH and GPG keys -> New SSH key ，在 Key 文本框里粘贴 id_rsa.pub 文件的内容

3. GitHub -> new repository(位于头像边上的加号下列菜单中)，在 Repository name 填入 git-test，点击 Create repository 按钮，就成功地创建了一个新的 Git 仓库

4. 在本地的 git-test 仓库下运行命令
``` git
git remote add origin git@github.com:GitHub账户名/仓库名称
// 这条命令在 github 建好仓库在提示中可以看到
```
5. 把本地库的所有内容推送到远程库上
``` git
git push -u origin master 
// 这条命令在 github 建好仓库在提示中可以看到
// 第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
// 如果提交成功，在 github 的 Repositories 下就可以看到了
```
6. 从现在起，只要本地作了提交（先 git add readme.txt，然后 git commit -m “提交说明”），就可以通过命令把本地master分支的最新修改推送至GitHub
``` git
git push origin master
```

### 从远程库克隆
>第一次链接远程仓库时，需要在第一次链接远程仓库的电脑上重新生成一个密钥，然后执行上面的“远程仓库”中的第 1 – 3 步
1. 登陆 GitHub -> new repository，创建一个新的仓库，创建时勾选 Initialize this repository with a README，这样 GitHub 会自动为我们创建一个 README.md 文件。

2. 使用命令 git clone 克隆一个本地库，在执行命令时，先 cd 到需要克隆到本地的目录（ cd e:/本地仓库 ）
``` git
cd e:/本地存放仓库的目录
git clone git@github.com:GitHub账户名/仓库名称
```
3. 克隆到本地后，执行初始化过程
``` git
git init // 初始化命令把这个目录变成 Git 可以管理的仓库
``` 
### 整个项目同步到GitHub
下列方法同样适用于，修改本地文件后同步到 GitHub 上
``` git
git init // 如果本地仓库已初始化，且只修改某个文件后同步到 GitHub 上，则省略此步骤

git add .  // 注意这里有个 . 

git commit -m “init” // init 是提交说明，init 代表首次初始化

git remote add origin 你的github仓库地址 // 也可以是：git remote add origin git@github.com:你的github用户名/你的仓库名称

git push origin master
```
### 获取并合并
git pull 与 git push 操作的目的相同，但是操作的目标相反。
``` git
git pull origin master:my_test
```
上面的命令是将 origin 仓库的 master 分支拉取并合并到本地的 my_test 分支上。

如果省略本地分支，则将自动合并到当前所在分支上。如下：
``` git
git pull origin master
```
### 参与GitHub上的项目
``` git
$git clone <远程Arepository> #克隆你 fork 出来的分支

$git remote add <远程Brepository标签> git@github.com:XXXX/ceph.git #添加远程 Brepository 标签

$git pull <远程B仓库标签> master:master  #从远程 Brepository 的 master 分支拉取最新 objects 合并到本地 master 分支

$git checkout YYYY #切换到要修改的分支上

$git branch develop; git checkout develop #在当前分支的基础上创建一个开发分支，并切换到该分支上，你将在该分支上 coding

coding...... #在工作区 coding

$git add .#将修改保存到索引区

$git commit -a #将修改提交到本地分区

$git push origin my_test:my_test #将本地分支my_test提交到远程A repository 的 my_test 分支上
```
### 报错

1. 提示出错信息：`fatal: remote origin already exists.`

    在输入 `git remote add origin git@github.com:github帐号/github仓库` 后出现 `fatal: remote origin already exists.`

    解决办法如下：

    1. 先输入  `git remote rm origin`

    2. 再输入  `git remote add origin git@github.com:你的github账号/github仓库` 就不会报错了！

2. 没有密钥的错误信息

```
Warning: Permanently added the RSA host key for IP address ‘192.30.253.112’ to the list of known hosts.
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
```

解决方法：

本机第一次链接远程仓库时，需要在第一次链接远程仓库的电脑上重新生成一个密钥，然后执行上面的“[远程仓库](#远程仓库)”中的第 1 – 3 步

### GitHub-tips
Git的奇技淫巧：[GitHub tips](https://github.com/git-tips/tips)