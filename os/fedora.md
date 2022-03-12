# Fedora配置

> **Fedora的维基百科**  
> Fedora Linux（第七版以前为Fedora Core）是较具知名度的Linux发行包之一，由Fedora项目社群开发、红帽公司赞助，目标是创建一套新颖、多功能并且自由（开放源代码）的操作系统。Fedora是商业化的Red Hat Enterprise Linux发行版的上游源码。  
> Fedora对于用户而言，是一套功能完备、更新快速的免费操作系统；而对赞助者Red Hat公司而言，它是许多新技术的测试平台，被认为可用的技术最终会加入到Red Hat Enterprise Linux中。  
> Fedora大约每六个月发布新版本。  
> 截至2016年2月，Fedora大约有120万用户[3]，这其中包括了Linux内核的作者林纳斯·托瓦兹。

## 1 使用镜像

[配置清华源镜像参考链接](https://mirrors.tuna.tsinghua.edu.cn/help/fedora/)

**Fedora 30 或更新版本**

### fedora 仓库 (/etc/yum.repos.d/fedora.repo)

```bash
[fedora]
name=Fedora $releasever - $basearch
failovermethod=priority
baseurl=https://mirrors.tuna.tsinghua.edu.cn/fedora/releases/$releasever/Everything/$basearch/os/
metadata_expire=28d
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-$releasever-$basearch
skip_if_unavailable=False
```

### updates 仓库 (/etc/yum.repos.d/fedora-updates.repo)

```bash
[updates]
name=Fedora $releasever - $basearch - Updates
failovermethod=priority
baseurl=https://mirrors.tuna.tsinghua.edu.cn/fedora/updates/$releasever/Everything/$basearch/
enabled=1
gpgcheck=1
metadata_expire=6h
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-$releasever-$basearch
skip_if_unavailable=False
```

### fedora-modular 仓库 (/etc/yum.repos.d/fedora-modular.repo)

```bash
[fedora-modular]
name=Fedora Modular $releasever - $basearch
failovermethod=priority
baseurl=https://mirrors.tuna.tsinghua.edu.cn/fedora/releases/$releasever/Modular/$basearch/os/
enabled=1
metadata_expire=7d
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-$releasever-$basearch
skip_if_unavailable=False
```

### updates-modular 仓库 (/etc/yum.repos.d/fedora-updates-modular.repo)

```
[updates-modular]
name=Fedora Modular $releasever - $basearch - Updates
failovermethod=priority
baseurl=https://mirrors.tuna.tsinghua.edu.cn/fedora/updates/$releasever/Modular/$basearch/
enabled=1
gpgcheck=1
metadata_expire=6h
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-$releasever-$basearch
skip_if_unavailable=False
```

## 2 常用软件

### Snap

```bash
sudo dnf install snapd
sudo ln -s /var/lib/snapd/snap /snap
sudo snap install <packagename>
```

### Typora

下载[二进制文件](https://typora.io/linux/Typora-linux-x64.tar.gz)

解压后放到自己规定的目录当中，然后将DesktopEntry目录中的Typora.desktop复制到`.local/share/applications`目录下即可。

### Vim&NeoVim

依赖：
- Anaconda3
- pynvim
- nodejs
- npm
- yarn

```bash
# Aanconda3的安装不再说明

# pynvim
pip install pynvim --upgrade 

# nodejs npm
sudo dnf install nodejs npm

# yarn
# 方法一：使用npm安装
sudo npm install yarn -g

# 方法二：使用dnf安装
curl -sL https://dl.yarnpkg.com/rpm/yarn.repo -o /etc/yum.repos.d/yarn.repo

sudo dnf install yarn 

# 方法三：使用脚本安装
curl -o- -L https://yarnpkg.com/install.sh | bash

# 最后打开neovim执行命令
:PlugInstall
```

**PS:** 在安装插件的过程中，如果遇到coc.nvim安装失败的情况，需要进入到nvim的插件安装目录`.vim`下的`coc.nvim`目录下运行`yarn install`即可。

### LazyGit

```bash
sudo dnf copr enable atim/lazygit -y
sudo dnf install lazygit
```

### 美化工具

```bash
sudo dnf install gnome-tweak-tool
```

## 3 常见问题

----

### 启动VSCode时提示：请改用“/usr/bin/gcc”。

出现这一问题是由于没有吧C、C++工具包安装全，安装以下的工具即可解决。

```bash
sudo dnf install make cmake gcc g++ gdb
```
## 安装软件时提示：`Repository fedora-modular is listed more than once in the configuration`

`/etc/yum.repos.d`文件夹下出现同名的文件，重命名即可。