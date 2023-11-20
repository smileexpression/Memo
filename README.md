# Memo
 A repo to store solutions to problems I've encountered on the way to learning.

## Problems

### 在ubuntu 18.04/22.04使用clash-linux-amd64-v1.2.0（弃用，不方便）

#### 下载clash-linux-amd64-v1.2.0

[clash下载地址 github](https://github.com/Dreamacro/clash/releases)

[别人下载好的](https://disk.pku.edu.cn/#/link/60D2F38BE69B49D11C6B32FEB32F31A3)

```bash
cd ~

mkdir clash

cd clash
// 将下载好的clash安装包放到这个文件夹

gzip -d clash-linux-amd64-xx.gz
// 将clash-linux-amd64-xx.gz更换成你下载的版本

chmod +x clash-linux-amd64-xx
```

#### 配置clash

在windows下获取clash的config.yaml和Country.mmdb

##### 获取config.yaml

文件路径

```bash
C:\Users\你的名字\.config\clash\profiles
```

选择最新的.yml文件，如1694746034143.yml，注意不是list.yml。将这个文件复制一份，重命名为config.yaml。

##### 获取Country.mmdb

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

#### 配置ubuntu网络代理

配置系统网络代理Network Proxy

HTTP Proxy 127.0.0.1 7890

HTTPS Proxy 127.0.0.1 7890

Socks Host 127.0.0.1 7891

其余默认。

#### 运行

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

#### 终端长期使用

长期生效需要将上面两条命令写入~/.bashrc文件中，写到最后即可。

#### 配置APT代理（没试过）

终端中的APT包管理器默认不使用系统代理。为了让APT也通过Clash进行网络连接，您需要编辑APT的配置文件。打开终端并运行以下命令编辑`apt.conf`文件：

```bash
sudo nano /etc/apt/apt.conf

// 在打开的文件中添加以下内容：
Acquire::http::Proxy "[http://127.0.0.1:7890/](http://127.0.0.1:7890/)";  
Acquire::https::Proxy "[http://127.0.0.1:7890/](http://127.0.0.1:7890/)";
```

### 在ubuntu 18.04/22.04使用Clash.for.Windows-0.20.39-x64-linux

最后一版clash，哀悼一秒。

#### 下载Clash.for.Windows-0.20.39-x64-linux

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

#### 配置ubuntu网络代理

配置系统网络代理Network Proxy

HTTP Proxy 127.0.0.1 7890

HTTPS Proxy 127.0.0.1 7890

Socks Host 127.0.0.1 7891

其余默认。

#### 运行

```bash
cd clash
./cfw
```

#### 导入订阅链接

和windows下使用没什么不同，懂得都懂。
