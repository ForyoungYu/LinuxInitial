# openSUSE配置

## 镜像

参考[openSUSE 镜像使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/opensuse/)

## IBUS输入法

gnome输入法经常出现各种各样的问题，主要原因是没有将其配置好。
**最稳妥的办法**是在`/etc/profile`文件中添加如下的内容：

```bash
# /etc/profile
export GTK_IM_MODULE=ibus
export XMODIFIERS=@im=ibus
export QT_IM_MODULE=ibus
```
## SnapCraft

参考[Installing snap on openSUSE](https://snapcraft.io/docs/installing-snap-on-opensuse)

## Nvidia显卡驱动

安装：[openSUSE Wiki: NVIDIA 驱动](https://zh.opensuse.org/SDB:NVIDIA_%E9%A9%B1%E5%8A%A8)

切换驱动：[NVIDIA SUSE Prime](https://zh.opensuse.org/SDB:NVIDIA_SUSE_Prime)

## 参考文献

[openSUSE Wiki](https://zh.opensuse.org/%E9%A6%96%E9%A1%B5)

