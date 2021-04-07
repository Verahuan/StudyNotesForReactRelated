如果你在克隆或从远程仓库获取数据时遇到很慢甚至超时的情况，那么此时你可能需要配置 Git 的代理。我这里就讲讲两种情况的代理方法：**使用 HTTP 或 HTTPS 协议连接到 Git 仓库的代理方法**和**使用 SSH 协议连接到 Git 仓库的代理方法**。

------

- 如果远程仓库的格式像下面那样，这种就是**使用 HTTP 或 HTTPS 协议连接到 Git 仓库**的情况

```
http://github.com/cms-sw/cmssw.git
https://github.com/cms-sw/cmssw.git
```

- 如果远程仓库的格式像下面那样，这种就是**使用 SSH 协议连接到 Git 仓库**的情况

```
git@github.com:cms-sw/cmssw.git
ssh://git@github.com/cms-sw/cmssw.git
```

------

## 预先准备

在**开始操作之前**，你**需要明确**这些内容：

- 电脑已经安装了 Git（这不是废话吗，23333）。如果你是 Windows 用户，那么本文的命令你需要通过 Git Bash 来执行；如果你是 Linux 或 macOS 用户直接在终端运行即可。
- 知道如何用 vim 编辑文件，退出编辑的基本操作。

------

## 使用 HTTP 或 HTTPS 协议连接到 Git 仓库的代理方法

### 针对所有域名的 Git 仓库

- 这样做是对所有域名生效的的：

> git config –global http.proxy `protocol`://127.0.0.1:`port`

**注意**： `--glboal` 选项指的是修改 Git 的全局配置文件 `~/.gitconfig`，而非各个 Git 仓库里的配置文件 `.git/config`。`protocol` 指的是代理的协议，如 http，https，socks5 等。`port` 则为端口号。

### 针对特定域名的 Git 仓库

- 或者针对特定域名生效：

> git config –global http.`url`.proxy `protocol`://127.0.0.1:`port`

**注意**：

