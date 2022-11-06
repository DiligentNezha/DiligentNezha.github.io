---
title: 软件包管理工具
date: 2022-11-06 10:58:24
tags:
  - 开发环境
  - shell
---

Mac 上常用的软件包管理工具有两个：

- brew

  - 官网地址

    > [The Missing Package Manager for macOS (or Linux) — Homebrew](https://brew.sh/)

  - 官网介绍

    > macOS（或 Linux）缺失的软件包的管理器

- MacPorts

  - 官网地址

    > [麦克波特项目 - 首页 (macports.org)](https://www.macports.org/)

  - 官网介绍

    > MacPorts 项目是一个开源社区计划，旨在设计一个易于使用的系统，用于在[Mac 操作系统](http://www.apple.com/macos/)上编译、安装和升级基于命令线、X11 或 Aqua 的开源软件

这里主要介绍 brew 的安装及常用命令，MacPorts 感兴趣可以自行研究，本篇不会对提到的相关软件进行详细介绍，详细介绍和使用有可能会在后续的其他文章中进行介绍。

### brew 安装

由于网络原因，为了能顺利安装软件，首先我们需要配置安装源镜像进行加速

根据自己的 shell，自行设置如下环境变量

检查当前 shell，在终端下执行，我的是 zsh

```shell
echo $SHELL
/bin/zsh
```

如果不是 zsh 建议使用如下命令进行 shell 的设置，后续的操作默认使用 zsh 作为 shell

```shell
sudo chsh -s /bin/zsh
```

- zsh 用户

  ``` shell
  echo 'export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.ustc.edu.cn/brew.git"' >> ~/.zshrc
  echo 'export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.ustc.edu.cn/homebrew-core.git"' >> ~/.zshrc
  echo 'export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles"' >> ~/.zshrc
  source ~/.zshrc
  ```

- bash 用户

  ``` shell
  echo 'export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.ustc.edu.cn/brew.git"' >> ~/.bash_profile
  echo 'export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.ustc.edu.cn/homebrew-core.git"' >> ~/.bash_profile
  echo 'export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles"' >> ~/.bash_profile
  source ~/.bash_profile
  ```

执行如下命令进行软件安装，此处使用了 CDN 加速源来进行安装脚本的下载，安装过程会提示你输入你的登陆密码，根据提示输入即可

```shell
/bin/bash -c "$(curl -fsSL https://cdn.jsdelivr.net/gh/Homebrew/install@HEAD/install.sh)"
```

看到如下提示说明安装成功了

![jlte4Ou8LPdAbk5](https://i.loli.net/2021/11/21/jlte4Ou8LPdAbk5.png)

Mac OS 自动的终端使用起来有些不方便，刚好可以利用 brew 安装大名鼎鼎的 iTerm2，在开始安装之前需要先设置下 cask 镜像地址

```shell
brew tap --custom-remote --force-auto-update homebrew/cask https://mirrors.ustc.edu.cn/homebrew-cask.git
source ~/.zshrc
```

```shell
brew install homebrew/cask/iterm2
```

![hW8Dp7KaSxBvqeT](https://i.loli.net/2021/11/21/hW8Dp7KaSxBvqeT.png)

安装 iTerm2 后，打开简单看下界面

![DVHkjpOoCLzJysr](https://i.loli.net/2021/11/21/DVHkjpOoCLzJysr.png)

### 终端主题

上边我们通过 brew 安装的 iTerm2 终端，现在我们来美化我们的终端。

首先安装 zsh 插件管理器，网上有很多关于 Oh My Zsh 的安装教程，但是 omz 其缺点非常明显，就是加载慢，因此，这里我选择 zinit 来作为 zsh 的插件管理器。

**由于安装过程需要从 GitHub clone 相关仓库，因此需要解决网络问题，避免安装过程中出现问题，我这边通过开启终端代理来解决，我的代理是用的 ClashX（这部分内容自行搜索解决，也可使用其他方式，如果使用其他方式，下边代码中的端口根据实际情况自行修改即可），在 .zshrc 文件中追加如下两个函数。**

```shell
function proxy_on() {
        export https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890 all_proxy=socks5://127.0.0.1:7890
        echo -e "已开启代理"
}
function proxy_off() {
        unset http_proxy
        unset https_proxy
        echo -e "已关闭代理"
}
# 此处可以直接调用来开启代理，也可以每次使用时临时调用开启
proxy_on
```

执行如下命令进行安装

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/zdharma-continuum/zinit/HEAD/scripts/install.sh)"
```

![fEZJVUnMlSiTgP6](https://i.loli.net/2021/11/21/fEZJVUnMlSiTgP6.png)

安装完成后执行如下命令，使其生效

```shell
zinit self-update
```

插件管理器我们安装好了，下来安装常用的命令行效率工具

打开 .zshrc 文件

```shel
vim ~/.zshrc
```

在 ~/.zshrc 文件末尾追加下边的代码

```shell
zinit light zsh-users/zsh-autosuggestions
zinit light zsh-users/zsh-completions
zinit light zsh-users/zsh-syntax-highlighting
```

安装完插件我们执行如下命令来查看下我们目前已经安装了哪些插件

```shell
zinit loaded
```

![Onq6uDr1YwAgm7H](https://i.loli.net/2021/11/21/Onq6uDr1YwAgm7H.png)

现在我们来体验安装了插件后的终端，我只敲了 zinit 紧跟着就会提示你历史中的命令，如果提示就是你想敲的命令，按下 <kbd>↑</kbd> 键就会将提示内容补全，是不是非常省心呢？

![e95QjxNVoAud6g8](https://i.loli.net/2021/11/21/e95QjxNVoAud6g8.png)

我们再来看一个例子，默认的终端下，我们敲 ls -- 然后按 <kbd>tab</kbd>键，只会提示有哪些可用参数，而装了插件后同样的操作，除了提示可用参数外还会告诉你该参数的作用，看图

![f7EX4dx9sl5e3pO](https://i.loli.net/2021/11/21/f7EX4dx9sl5e3pO.png)

更多插件相关介绍可以去官网了解

- [zsh-users/zsh-autosuggestions: Fish-like autosuggestions for zsh (github.com)](https://github.com/zsh-users/zsh-autosuggestions)
- [zsh-users/zsh-syntax-highlighting: Fish shell like syntax highlighting for Zsh. (github.com)](https://github.com/zsh-users/zsh-syntax-highlighting)
- [zsh-users/zsh-completions: Additional completion definitions for Zsh. (github.com)](https://github.com/zsh-users/zsh-completions)

现在，我们插件管理器有了，可以美化我们的终端了，就是给终端换个主题。

打开 .zshrc 配置文件

```shell
vim ~/.zshrc
```

打开文件后在文件末尾追加如下代码

```shell
# Load powerlevel10k theme
zinit ice depth = 1 # git clone depth
zinit light romkatv/powerlevel10k
```

保存后执行如下命令使其生效

```shell
source ~/.zshrc
```

执行后会先下载相关字体，然后根据提示退出程序重新启动，启动后第一次会提示你进行配置，根据提示进行交互式配置，非常简单，这里我截取了第一个配置项提示交互界面，根据自己的喜好进行选择即可。

![miyPb48McnSzkDR](https://i.loli.net/2021/11/23/miyPb48McnSzkDR.png)

配置完成后的效果如下，我进入到 zinit 的安装目录

![w7ANLqk4vKztfcR](https://i.loli.net/2021/11/23/w7ANLqk4vKztfcR.png)

**注意**

在我刚配置好的会话窗口中，一切正常，当我新打开一个 tab，输入 ls 命令，双击 <kbd>tab</kbd> 发现提示没有了，不知道什么原因，执行下边的命令进行插件更新后功能又可以正常使用了，下边命令能正常执行的前提是终端代理正常开启，为了每次打开新的 tab 都能正常使用，也可以把下边的命令追加在 .zshrc 文件末尾，如果没有该问题，忽略即可。

```shell
zinit update --parallel 100
```

### 常用命令行工具

#### autojump

- 官网地址：[wting/autojump: A cd command that learns - easily navigate directories from the command line (github.com)](https://github.com/wting/autojump)

- 官网介绍：一种快速导航你系统的方式

- 解决痛点：在终端中我们经常需要在不同的路径来回切换，这样，路径很长时，切换非常麻烦，因为根本记不住这么长的路径，即使可以记住，把整个路径输入也是很耗费精力的一件事。autojump 通过为你经常访问的路径建立一个字典保存在数据库中，这样在你下次进入到相同路径时，只需要根据给出你要进入的路径索引，autojump 便可以快速进入该索引对应的路径。

- 安装方式：

  ```shell
  brew install autojump
  ```

  ![fvdL5FZDwxsK1BX](https://i.loli.net/2021/11/27/fvdL5FZDwxsK1BX.png)

  如上图，安装成功后，会提示我们在 shell 配置文件中加载相关 shell 脚本文件，执行下边的命令

  ```shell
  echo "[ -f /usr/local/etc/profile.d/autojump.sh ] && . /usr/local/etc/profile.d/autojump.sh" >> ~/.zshrc
  source ~/.zshrc
  ```

  看效果，我分别通过 cd 进入到 /usr/local/Cellar/ruby /usr/local/Cellar/ranger /usr/local/Cellar/readline，此时我敲 j r 双击 <kbd>tab</kbd> 就会提示我匹配的路径索引，根据提示输入索引回车即可进入，是不是非常方便，当然，也可以通过 jo 快速开发 Finder

  ![6CLrEsf89lVHQhU](https://i.loli.net/2021/11/28/6CLrEsf89lVHQhU.gif)

#### the fuck

官网地址：[nvbn/thefuck: Magnificent app which corrects your previous console command. (github.com)](https://github.com/nvbn/thefuck)

官网介绍：the fuck 是一个纠正前一个控制台命令错误的工具

解决痛点：我们经常在敲命令的时候由于敲错一个字符导致命令执行失败，这真是一个令人沮丧的时刻，the fuck 就是在这种沮丧的时刻给我们鼓励，只需要在终端中输入 fuck ，the fuck 机会帮我们纠正错误的命令，是不是很神奇

安装方式：

```shell
brew install thefuck
echo "eval $(thefuck --alias)" >> ~/.zshrc
source ~/.zshrc
```

安装完成后看效果，我先输入正确的命令，然后输入错误的命令，提示 unknown command，此时输入 fuck，将会交互式提示你正确的命令，可以通过<kbd>↑</kbd><kbd>↓</kbd>进行切换选择，<kbd>Enter</kbd>确认执行，<kbd>Ctrl</kbd>+<kbd>c</kbd>取消。

![RwYc8ykpvsniJMU](https://i.loli.net/2021/11/27/RwYc8ykpvsniJMU.gif)

#### tldr

官网地址：[tldr pages](https://tldr.sh/)

官网介绍：tldr 是社区驱动，努力提供简明的 man pages 最佳实践案例

安装方式：安装方式有两种，npm 和 pip3，npm 和 pip3 的安装也可以参考下边的 Node 和 Python 环境搭建

```shell
# npm
npm install -g tldr

# pip3
pip3 install tldr
```

安装后，如何使用呢？比如我现在想解压一个压缩包，我只记得是 tar 命令，参数不记得是什么了，这个时候就可以使用 tldr tar 来查询

```shell
tldr tar
```

效果如下

![VMCm7P8ANovLusj](https://i.loli.net/2021/11/29/VMCm7P8ANovLusj.png)

#### vimrc

官网地址：[[GitHub - amix/vimrc: The ultimate Vim configuration (vimrc)](https://github.com/amix/vimrc)](https://tldr.sh/)

官网介绍：终极的 Vim 配置

安装方式：这里有两个版本，基础版和终极版，这里分别介绍安装方式

- 基础版

  ```shell
  git clone --depth=1 https://github.com/amix/vimrc.git ~/.vim_runtime
  sh ~/.vim_runtime/install_basic_vimrc.sh
  ```

  安装完成后我们打开 ~/.zshrc 文件看看效果

  ![G6d7gcZOTlKFoJr](https://s2.loli.net/2021/12/19/G6d7gcZOTlKFoJr.png)

- 终极版

  ```shell
  git clone --depth=1 https://github.com/amix/vimrc.git ~/.vim_runtime
  sh ~/.vim_runtime/install_awesome_vimrc.sh
  ```

  ![GYT49meuQ27VNpv](https://s2.loli.net/2021/12/19/GYT49meuQ27VNpv.png)

  安装完成后我们打开 ~/.zshrc 文件看看效果，可以很直观的看到，配色主题有所差异，其实终极版还为我们安装了很多有用的插件，比如 nerdtree，看效果 

  ![ahQydT2HVM9sOi5](https://s2.loli.net/2021/12/19/ahQydT2HVM9sOi5.png)

#### mycli

官网地址：[mycli](https://www.mycli.net/)

官网介绍：mycli 是一个为 MySQL, MariaDB, and Percona  提供自动补全和语法高亮功能的命令行工具

安装方式：

```shell
brew install mycli
```

安装非常简单，安装成功后连接到数据看效果

```shell
mycli -h 127.0.0.1 -P 3306 -u root
```

![g6wN5RLSlVqP3Te](https://s2.loli.net/2021/12/21/g6wN5RLSlVqP3Te.gif)

#### fzf

官网地址：[GitHub - junegunn/fzf: A command-line fuzzy finder](https://github.com/junegunn/fzf)

官网介绍：fzf 是一个通用的命令行模糊查找器

安装方式：

```shell
brew install fzf
# 安装键盘绑定和模糊补全
$(brew --prefix)/opt/fzf/install
```

安装非常简单，安装成功后连接到数据看效果

#### bat

#### ranger

### 常用开发环境

#### Docker

Docker 已经是程序员必须要了解和掌握的技术了，不管是前端还是后端。通常来说，本地机器，不管是 Mac 还是 Windows，官网都提供了可视化的安装包，根据自己的操作系统下载，一路下一步即可。

官网地址：[Get Started with Docker | Docker](https://www.docker.com/get-started)

官网介绍：Docker 是开发更加高效和可预测

安装方式：官网根据自己操作系统下载对应安装包安装即可。

#### Java

Java 入门都是从 JDK 安装开始，随着 JDK 版本的快速迭代，如何在本地管理多个版本的 JDK 并根据需要切换 JDK 版本？本地多个版本如何设置环境变量？这里通过介绍两款工具来解决以上问题。jabba 和 jenv。

##### jabba

官网地址：[shyiko/jabba: (cross-platform) Java Version Manager (github.com)](https://github.com/shyiko/jabba)

官网介绍：jabba 是一个受 nvm 启发的 Java 版本管理器

安装方式：

```shell
curl -sL https://github.com/shyiko/jabba/raw/master/install.sh | bash && . ~/.jabba/jabba.sh
```

通过下边的命令安装一个 JDK 8 和一个 JDK 11

```shell
jabba install adopt-openj9@1.8.0-292
jabba install adopt-openj9@1.11.0-11
```

安装完 JDK 就可以通过 java -version 来验证是否正常安装。由于我们依次安装的 JDK8 和 JDK11，在使用 java -version 输出时，我们看到的是 JDK11，那我想使用 JDK8 时该怎么做呢？实际上 jabba 也提供了相关功能，但是我发现不好用（个人观点），下边我们通过另外一个工具来解决 JDK 版本切换问题。

##### jEnv

官网地址：[jEnv - Manage your Java environment](https://www.jenv.be/)

官网介绍：jEnv 是一个让你忘记如何设置 JAVA_HOME 环境变量的工具

安装方式：

```shell
brew install jenv
echo 'export PATH="$HOME/.jenv/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(jenv init -)"' >> ~/.zshrc
source ~/.zshrc
```

jEnv 安装好之后执行下边的命令将我们上边安装的两个 JDK 加入进来，如果还安装了其他版本，注意替换版本号，

```shell
jenv add ~/.jabba/jdk/adopt-openj9@1.8.0-292/Contents/Home
jenv add ~/.jabba/jdk/adopt-openj9@1.11.0-11/Contents/Home
```

添加号 JDK 后，通过下边的命令来列出目前已有的 JDK 版本

```shell
jenv versions
```

![R2McSNgyKF3tm58](https://i.loli.net/2021/11/27/R2McSNgyKF3tm58.png)

jenv 提供了三个子命令来指定使用哪个 JDK 版本

- global 配置的全局的 JDK 版本，我这里设置我的全局 JDK 版本为 1.8

  ```shell
  jenv global 1.8
  # 设置完成后可以直接使用 jenv global 来查看全局 JDK 版本的设置
  ```

- local 配置当前目录的 JDK 版本

  ```
  jenv local 11
  ```

- shell 配置当前 shell 的 JDK 版本

  ```shell
  jenv shell 1.8
  ```

启用导出插件

```shell
jenv enable-plugin export
exec $SHELL -l
```

检查  JAVA_HOME 是否正常设置

```shell
echo ${JAVA_HOME}
```

安装好 JDK 后就是按照 Maven 和 Gradle 了，非常方便

```shell
brew install maven
brew install gradle
```

现在来为 Maven 和 Gradle 配置镜像加速，这里使用阿里云，直接参照 https://developer.aliyun.com/mirror/maven

#### Node

和 Java 一样 Node 的版本发布也很频繁，因此也需要一个管理 Node 版本的工具，此处我们使用 nvm 来实现该需求。

- nvm

  官网地址：[nvm-sh/nvm: Node Version Manager - POSIX-compliant bash script to manage multiple active node.js versions (github.com)](https://github.com/nvm-sh/nvm)

  官网介绍：nvm 是一个 node.js 版本管理器

  安装方式：

  ```shell
  brew install nvm
  
  mkdir ~/.nvm
  echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.zshrc
  echo '[ -s "/usr/local/opt/nvm/nvm.sh" ] && . "/usr/local/opt/nvm/nvm.sh"' >> ~/.zshrc
  echo '[ -s "/usr/local/opt/nvm/etc/bash_completion.d/nvm" ] && . "/usr/local/opt/nvm/etc/bash_completion.d/nvm"' >> ~/.zshrc
  
  source ~/.zshrc
  ```

  通过 nvm ls-remote 可以查看目前有哪些版本

  这里我安装两个版本的 node js

  ```shell
  nvm insatll v14.18.1
  nvm install v16.13.0
  ```

  通过 nvm current 可以查看当前使用的 node 版本，通过 use 可以切换当前使用哪个版本

  ![image-20211127215947371](/Users/jingkaihui/Library/Application Support/typora-user-images/image-20211127215947371.png)

安装好了 Node，然后给 npm 包管理配置镜像加速，这里使用阿里云镜像。

```shell
npm install -g cnpm --registry=https://registry.npmmirror.com
```

#### Python

Python 解释器版本混乱，2 和 3 差别巨大，因此也需要一个版本管理工具，这里使用 pyenv 来实现该需求。

- pyenv

  官网地址：[pyenv/pyenv: Simple Python version management (github.com)](https://github.com/pyenv/pyenv)

  官网介绍：pyenv 让你很方便的在多个 Python 版本之间切换，它很简单、遵循仅仅将一件事情做好的 UNIX 哲学。

  安装方式

  ```shell
  brew install pyenv	
  ```

  安装好 pyenv 后，这里分别安装 Python2 和 Python3

  ```shell
  pyenv install 2.7.18
  pyenv install 3.9.9
  ```

  pyenv 提供了三个子命令来指定使用哪个 Python 版本

  - global 配置的全局的 Python 版本，我这里设置我的全局 Python 版本为 1.8

    ```shell
    pyenv global 3.9.9
    # 设置完成后可以直接使用 pyenv global 来查看全局 Python 版本的设置
    ```

  - local 配置当前目录的 Python 版本

    ```
    pyenv local 3.9.9
    ```

  - shell 配置当前 shell 的 Python 版本

    ```shell
    pyenv shell 2.7.18
    ```

  安装好两个Python 后使用

  ```shell
  pyenv versions
  ```

  查看当前已安装的 Python 版本，然后为 Python 配置 pip 包加速镜像，这里使用阿里云镜像

  ```shell
  pip3 config set global.index-url https://mirrors.aliyun.com/pypi/simple/
  ```

