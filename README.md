# Memo
 A repo to store solutions to problems I've encountered on the way to learning.

## Problems

### 在ubuntu 18.04/22.04使用clash-linux-amd64-v1.2.0（弃用，不方便）

**下载clash-linux-amd64-v1.2.0**

[clash下载地址 github](https://github.com/Dreamacro/clash/releases)（已跑路）

[别人下载好的](https://disk.pku.edu.cn/#/link/60D2F38BE69B49D11C6B32FEB32F31A3)（已失效）

```bash
cd ~

mkdir clash

cd clash
// 将下载好的clash安装包放到这个文件夹

gzip -d clash-linux-amd64-xx.gz
// 将clash-linux-amd64-xx.gz更换成你下载的版本

chmod +x clash-linux-amd64-xx
```

**配置clash**

在windows下获取clash的config.yaml和Country.mmdb

**获取config.yaml**

文件路径

```bash
C:\Users\你的名字\.config\clash\profiles
```

选择最新的.yml文件，如1694746034143.yml，注意不是list.yml。将这个文件复制一份，重命名为config.yaml。

**获取Country.mmdb**

文件路径

```bash
C:\Users\你的名字\.config\clash
```

看到Country.mmdb。

将刚才重命名的config.yaml和Country.mmdb放到ubuntu系统下。

```bash
mkdir    ~/.config/clash
sudo mv   Country.mmdb    ~/.config/clash
sudo mv    config.yaml     ~/.config/clash
```

**配置ubuntu网络代理**

配置系统网络代理Network Proxy

HTTP Proxy 127.0.0.1 7890

HTTPS Proxy 127.0.0.1 7890

Socks Host 127.0.0.1 7891

其余默认。

**运行**

```bash
cd ~/clash
./clash-linux-amd64-xx
```

注意：此时可以使用浏览器访问谷歌，但是终端网络不会经过clash。需要

```bash
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890
```

且仅在当前终端生效。

**终端长期使用**

长期生效需要将上面两条命令写入~/.bashrc文件中，写到最后即可。

**配置APT代理（没试过）**

终端中的APT包管理器默认不使用系统代理。为了让APT也通过Clash进行网络连接，您需要编辑APT的配置文件。打开终端并运行以下命令编辑`apt.conf`文件：

```bash
sudo nano /etc/apt/apt.conf

// 在打开的文件中添加以下内容：
Acquire::http::Proxy "[http://127.0.0.1:7890/](http://127.0.0.1:7890/)";  
Acquire::https::Proxy "[http://127.0.0.1:7890/](http://127.0.0.1:7890/)";
```

### 在ubuntu 18.04/22.04使用Clash.for.Windows-0.20.39-x64-linux

最后一版clash，哀悼一秒。

**下载Clash.for.Windows-0.20.39-x64-linux**

[下载地址](https://archive.org/download/clash_for_windows_pkg)

下载Clash.for.Windows-0.20.39-x64-linux.tar.gz

放到ubuntu

执行解压命令

```bash
解压
tar -zxvf xxx.tar.gz
改下文件夹名
sudo mv Clash\ for\ Windows-0.20.39-x64-linux clash
```

**配置ubuntu网络代理**

配置系统网络代理Network Proxy

HTTP Proxy 127.0.0.1 7890

HTTPS Proxy 127.0.0.1 7890

Socks Host 127.0.0.1 7891

其余默认。

**运行**

```bash
cd clash
./cfw
```

**导入订阅链接**

和windows下使用没什么不同，懂得都懂。

### 多电脑使用相同虚拟机

[VMware虚拟机从一台电脑复制到另一台电脑](https://zhuanlan.zhihu.com/p/603483636#:~:text=%E5%9C%A8%E4%B8%80%E5%8F%B0%E7%94%B5%E8%84%91%E4%B8%8A,%E7%94%B5%E8%84%91%E4%B8%8A%EF%BC%8C%E7%9C%81%E6%97%B6%E7%9C%81%E5%8A%9B%E3%80%82)

### 虚拟机迁移后无法联网

虚拟机迁移后遇到突然连不上网的问题

1. 宿主windows中，settings -> Betwork & internet -> Advanced network settings，允许虚拟机的网络适配器

2. 虚拟机中

   ```bash
   sudo dhclient ens33
   ```

### vscode设置自动换行

linux/windows中 `ctrl + ,` 搜索word wrap，改为on

### Ubuntu 18.04/20.04默认全局缩放修改

[Ubuntu 20.04默认全局缩放修改，简单五步即可实现](https://blog.csdn.net/Pthumeru/article/details/119304019)

### win11使用wsl

**方法一：直接下载**

直接下载，会自动安装最新的ubuntu

```bash
wsl -l
```

**方法二：导入**

**导入**

```bash
wsl --import <Distribution Name> <InstallLocation> <FileName>
```

例如

```bash
wsl --import compilers D://Software/SoftwareData/compilers D://Software/SoftwareData/compilers/compilers.tar
```

**运行**

```bash
wsl -d <Distribution Name>
```

**设置用户账户**

```bash
NEW_USER=<USERNAME> // <USERNAME>替换成你的用户名
useradd -m -G sudo -s /bin/bash "$NEW_USER"
passwd "$NEW_USER"
```

**设定默认用户**

```bash
tee /etc/wsl.conf <<_EOF
[user]
default=${NEW_USER}
_EOF
```

#### 问题

```bash
wsl: A localhost proxy configuration was detected but not mirrored into WSL. WSL in NAT mode does not support localhost proxies.
```

**方法一：开启TUN MODE**

把clash或其他代理客户端开TUN MODE。

**方法二：WSL2配置代理**

**新建脚本`proxy.sh`**

```sh
#!/bin/sh
hostip=$(cat /etc/resolv.conf | grep nameserver | awk '{ print $2 }')
wslip=$(hostname -I | awk '{print $1}')
port=7890
 
PROXY_HTTP="http://${hostip}:${port}"
 
set_proxy(){
  export http_proxy="${PROXY_HTTP}"
  export HTTP_PROXY="${PROXY_HTTP}"
 
  export https_proxy="${PROXY_HTTP}"
  export HTTPS_proxy="${PROXY_HTTP}"
 
  export ALL_PROXY="${PROXY_SOCKS5}"
  export all_proxy=${PROXY_SOCKS5}
 
  git config --global http.proxy ${PROXY_HTTP}
  git config --global https.proxy ${PROXY_HTTP}
 
  echo "Proxy has been opened."
}
 
unset_proxy(){
  unset http_proxy
  unset HTTP_PROXY
  unset https_proxy
  unset HTTPS_PROXY
  unset ALL_PROXY
  unset all_proxy
  git config --global --unset http.proxy
  git config --global --unset https.proxy
 
  echo "Proxy has been closed."
}
 
test_setting(){
  echo "Host IP:" ${hostip}
  echo "WSL IP:" ${wslip}
  echo "Try to connect to Google..."
  resp=$(curl -I -s --connect-timeout 5 -m 5 -w "%{http_code}" -o /dev/null www.google.com)
  if [ ${resp} = 200 ]; then
    echo "Proxy setup succeeded!"
  else
    echo "Proxy setup failed!"
  fi
}
 
if [ "$1" = "set" ]
then
  set_proxy
 
elif [ "$1" = "unset" ]
then
  unset_proxy
 
elif [ "$1" = "test" ]
then
  test_setting
else
  echo "Unsupported arguments."
fi
```

注意：其中第4行的`port`更换为自己的代理端口号。

- `source ./proxy.sh set`：开启代理
- `source ./proxy.sh unset`：关闭代理
- `source ./proxy.sh test`：查看代理状态

**任意路径下开启代理**

可以在`~/.bashrc`中添加如下内容，并将其中的路径修改为上述脚本的路径：

```bash
alias proxy="source /path/to/proxy.sh"
```

然后输入如下命令：

```bash
source ~/.bashrc
```

那么可以直接在任何路径下使用如下命令：

- `proxy set`：开启代理
- `proxy unset`：关闭代理
- `proxy test`：查看代理状态

**自动设置代理**

也可以在`~/.bashrc`添加如下内容，即在每次shell启动时自动设置代理，同样的，更改其中的路径为自己的脚本路径：

```bash
. /path/to/proxy.sh set
```

使用`curl`即可验证代理是否成功，如果有返回值则说明代理成功。

```bash
curl www.google.com
```

**参考文献**

[WSL2配置代理](https://www.cnblogs.com/tuilk/p/16287472.html)

### gitee的draw.io

没有适配长名字的仓库，提交.drawio时要求输入文件名，却因为仓库名太长而导致输入框隐藏。

直接检查元素找到那一块前端代码，将仓库名改短。

### Folder name

Avoid using spaces if possible. I tried to use path as

```bash
../../lab05/Priests and Devils/README.md
```

It worked in marktext, but not in github

The final solution is to replace spaces with `%20` beacause github will automatically replace spaces in path with `%20`.

### github markdown中latex块内公式换行

https://github.com/flysnow-org/maupassant-hugo/issues/21

懒得试，自行尝试。想要在github上渲染出来，会导致pdf很丑。为了兼顾导出的pdf和github上渲染出来的效果，直接开新块了。

### vscode跟随系统切换主题

settings搜索auto detect color scheme，勾上。
