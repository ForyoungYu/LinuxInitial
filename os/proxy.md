### 1.4 代理

<img src="images/windows-cfw-1.png" alt="Clash" style="zoom: 80%;" />

**Clash** 是由 Dreamacro 开发的，是一个使用 [Go](https://zh.wikipedia.org/wiki/Go) 开发的、基于规则的隧道。

**Clash**本身没有[图形界面](https://zh.wikipedia.org/wiki/图形界面)，仅提供[HTTP](https://zh.wikipedia.org/wiki/HTTP) [RESTful](https://zh.wikipedia.org/wiki/RESTful) [API](https://zh.wikipedia.org/wiki/API)。社区开发了 Clash for Android、Clash for Windows、ClashX（ MacOS 平台 ）等工具，处理到 Clash 的指令，用户从而可以直接在图形界面下控制其行为。

1. 首先下载**Clash 客户端**（推荐）：

```bash
sudo pacman -S clash
```

或者去[Clash 的 Github 下载页面](https://github.com/Dreamacro/clash/releases)下载最新的压缩文件。

2. 进入到 clash 的目录`~/.config/clash`,运行下面的命令下载配置文件：

```bash
wget -O config.yaml https://stc-spadesdns.com/link/Nz5QuF88lZftTOlf?clash=2&log-level=info
```

![命令](images/2021-08-31%2020-23-30%20的屏幕截图.png))

3. 然后打开系统设置，找到**网络**，选择**手动**，填写 HTTP 和 HTTPS 代理为 `127.0.0.1:7890`，填写 Socks 主机为 `127.0.0.1:7891`，即可启用系统代理。

![设置](images/linux-clash-5.jpg)

4. 接着访问[Clash Dashboard](http://clash.razord.top/)可以进行切换节点、测试延迟等操作。
   ![网站](images/linux-clash-4.jpg)

### 1.5 终端代理

#### 方法一

有时候代理搭建好了，但是在终端中却无法使用代理，这就需要用到另一个工具：**proxychains**

ProxyChains 是 Linux 和其他 Unix 下的代理工具。 它可以使任何程序通过代理上网， 允许 TCP 和 DNS 通过代理隧道， 支持 HTTP、 SOCKS4 和 SOCKS5 类型的代理服务器， 并且可配置多个代理。 ProxyChains 通过一个用户定义的代理列表强制连接指定的应用程序， 直接断开接收方和发送方的连接。

**安装：**

```bash
sudo pacman -S proxychains-ng
```

ProxyChains 的配置文件位于`/etc/proxychains.conf` ，打开后你需要在末尾添加你使用的代理。例如：

```bash
[ProxyList]
# add proxy here ...
# meanwile
# defaults set to "tor"

socks5  127.0.0.1 7891
```

**使用：**

ProxyChains 的使用方式非常简单，直接在应用程序前加上 proxychains4 即可。例如

```bash
proxychains4 curl www.httpbin.org/ip
```

这个命令可以测试当前的公网 IP，分别用 ProxyChains 测试一下看看 IP 地址是否改变。

#### 方法二

在终端中直接运行：

```bash
export http_proxy=http://proxyAddress:port
```

如果你是 SSR,并且走的 http 的代理端口是 12333，想执行 wget 或者 curl 来下载国外的东西，可以使用如下命令：

```bash
export http_proxy=http://127.0.0.1:12333
```

如果是 https 那么就经过如下命令：

```bash
export https_proxy=http://127.0.0.1:12333
```

或者把代理服务器地址写入 shell 配置文件.bashrc 或者.zshrc 直接在.bashrc 或者.zshrc 添加下面内容

```bash
export http_proxy="http://localhost:port"
export https_proxy="http://localhost:port"
```

或者走 socket5 协议（ss,ssr）的话，代理端口是 1080

```bash
export http_proxy="socks5://127.0.0.1:7891"
export https_proxy="socks5://127.0.0.1:7891"
```

或者干脆直接设置 ALL_PROXY

```bash
export ALL_PROXY=socks5://127.0.0.1:7891
```

最后在执行如下命令应用设置

```bash
source ~/.bashrc
```
