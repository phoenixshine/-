[TOC]

## 1. git 安装与配置

Mac默认安装了git，可以用下面的语句看一下

```
git --version
```

如果显示版本号，说明已安装。

安装完成后需要设置：

用户信息设置：

```python
# 当前仓库用户信息配置

git config user.name "John Doe"

git config user.email johndoe@example.com

# 全局用户信息配置

git config --global user.name "John Doe"

git config --global user.email johndoe@example.com

# 注意git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。
```

检查配置信息:

```python
git config --list # 列出所有git能找到的配置信息
git config <key> # 检查某一项配置，如：git config user.name
```

## 2. git仓库建立以及基本操作

有两种取得 Git 项目仓库的方法

* 第一种是在现有项目或目录下导入所有文件到 Git 中；

* 第二种是从一个服务器克隆一个现有的 Git 仓库。

### 2.1 新建仓库

第一步：

在本地合适的地方创建一个空目录。

第二步：

通过 `git init` 命令把这个目录变成git 可以管理的仓库。

```python
git init
```

### 2.2 将文件添加到仓库

第一步：

用`git add`命令把文件添加到仓库的暂存区（Stage）

```python
git add test.txt  # add单个修改的文件
git add -A  # add所有修改的文件
git add .  # add所有修改的文件
```

第二步：

用`git commit`命令把文件从暂存区（Stage）提交到仓库分支，`-m`后面输入的是本次提交说明

```python
git commit -m "Creating a test file"
```

说明：

为什么git添加文件要分两步？

因为`commit`可以一次提交很多文件，所以可以多次`add`不同的文件，然后一次性提交。

### 2.3 版本回退

第一步：

查看仓库以及文件的历史状态，日志文件中一串十六进制的字符，就是commit id.

```python
git status # 查看仓库状态
git diff <file_name> # 查看文件修改内容
git log # 从最近到最远的提交日志
git log --pretty=oneline # 每行显示一条提交日志
```

第二步：

回退到前面的版本

```python
git reset --hard HEAD^ # 回退到上一个版本
git reset --hard HEAD~n # 回退到往上 n 个版本
```

其他命令：

```python
git reset --hard commit_id # 可以在不同的版本之间穿梭
git reflog # 可以查看命令历史，以便可以回到未来的某个版本
```

### 2.4 工作区和暂存区

工作区(working directory)：在电脑上工作的目录就是工作区。

工作区有个隐藏目录`.git`，这个不算工作区，是Git的版本库。

![工作区暂存区仓库](Images/Git配置及使用入门教程.img/工作区暂存区仓库.png)

git add命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行git commit就可以一次性把暂存区的所有修改提交到分支。

### 2.5 撤销修改

```python
git checkout -- <file_name> # 丢弃工作区的修改，文件add到暂存区之前

git reset HEAD <file_name> # 将文件从暂存区撤离，已经add到暂存区的文件
```

如果已经提交了，那就用版本回退的方式，退回到之前的版本。

### 2.6 删除文件

删除文件：

```python
rm <file> # 从文件管理器中删除文件
git rm <file> # 从版本库中删除文件，commit 之后就会生效
```

如果错删了文件，版本库里还有，那可以进行恢复。

```python
git checkout -- <file>
# git checkout 其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。
```

## 3. 分支管理

创建分支

```python
git branch test # 创建分支test，但是不会将我们带入分支
git checkout test # 使用checkout命令来切换分支

git checkout -b test # 创建分支 test 并切换到 test 分支，相当于上面两个命令
# 或者使用switch来切换分支
git switch test
# 查看分支信息
git branch
```

回到主分支

合并分支

```python
git merge test # 回到主分支，再使用合并
```

删除分支

```python
git branch -d test # 使用 -d 标识删除分支
```

## 4. 远程仓库(以GitHub为例)

GitHub 从2021年8月开始，已经不再支持使用密码进行 Git 操作了。

适合于下载远程仓库，或者是新手使用。如果长期管理远程仓库，不建议。因为https每次提交都需要输入账户和密码，太麻烦了。

所以需要 GitHub 的 SSH 方式访问：

SSH 需要先创建私钥/公钥对，适合管理远程仓库。

### 4.1 本地->远程仓库

第一步：

创建Repository，得到 Repository 的 SSH 地址。

第二步：

将本地仓库与远程仓库关联起来。

```python
# 下面一行的命令，关联本地仓库与远程仓库

git remote add origin <远程库地址> # 添加远程库地址

git push -u origin main # 把本地库的所有内容推送到远程库main分支

git push origin main # 简化的推送main分支命令
```

管理远程仓库地址

```python
# 查看远程库信息
git remote -v

# 删除远程库地址，根据远程库名字删除（一般默认origin）
git remote rm origin
```

说明：

1. 远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。

2. 把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支main推送到远程。

3. 由于远程库是空的，我们第一次推送main分支时，加上了`-u`参数，Git不但会把本地的main分支内容推送的远程新的main分支，还会把本地的main分支和远程的main分支**关联起来**，在以后的推送或者拉取时就可以简化命令。

4. 关联后，使用命令`git push -u origin main`第一次推送master分支的所有内容；此后，每次本地提交后，只要有必要，就可以使用命令`git push origin main`推送最新修改。

### 4.2 远程->本地仓库

克隆远程库：

在 GitHub 中找到 Repository 的地址，然后使用如下命令：

```python
git clone <远程库地址>
```

从远程获取代码合并本地的版本：

```python
git pull <远程主机名> <远程分支名>:<本地分支名>

# <远程主机名> <远程分支名> <本地分支名>，这三个都可以省略
```

```python
git pull # 当前分支自动与唯一一个追踪分支合并

git pull origin # 当前分支自动与origin库中唯一一个追踪分支合并

git pull origin main # 远程的main分支与当前分支合并

git pull origin main:test # 远程main分支与 test 分支合并
```

## 5. GitHub的SSH配置

1. 生成SSH文件

默认会生成到路径：~/.ssh。在这个路径下，会有两个文件，*id_ras*表示私钥，*id_rsa.pub*表示公钥。

```python
# 生成ssh文件

ssh-keygen -t rsa -C "<你的邮箱>或者其他备注"

# 提示Enter passphrase的时候，直接回车键，可以不用输入密码。
```

2. 在GitHub上设置SSH

找到GitHub的设置SSH，添加一个SSH，将上面**公钥**的内容复制进去，保存。

3. 验证SSH是否成功

```python
ssh -T git@github.com
```

如果出现下面的内容，则表示成功：

Hi xxx! You’ve successfully authenticated, but GitHub does not provide shell access.

---

参考网站

1. git官网：[https://git-scm.com/](https://git-scm.com/)

2. git官网教程：[https://git-scm.com/book/zh/v2](https://git-scm.com/book/zh/v2)

3. 廖雪峰git教程：[教程链接](https://www.liaoxuefeng.com/wiki/896043488029600)
