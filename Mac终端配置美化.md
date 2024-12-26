# Mac终端配置美化

Mac终端配置有两种：

1. 配置原生的终端（推荐）

2. 安装iTerm2，并配置iTerm2

## 1. 配置原生终端（推荐）

### 1.1 查看并切换终端为zsh

先检查是否安装zsh，并启用zsh。（Mac基本都已经安装，并启用了，可以跳过这部分。）

```bash
# 查看已安装的shell
cat /etc/shells
```

```bash
# 查看当前运行的shell
echo $SHELL
```

如果出现 `/bin/zsh` ，则默认终端为zsh。如果不是zsh，则用下面的命令，将默认终端切换为zsh。

```bash
# 切换终端为zsh
chsh -s /bin/zsh
```

### 1.2 安装配置oh-my-zsh

安装方法 1：从GitHub上的源进行安装。

```bash
# 安装方法1：从GitHub上的源进行安装，可以用下面的任意一种方式
```bash
# 方式一：
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
# 方式二：
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

安装方法2：下载到本地安装。

```bash
# 将整个项目打包下载到本地
地址：https://github.com/ohmyzsh/ohmyzsh

# 解压进入目录
cd ohmyzsh-master

# 执行安装命令
./tools/install.sh

# 下面的代码可以使配置立刻生效
source ~/.zshrc
```

安装方法3：如果GitHub的源安装失败，可以使用Gitee的源。

```bash
# 方式一：
sh -c "$(curl -fsSL https://gitee.com/mirrors/oh-my-zsh/raw/master/tools/install.sh)"

# 方式二：
sh -c "$(wget -O- https://gitee.com/pocmon/ohmyzsh/raw/master/tools/install.sh)"
```

配置oh-my-zsh

安装oh-my-zsh后，会默认生成一个配置文件，可以通过配置文件来修改主题。如果觉得默认主题OK，则可以直接开始使用。

官方的主题效果，可以在[这里查看](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes)。

```bash
# 配置文件路径
~/.zshrc

# 配置主题的代码为ys(这个可以改为自己喜欢的主题)
ZSH_THEME="ys"

# 下面的代码可以使配置立刻生效
source ~/.zshrc
```

注意：

配置agnoster等主题时，会有字体显示问题：需要安装powerline字体。安装和使用字体方法，请参考：https://github.com/powerline/fonts。

### 1.3 配置默认终端的主题和显示效果

下载solarized主题，并解压。

```bash
# 下载solarized主题
git clone https://github.com/altercation/solarized.git
```

打开终端配置

【描述文件】->【导入】，将solarized的主题导入。主题路径为：solarized-master/osx-terminal.app-colors-solarized/Solarized Dark ansi.terminal，我这里用的是Dark主题。

设置终端

字体大小：13或者14号

窗口大小：120 x 30

如果只需要用原生终端，那后面的 iTerm2 配置就不用看了。

## 2. iTerm2 终端配置

### 2.1 安装iTerm2

* [iTerm2官网](https://iterm2.com)下载iTerm2客户端，并且安装。

### 2.2 查看并切换终端为zsh

先检查是否安装zsh，并启用zsh。（Mac基本都已经安装，并启用了，可以跳过这部分。）

```bash
# 查看已安装的shell
cat /etc/shells
```

```bash
# 查看当前运行的shell
cho $SHELL
```

如果出现 `/bin/zsh` ，则默认终端为zsh。如果不是zsh，则用下面的命令，将默认终端切换为zsh。

```bash
# 切换终端为zsh
chsh -s /bin/zsh
```

### 2.3 安装配置oh-my-zsh

安装方法 1：从GitHub上的源进行安装。

```bash
# 安装方法1：从GitHub上的源进行安装，可以用下面的任意一种方式
```bash
# 方式一：
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
# 方式二：
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

安装方法2：下载到本地安装。

```bash
# 将整个项目打包下载到本地
地址：https://github.com/ohmyzsh/ohmyzsh

# 解压进入目录
cd ohmyzsh-master

# 执行安装命令
./tools/install.sh

# 下面的代码可以使配置立刻生效
source ~/.zshrc
```

安装方法3：如果GitHub的源安装失败，可以使用Gitee的源。

```bash
# 方式一：
sh -c "$(curl -fsSL https://gitee.com/mirrors/oh-my-zsh/raw/master/tools/install.sh)"

# 方式二：
sh -c "$(wget -O- https://gitee.com/pocmon/ohmyzsh/raw/master/tools/install.sh)"
```

配置oh-my-zsh

安装oh-my-zsh后，会默认生成一个配置文件，可以通过配置文件来修改主题。如果觉得默认主题OK，则可以直接开始使用。

官方的主题效果，可以在[这里查看](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes)。

```bash
# 配置文件路径
~/.zshrc

# 配置主题的代码
ZSH_THEME="agnoster"

# 下面的代码可以使配置立刻生效
source ~/.zshrc
```

注意：

配置agnoster等主题时，会有字体显示问题：需要安装powerline字体。安装和使用字体方法，请参考：[https://github.com/powerline/fonts。](https://github.com/powerline/fonts%E3%80%82)

### 2.4 解决agnoster主题字体问题说明

1. 先安装powerline字体，可以访问下面的仓库，安装方法链接中也有。

https://github.com/powerline/fonts

2. 然后设置iTerm2的字体为powerline中的任意一个字体。
3. 我的配色方案如下：
* zsh文件配置：ZSH_THEME="agnoster" # 主题也可以设置为：ZSH_THEME="ys"
* iTerm2字体：Meslo LG L DZ for Powerline
* iTerm2的主题选择：Solarized Dark
