# Arch 配置

## Arch 的安装

参考链接：[Get-my-Arch-Linux](https://github.com/NiiiKlaus/Get-my-Arch-Linux)

## Arch 配置

### 镜像源配置

#### pacman

Arch 及其衍生版本均使用`pacman`作为包管理器。

对于 Manjaro, 只需运行如下命令即可切换到中国源：

```bash
sudo pacman-mirrors -i -c China -m rank
```

选择速度最快的一个软件源，manjaro 所做的这一步相当于切换软件仓库镜像（所指下一部分）

对于 Arch Linux, 则需要两步：

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

## 输入法配置

### fcitx

```bash
# fcitx框架
sudo pacman -S fcitx-im fcitx-configtool

# 输入法
sudo pacman -S fcitx-rime rime-double-pinyin
sudo pacman -S kcm-fcitx # KDE Config Module for Fcitx
```

### fcitx5

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

## Linux 必安装的依赖以及应用

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

## 常见问题

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
