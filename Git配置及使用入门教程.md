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

## 2. git本地仓库建立以及基本操作

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
git add -A  # add所有修改的文件，不仅会添加当前目录下的文件，还会添加子目录中的文件
git add .  # add所有修改的文件，不会添加子目录中的文件
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
# 1. 硬回退
git reset --hard HEAD^ # 回退到上一个版本
git reset --hard HEAD~n # 回退到往上 n 个版本
git reset --hard commit_id # 回退到指定版本

# 注意：HEAD表示当前版本，硬回退当前版本会被直接覆盖。
# 2. 使用 checkout 回退
git checkout -b <new - branch - name> commit_id
# 创建新分支来切换到之前的版本时，当前版本的修改也不会丢失。

```

其他命令：

```python
git reflog # 可以查看命令历史，以便可以回到未来的某个版本。
git reset --hard commit_id # 可以在不同的版本之间穿梭，可以在退回之后（包括硬回退），又可以继续往未来的版本进行跳转。
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

git checkout -b dev origin/dev # 新建 dev 分支，并将远程库的dev分支与本地的dev分支关联起来

git push -u origin main # 把本地库与远程库关联起来，并且，把所有内容推送到远程库main分支

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

第一步：

创建Repository，得到 Repository 的 SSH 地址。

第二步：克隆远程库

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

然后，就可以开始在本地开发了。

## 5. GitHub的SSH配置

1. 生成SSH文件

默认会生成到路径：~/.ssh。在这个路径下，会有两个文件，*id_ras*表示私钥，*id_rsa.pub*表示公钥。

```python
# 生成ssh文件

ssh-keygen -t rsa -b 4096 -C "<你的邮箱>或者其他备注"

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

## 6. git一般的协作工作流程

在日常开发中，通常会遵循一定的工作流程来确保代码的稳定性和团队协作的效率。以下是一个常见的基于 `main` 和 `dev` 分支的工作流程，适用于大多数开发团队：

### 工作流程概述

1. **从远程仓库获取最新代码**
2. **创建新的功能分支**
3. **在功能分支上进行开发**
4. **提交并推送功能分支**
5. **创建 Pull Request（或者直接合并）**
6. **代码审查和合并**
7. **更新本地分支**

### 详细步骤

#### 1. 从远程仓库获取最新代码

首先，确保你的本地 `main` 和 `dev` 分支都是最新的：

```sh
git checkout main
git pull origin main

git checkout dev
git pull origin dev
```

#### 2. 创建新的功能分支

从 `dev` 分支创建一个新的功能分支。功能分支名称通常基于你正在开发的功能或修复的内容。

```sh
git checkout dev
git checkout -b feature/my-new-feature
```

#### 3. 在功能分支上进行开发

在新的功能分支上进行开发，添加和提交你的更改：

```sh
# 编辑代码文件
git add .
git commit -m "描述你的更改"
```

开发过程中可以多次提交，确保每个逻辑单元独立提交。

#### 4. 提交并推送功能分支

将功能分支推送到远程仓库，以便其他团队成员可以查看和审查：

```sh
git push origin feature/my-new-feature
```

#### 5. 创建 Pull Request（或者直接合并）

在支持 Pull Request 的平台（如 GitHub、GitLab、Bitbucket 等）上，创建一个从 `feature/my-new-feature` 到 `dev` 分支的 Pull Request。描述你的更改并请求代码审查。

#### 6. 代码审查和合并

团队成员会对 Pull Request 进行审查，提出修改建议。所有问题解决后，合并 Pull Request。这一步通常由项目维护者或有权限的开发人员完成。合并后删除远程的功能分支：

```sh
git branch -d feature/my-new-feature  # 删除本地分支
git push origin --delete feature/my-new-feature  # 删除远程分支
```

#### 7. 更新本地分支

合并完成后，确保你的本地 `dev` 和 `main` 分支是最新的：

```sh
git checkout dev
git pull origin dev

git checkout main
git pull origin main
```

如果 `dev` 分支的更改需要推送到 `main` 分支，可以按照之前提到的合并流程，将 `dev` 分支合并到 `main` 分支：

```sh
git checkout main
git merge dev
git push origin main
```

通过这个流程，团队可以保持代码库的稳定性，确保所有更改经过审查后才合并到主分支，同时也能灵活地管理多个人同时开发的不同功能。

### 总结

- **从远程仓库获取最新代码**：确保你的 `main` 和 `dev` 分支是最新的。
- **创建新的功能分支**：基于 `dev` 分支创建一个新的功能分支。
- **在功能分支上进行开发**：在功能分支上进行开发并提交更改。
- **提交并推送功能分支**：将功能分支推送到远程仓库。
- **创建 Pull Request**：请求将功能分支合并到 `dev` 分支。
- **代码审查和合并**：审查并合并 Pull Request。
- **更新本地分支**：确保本地 `dev` 和 `main` 分支是最新的，并在需要时合并 `dev` 到 `main`。

---

参考网站

1. git官网：[https://git-scm.com/](https://git-scm.com/)

2. git官网教程：[https://git-scm.com/book/zh/v2](https://git-scm.com/book/zh/v2)

3. 廖雪峰git教程：[教程链接](https://www.liaoxuefeng.com/wiki/896043488029600)
