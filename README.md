# Linux 系统配置指南

> Linux 的配置指南  
> 包括的 Linux 系统有 Arch 系（Arch、Manjaro）和 Debian 系（Ubuntu、deepin、Mint）  
> 本仅提供常见 Linux OS 的配置方法，不提供系统的安装方法  
> 如发现错误，欢迎指正

## 系统跳转

一些个别的系统需要特殊配置，以下列出了常见 Linux 系统的配置方法。

[Arch Linux](os/arch.md)

[Manjaro Linux](os/manjaro.md)

[Mint Linux](os/mint.md)

[Ubuntu](os/ubuntu.md)

[Fedora](os/fedora.md)

[OpenSUSE](os/opensuse.md)

## Linux 通用配置

---

### 1.1 软件源配置

#### pacman

**Manjaro 切换到中国源**

```bash
# manjaro
sudo pacman-mirrors -i -c China -m rank
```

选择速度最快的一个软件源，manjaro 所做的这一步相当于切换软件仓库镜像（所指下一部分）

**Arch Linux 软件仓库镜像**

编辑 `/etc/pacman.d/mirrorlist`， 在文件的最顶端添加如下行（清华源）：

```bash
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
```

之后更新软件包缓存：

```bash
sudo pacman -Syy
```

**ArchlinuxCN 镜像**

在 `/etc/pacman.conf` 文件末尾添加以下两行：