- 此处的 `url` 即为你需要走代理的仓库域名，`url` 以 `http://` 和 `https://` 打头的**均用**这个方法。
- 网上很多中文教程，可能会告诉你 `https://` 打头的 `url` 使用“git config –global **https**.[https://example.com.proxy](https://example.com.proxy/) `protocol`://127.0.0.1:`port`”，这种做法其实是错的！**记住一点**：Git 不认 *https.proxy* ，设置 *http.proxy* 就可以支持 https 了。
- 如果想了解 `url` 的更多模式，如子域名等的情况，可参照 [Git 的官方文档](https://git-scm.com/docs/git-config) 。网页内容搜索 `http.<url>.*`，即可找到相关信息。

------

### 实例

> 针对 HTTP 或 HTTPS 协议连接到 Git 仓库的代理方法 Windows，Linux，macOS 用户的操作是一样的。

也许你光看我上面的内容还看不明白，不妨我们来看下实例部分：

此处以 Clash for Windows 为例子。如图：

![img](D:\typora\images\1.png)

Clash for Windows 既支持 **HTTP / HTTPS** 协议代理，也支持 **SOCKS v5** 协议代理。如果你使用其他的代理软件，你可以根据你使用的代理软件的**代理协议**和本地**端口号**参考本文修改即可。

------

#### 针对所有域名的 Git 仓库

根据你的代理软件支持的代理协议**选取其中一种**即可：

- http 代理
- socks5 代理

```
git config --global http.proxy http://127.0.0.1:7890
```

**注意**：*7890* 为 Clash for Windows 的 http 代理端口。

------

#### 针对特定域名的 Git 仓库

前面我们说的是，让所有域名下的仓库都走代理的情况，但是在现实情况下我们并不想这么做。那么现在我来介绍一下针对特定域名仓库走代理的做法，此处以 GitHub 为例:

当我们从 GitHub 仓库克隆源码时我们往往是这么做的的：

```
COPYgit clone https://github.com/<user>/<repository>.git
```

那么我前面所提到的 `url` 就是 `https://github.com`

根据你的代理软件支持的代理协议**选取其中一种**即可：

- http 代理
- socks5 代理

```
git config --global http.https://github.com.proxy http://127.0.0.1:7890
```

------

## 使用 SSH 协议连接到 Git 仓库的代理方法

在这种情况下，Git 依靠 ssh 处理连接； 为了通过代理进行连接，您必须配置 ssh 本身，在 `~/.ssh/config` 文件中设置 **ProxyCommand** 选项。Linux 和 macOS 是通过 `nc` 来执行 ProxyCommand 的，Windows 下则是通过 `connect`。

------

### 相关文档

- [ssh_config(5)](https://man.openbsd.org/ssh_config) ProxyCommand 的内容:

```
COPYProxyCommand

Specifies the command to use to connect to the server. The command string extends to the end of the line, and is executed using the user's shell ‘exec’ directive to avoid a lingering shell process.

Arguments to ProxyCommand accept the tokens described in the TOKENS section. The command can be basically anything, and should read from its standard input and write to its standard output. It should eventually connect an sshd(8) server running on some machine, or execute sshd -i somewhere. Host key management will be done using the Hostname of the host being connected (defaulting to the name typed by the user). Setting the command to none disables this option entirely. Note that CheckHostIP is not available for connects with a proxy command.

This directive is useful in conjunction with nc(1) and its proxy support. For example, the following directive would connect via an HTTP proxy at 192.0.2.0:

ProxyCommand /usr/bin/nc -X connect -x 192.0.2.0:8080 %h %p
```

- [nc(1)](https://man.openbsd.org/nc) `-X` 和 `-x` 选项的的内容:

```
COPY-X proxy_protocol
Use proxy_protocol when talking to the proxy server. Supported protocols are 4 (SOCKS v.4), 5 (SOCKS v.5) and connect (HTTPS proxy). If the protocol is not specified, SOCKS version 5 is used.

-x proxy_address[:port]
Connect to destination using a proxy at proxy_address and port. If port is not specified, the well-known port for the proxy protocol is used (1080 for SOCKS, 3128 for HTTPS). An IPv6 address can be specified unambiguously by enclosing proxy_address in square brackets. A proxy cannot be used with any of the options -lsuU.
```

- [connect](https://bitbucket.org/gotoh/connect/wiki/Home#!more-detail) `-H` 和 `S` 选项的内容

```
COPY-H option specify hostname and port number of http proxy server to relay. If port is omitted, 80 is used. You can specify this value by environment variable HTTP_PROXY and give -h option to use it.

-S option specify hostname and port number of SOCKS server to relay. Like -H option, port number can be omit and default is 1080. You can also specify this value pair by environment variable SOCKS5_SERVER and give -s option to use it.

-4 and -5 is for specifying SOCKS protocol version. It is valid only using with -s or -S. Default is -5 (protocol version 5)
```

------

### 实例

接下来的操作，请按照你的**系统**，以及所需**代理协议**进行选择：

------

#### Linux 和 macOS 用户

- https 代理
- socks5 代理

编辑 `~/.ssh/config` 文件

```
COPYvim ~/.ssh/config
```

给文件加上如下内容:

```
COPYHost github.com
    User git
    ProxyCommand nc -X connect -x 127.0.0.1:7890 %h %p
```

> **解释**:
>
> - `Host` 后面 接的 `github.com` 是指定要走代理的仓库域名。
> - 在 ProxyCommand 中，Linux 和 macOS 用户用的是 `nc`。
> - `-X` 选项后面接的是 `connect` 的意思是 HTTPS 代理。
> - `-x` 选项后面加上代理地址和端口号。
> - 在调用 ProxyCommand 时，`％h` 和 `％p` 将会被自动替换为**目标主机名**和 **SSH 命令指定的端口**（`%h` 和 `%p` 不要修改，保留原样即可）。

------

#### Windows 用户

- http 代理
- socks5 代理

编辑 `~/.ssh/config` 文件

```
COPYvim ~/.ssh/config
```

给文件加上以下内容：

```
COPYHost github.com
    User git
    ProxyCommand connect -H 127.0.0.1:7890 %h %p
```

> **解释**:
>
> - `Host` 后面 接的 `github.com` 是指定要走代理的仓库域名。
> - 在 ProxyCommand 中，Windows 用户用的是 `connect` 。
> - `-H` 选项的意思是 HTTP 代理。
> - 在调用 ProxyCommand 时，`％h` 和 `％p` 将会被自动替换为**目标主机名**和 **SSH 命令指定的端口**（ `%h` 和 `%p` 不要修改，保留原样即可）。

------

## 如何取消 Git 和 ssh 的代理

这里就不多说了，说了那么多，我们无非就是修改了 2 个文件，即 `~/.gitconfig` 和 `~/.ssh/.config` ，**删除**或**注释**（在相应行的开头加上 `#` 即可）我们增加的相应内容即可完成取消代理。

------

## 参考链接

​	https://ericclose.github.io/git-proxy-config.html

1. [Git - git-config Documentation](https://git-scm.com/docs/git-config)
2. [Configure Git to use a proxy](https://gist.github.com/evantoli/f8c23a37eb3558ab8765)
3. [Tutorial: how to use git through a proxy](https://cms-sw.github.io/tutorial-proxy.html)
4. [ssh_config(5) - OpenBSD manual pages](https://man.openbsd.org/ssh_config)
5. [nc(1) - OpenBSD manual pages](https://man.openbsd.org/nc)
6. [SSH Proxy Command – connect.c](https://bitbucket.org/gotoh/connect/wiki/Home#!more-detail)
7. [How can I use SSH with a SOCKS 5 proxy?](https://superuser.com/a/454211)
8. [Git 基础——代理](https://jeshs.github.io/2019/04/git基础代理/)
9. [SSH via HTTP proxy in OSX](https://www.perkin.org.uk/posts/ssh-via-http-proxy-in-osx.html)