```bash
[archlinuxcn]
SigLevel = Never
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

之后安装 archlinuxcn-keyring 包导入 GPG key。

#### yay

首先安装 yay：

```bash
sudo pacman -S yay
```

给 AUR 添加中国源：

```bash
yay --aururl "https://aur.tuna.tsinghua.edu.cn" --save
```

修改的配置文件位于 ~/.config/yay/config.json ，还可通过以下命令查看修改过的配置：

```bash
yay -P -g
```

#### apt

##### Ubuntu

Ubuntu 的软件源配置文件是 `/etc/apt/sources.list`。将系统自带的该文件做个备份，将该文件替换为下面内容，即可使用 TUNA 的软件源镜像。

```bash
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
```

##### Mint

Linux Mint 也采用 apt 作为包管理器，与 Ubuntu 和 Debian 类似，你需要编辑 `/etc/apt/sources.list` 和 `/etc/apt/sources.list.d/*` 中的路径。对于来自 Ubuntu 的部分源，可以参考 Ubuntu 镜像使用帮助进行修改。

以 sonya 为例，需要修改 `/etc/apt/sources.list.d/mint.list`，把 `packages.linuxmint.com` 替换为 `mirrors.tuna.tsinghua.edu.cn/linuxmint` ：

```bash
deb http://mirrors.tuna.tsinghua.edu.cn/linuxmint/ sonya main upstream import backport
```

然后运行 apt update 即可。

### 1.2 时间

在 windows 和 linux 双系统的情况下会出现 linux 系统时间比当地时间快 8 小时的情况

```bash
sudo timedatectl set-local-rtc true
```

### 1.3 输入法

#### fcitx

**ArchLinux 系统**

```bash
# fcitx框架
sudo pacman -S fcitx-im fcitx-configtool

# 输入法
sudo pacman -S fcitx-rime rime-double-pinyin
sudo pacman -S kcm-fcitx # KDE Config Module for Fcitx
```

**Ubuntu 系统**

```bash
sudo apt install fcitx-rime
sudo apt install ibus-rime
sudo apt install librime-data-double-pinyin # rime双拼
```

系统安装完以上软件后将`config_files/default.custom.yaml`文件复制到`~/.config/fcitx/rime/`目录下。

在家目录下创建文件`.xprofile`，写入以下内容：

```bash
# ~/.xprofile
export LANG=zh_CN.UTF-8
export LC_ALL=zh_CN.UTF-8
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

#### fcitx5

Fcitx5 是继 Fcitx 后的新一代输入法框架。

```bash
# fcitx5框架
sudo pacman -S fcitx5 fcitx5-im fcitx5-configtool

# 输入法
sudo pacman -S fcitx5-rime rime-double-pinyin # 中州韵输入法及其双拼输入法
sudo pacman -S fcitx5-lua # 一些额外的插件。例如对 时间和日期 的候选
sudo pacman -S fcitx5-chinese-addons # 包含了大量中文输入方式：拼音、双拼、五笔拼音、自然码、仓颉、冰蟾全息、二笔等

# Themes
sudo pacman -S fcitx5-nord fcitx5-material-color
```

欲在程序中正常启用 Fcitx5, 须设置以下环境变量，并重新登陆：

```bash
# ~/.pam_environment
GTK_IM_MODULE DEFAULT=fcitx
QT_IM_MODULE  DEFAULT=fcitx
XMODIFIERS    DEFAULT=@im=fcitx
INPUT_METHOD  DEFAULT=fcitx
SDL_IM_MODULE DEFAULT=fcitx
```

最后那行 SDL_IM_MODULE 是为了让一些使用特定版本 SDL2 库的游戏，比如 Dota2 能正常使用输入法。

fcitx5 的配置文件与 fcitx 位置不同，它的配置文件在`~/.local/share/fcitx5/`目录下，rime 的配置文件也在其中。

```bash
# 框架
sudo pacman -S fcitx5-im fcitx5-configtool

# 输入法
sudo pacman -S fcitx5-rime rime-double-pinyin

# Themes
sudo pacman -S fcitx5-nord fcitx5-material-color
```

#### ibus

```bash
# ibus框架
sudo pacman -S ibus ibus-rime

# 输入法
sudo pacman -S rime-double-pinyin ibus-pinyin rime-emoji
```

在家目录下创建文件.xprofile，写入以下内容：

```bash
export LANG=zh_CN.UTF-8
export LC_ALL=zh_CN.UTF-8
export GTK_IM_MODULE=ibus
export QT_IM_MODULE=ibus
export XMODIFIERS="@im=ibus"
```

## 2 环境搭建

---

### 2.1 Python 环境

#### pip

> 可以安装 Python 软件包的 PyPA 工具。

```bash
# Ubuntu
sudo apt install python3-pip

# Arch
sudo pacman -S python-pip

#查看pip版本
pip -V
```

添加 pip 源：复制.pip/文件夹到家目录。

#### Conda（推荐）

**Anaconda 镜像使用帮助**

Anaconda 是一个用于科学计算的 Python 发行版，支持 Linux, Mac, Windows, 包含了众多流行的科学计算、数据分析的 Python 包。

[Anaconda 下载界面](https://www.anaconda.com/products/individual#Downloads)

[Anaconda3-2021.05-Linux-x86_64 下载地址](https://repo.anaconda.com/archive/Anaconda3-2021.05-Linux-x86_64.sh)

TUNA 还提供了 Anaconda 仓库与第三方源（conda-forge、msys2、pytorch 等，查看完整列表）的镜像，各系统都可以通过修改用户目录下的 `.condarc` 文件。Windows 用户无法直接创建名为 `.condarc` 的文件，可先执行 `conda config --set show_channel_urls yes` 生成该文件之后再修改。

注：由于更新过快难以同步，我们不同步 pytorch-nightly, pytorch-nightly-cpu, ignite-nightly 这三个包。

添加 conda 源，将以下内容复制到`.condarc`文件下。

```bash
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

即可添加 Anaconda Python 免费仓库。

运行 `conda clean -i` 清除索引缓存，保证用的是镜像站提供的索引。

运行 `conda create -n myenv numpy` 测试一下吧。

**利用 conda 升级 Anaconda 及其包**

升级 conda(升级 Anaconda 前需要先升级 conda)：`conda update conda`

升级 anaconda：`conda update anaconda`

升级 spyder：`conda update spyder`

更新所有包：`conda update --all`

安装包：`conda install package`

更新包：`conda update package`

查询某个 conda 指令使用-h 后缀，如`conda update -h`

**Miniconda 镜像使用帮助**

Miniconda 是一个 Anaconda 的轻量级替代，默认只包含了 python 和 conda，但是可以通过 pip 和 conda 来安装所需要的包。

Miniconda 安装包可以到 <https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/> 下载。

### 2.2 Go 环境

安装 Golang 的命令：

```bash
# 安装Golang
sudo pacman -S go
# 设置`GOPROXY`
go env -w GOPROXY=https://goproxy.cn,direct
```

设置 Go 语言环境，需要添加`GOPATH`和`GOROOT`到环境变量。

```bash
export GOPATH=$HOME/Go/bin
export GOROOT=/usr/lib/go # Golang的安装目录
```

### 2.3 Latex

TeX Live 是一个完整、功能强大的 TeX 发布版本，包含了主要的 Tex 相关程序、宏和字体，官方软件仓库收录了它。 老的(停止开发) teTeX 发布版本位于 AUR

[TexLive ArchWiki](<https://wiki.archlinux.org/index.php/TeX_Live_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)>)

```bash
# Arch
sudo pacman -S texlive-most texlive-langchinese

# Ubuntu
sudo apt install texlive-full
```

## 3 工具及终端配置

---

### Alacritty

从软件库中安装的 [Alacritty](https://github.com/alacritty/alacritty) 往往有一些 bug，这时不如自己手动编译它，[Alacritty 安装方法](https://github.com/alacritty/alacritty/blob/master/INSTALL.md)

安装 alacritty 主题配置工具：

```bash
pip install --user alacritty-colorscheme
```

### 3.1 vim/neovim 配置

vim：只需将配置文件`.vim`放到家目录即可（配置文件名为`.vimrc`）  
neovim：须将其配置文件放到`~/.config/nvim/`目录下（配置文件名为`init.vim`）

vim-plug 安装

> 在安装 vim-plug 之前确保 python 以及相关库已经安装  
> [Jump to Python 的环境搭建](#Python环境)

```bash
# vim
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim

# neovim
sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'

# 如果拒绝连接，编辑/etc/hosts文件
sudo echo 199.232.28.133 raw.githubusercontent.com >> /etc/hosts

# 安装插件的依赖库
pip install pynvim --upgrade
sudo pacman -S nodejs npm yarn

# 最后打开neovim执行命令
:PlugInstall
```

NeoVim 依赖

```bash
# Node.js >=v16.14.0

# 格式化工具

# latexindent
conda install latexindent.pl -c conda-forge

# python: yapf
pip install yapf

# shell: shfmt
# c/c++: clang-format
```

**PS:** 在安装插件的过程中，如果遇到 coc.nvim 安装失败的情况，需要进入到 nvim 的插件安装目录`.vim`下的`coc.nvim`目录下运行`yarn install`即可。

### Tmux

这里使用已配置好的[Tmux](https://github.com/gpakosz/.tmux)

```bash
cd
gh repo clone gpakosz/.tmux
ln -s -f .tmux/.tmux.conf
cp .tmux/.tmux.conf.local .
```

### 3.2 zsh 配置

#### 安装 zsh

```bash
# Arch
sudo pacman -S zsh

# Ubuntu
sudo apt install zsh
```

修改默认 Shell 为 zsh

```bash
chsh -s /usr/bin/zsh
```

#### 安装 oh-my-zsh

```bash
curl -Lo install.sh https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh && sh install.sh

# Gitee
sh -c "$(curl -fsSL https://gitee.com/shmhlsy/oh-my-zsh-install.sh/raw/master/install.sh)"

# or
sh -c "$(wget -O- https://gitee.com/shmhlsy/oh-my-zsh-install.sh/raw/master/install.sh)"
```

提供一些[oh-my-zsh 皮肤](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes)，下面的是我常用的一些皮肤：

```bash
# typewritten
git clone https://github.com/reobin/typewritten.git $ZSH_CUSTOM/themes/typewritten

ln -s "$ZSH_CUSTOM/themes/typewritten/typewritten.zsh-theme" "$ZSH_CUSTOM/themes/typewritten.zsh-theme"

# powerlever10k
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/themes/powerlevel10k

# 配置powerlever10k命令
p10k configure
```

**oh-my-zsh 插件**

```bash
# zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

# zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

# zsh-completions
git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:-${ZSH:-~/.oh-my-zsh}/custom}/plugins/zsh-completions
```

### 3.3 autojump

```bash
git clone git://github.com/wting/autojump.git
cd autojump
./install.py

#卸载
./uninstall.py
```

### 3.4 fzf

fzf is a general-purpose command-line fuzzy finder.

输入如下命令安装 fzf:

```bash
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
```

或者直接安装 fzf：

```bash
sudo pacman -S fzf
```

### 3.5 ranger 配置

[ranger 的 Github 页面](https://github.com/ranger/ranger)

ranger 的主要配置文件及其作用如下：

> `commands.py`用于配置自定义命令
>
> `rc.conf`用于配置 ranger 的按键操作
>
> `plugins`文件夹用于添加插件

目前已经添加的插件、命令以及快捷键如下：

插件（plugins 目录中）：

- ranger-devicons
- ranger-autojump

安装插件命令：

```bash
# ranger-devicons
git clone https://github.com/ForyoungYu/ranger_devicons ~/.config/ranger/plugins/ranger_devicons
```

在`commands.py`中已添加的功能(参考[custom commands](https://github.com/ranger/ranger/wiki/Custom-Commands))：

- my_edit
- extract <paths>
- extracterhere
- compress
- mkcd
- fzf_select
- up
- toggle_flat

在`rc.conf`中已添加的快捷键(参考[Keybindings](https://github.com/ranger/ranger/wiki/Keybindings))：

- `<A-f>`：打开 fzf(需安装[fzf](#fzf))
- `cj`:autojump 插件命令(需要安装[autojump](#autojump))
- `mk`：mkcd 命令
- `C`:compress
- `X`:extracthere

ranger 相关命令：

```bash
# 生成ranger配置文件命令
ranger --copy-config=all

# 修改默认编辑器命令
select-editor # Ubuntu
```

## 4 st & dwm

---

[suckless 官网](https://suckless.org/)

分别进入两个目录下，输入以下命令可完成编译及安装：

```bash
make
sudo make clean install
```

其中，dwm 需复制`dwm.desktop`文件到`/usr/share/xsessions`目录下，并将`.dwm`目录复制到家目录中。

**DWM 依赖**

```bash
sudo pacman -S xorg xorg-server # xsetroot命令
sudo pacman -S feh # 壁纸管理
sudo pacman -S picom # 淡入淡出、半透明、阴影等视觉效果
sudo pacman -S trayer # 系统托盘
sudo pacman -S dmenu
```

## 5 美化

---

### 5.1 Gnome 美化

```bash
# gnome美化面板
sudo pacman -S gnome-tweak-tool

# 用chromium配置gnome的工具
sudo pacman -S chrome-gnome-shell
```

**插件推荐**

- Dash to Dock
- IBus Tweaker
- Coverflow Alt-Tab
- OpenWeather
- User Themes
- Net speed Simplified
- GSConnect
- ArcMenu
- Application Menu
- Pop Shell
- ~~Lunar Calendar 农历~~
- ~~ray Icons~~

### 5.2 KDE Plasma 美化

#### Dock

```bash
# kde dock
sudo pacman -S latte-dock

# plank dock
sudo pacman -S plank

# 基于 OpenGL的混合型窗口管理器
sudo pacman -S compiz
```

之后运行`latte-dock`或`plank`开启。
这两种 Dock 会默认自启动。

#### 主题

#### Plasma 样式

- brease2
- brease2 Dark
- McMojave
- cherry
- Qogir-dark
- ChromeOS

#### 图标

- Tela
- Tela Dark

#### 光标

- Breeze 微风
- Breeze 微风亮色
- DeepinDark Cursors
- DeepinWhite Cursors

#### 欢迎屏幕

- BeautifulTreeAnimation
- Harmaony
- McMojave
- QuarksSplashDark
- QuarksSplashDarkLight

## 6 Linux 必安装的依赖以及应用

---

```bash
# ======================基础依赖===========================

# 设备驱动
sudo pacman -S net-tools
sudo pacman -S xorg xorg-server
sudo pacman -S xf86-input-synaptics # 触摸板驱动
sudo pacman -S xf86-input-libinput
sudo pacman -S xf86-video-intel # intel显卡驱动
sudo pacman -S xf86-video-ati # amd显卡驱动
sudo pacman -S alsa-utils pulseaudio pulseaudio-alsa # 声音管理器

# 底层应用
sudo pacman -S w3m # 终端浏览器
sudo pacman -S ntfs-3g # 识别windows分区
sudo pacman -S make cmake # 编译工具

# ======================应用程序==========================
# Arch / Manjaro
sudo pacman -S dolphin
sudo pacman -S nmtui # 修改静态IP地址
sudo pacman -S zsh
sudo pacman -S fish
sudo pacman -S git
sudo pacman -S yay # Arch的AUR

# 语言相关
sudo pacman -S g++ clang gdb
sudo pacman -S go

# 终端模拟器
sudo pamcan -S gnome-terminal # Gnome默认终端
sudo pacman -S kconsole # KDE默认终端
sudo pacman -S alacritty
sudo pacman -S yakuake
sudo pacman -S tilda

# 终端应用
sudo pacman -S htop
sudo pacman -S ranger
sudo pacman -S lazygit
sudo pacman -S clash  # Clash代理

# 窗口管理器
sudo pacman -S i3
sudo pacman -S polybar

# 应用软件
sudo pacman -S visual-studio-code-bin # VSCode
sudo pacman -S google-chrome # Google Chrome
sudo pacman -S netease-cloud-music # 网易云音乐
sudo pacman -S kdenlive # 视频剪辑软件
sudo pacman -S gimp # 修图软件
sudo pacman -S inkscape # 矢量图制作软件
sudo pacman -S simplescreenrecorder # 录屏软件
sudo pacman -s obs-studio
sudo pacman -S libreoffice-fresh-zh-cn # 中文版libreoffice

# PDF浏览器
sudo pacman -S okular
sudo pacman -S evince

# Latex编辑器
sudo pacman -S texstudio
sudo pacman -S texworks

# Wechat
yay -S wechat-uos # 统信 UOS 魔改版
yay -S deepin-wine-wechat
yay -S deepin.com.wechat2
sudo pacman -S electronic-wechat-git
sudo pacman -S electron-wechat
yay -S wechat-devtools

# QQ & TIM
yay -S deepin-wine-tim
yay -S deepin-wine-qq
```

## 7 常见问题

### Alacritty 终端模拟器

**问题：**

当输入 clear 时出现`'alacritty': unknown terminal type.`时

**解决办法：**
**解决办法：**

修改环境变量，在/etc/profile 中添加如下：

$ export TERMINFO=/usr/share/terminfo
重启系统生效。

### 在 Gnome 中使用 fxitx

**问题：**

将 ibus 卸载后安装 fcitx 后无法打字

**解决办法：**

在`/etc/environment`文件中输入以下内容:

```bash
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS="@im=fcitx"
```

### manjaro 系统编译 LaTeX 生成的 PDF 无法显示中文

**问题：**

manjaro 系统编译 LaTeX 生成的 PDF 无法显示中文

**解决办法：**

原因是 okular 软件使用软件包 poppler-data 解析 pdf 中的中文

```bash
sudo pacman -S poppler-data
```

### Gnome40 无法使用 dash-to-dock

> [参考 dash-to-dock/gnome40 分支](https://github.com/ewlsh/dash-to-dock/tree/ewlsh/gnome-40) >[issue 问题](https://github.com/micheleg/dash-to-dock/pull/1402#issuecomment-814937395) >[YouTobe 教程](https://www.youtube.com/watch?v=hhhNy7mY0nI&ab_channel=JulianGonzalez)

```bash
# 依赖sassc
npm install sass
sudo pacman -S sassc

git clone https://github.com/ewlsh/dash-to-dock/
cd dash-to-dock
git checkout ewlsh/gnome-40
make
make install
```

更新仓库：

```bash
git pull
make
make install
```
