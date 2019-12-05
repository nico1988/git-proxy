内网使用git的时候由于某些原因导致网络不通畅或者下载速度很慢,使用git proxy可以完美的解决github访问慢的问题,体验github非一般的下载速度
## 前提
搭建自己的代理服务器,可以使用[shadowsocks](https://github.com/shadowsocks/shadowsocks)

## git clone仓库的两种方式

HTTP 形式：
git clone https://github.com/owner/git.git

SSH 形式：
git clone git@github.com:owner/git.git

一、HTTP

use http proxy
```
git config --global http.proxy "http://127.0.0.1:1080"
git config --global https.proxy "http://127.0.0.1:1080"
```
use socks5 proxy（such as Shadowsocks）
```
git config --global http.proxy "socks5://127.0.0.1:1080"
git config --global https.proxy "socks5://127.0.0.1:1080"
```

cancle proxy
```
git config --global --unset http.proxy
git config --global --unset https.proxy
```

二、SSH
修改 ~/.ssh/config(windows : C:\Users\Administrator\.ssh\config) 文件（不存在则新建）：

## Host必须是 github.com
```
Host github.com
   HostName github.com
   User git
   # 走 HTTP 代理
   # ProxyCommand socat - PROXY:127.0.0.1:%h:%p,proxyport=8080
   # 走 socks5 代理（如 Shadowsocks）
   # ProxyCommand nc -v -x 127.0.0.1:1080 %h %p
```

如果报错nc
```
Cloning into 'erlang_samples'...
/bin/sh: line 0: exec: nc: not found
ssh_exchange_identification: Connection closed by remote host
fatal: Could not read from remote repository.
```
windows10可以这样设置
```
Host github.com
    User git
    Hostname ssh.github.com
    IdentityFile ~/.ssh/id_rsa
    Port 443
    ProxyCommand connect.exe -H 127.0.0.1:1080 %h %p
    AddressFamily inet
Host ssh.github.com
    User git
    Hostname ssh.github.com
    IdentityFile ~/.ssh/id_rsa
    Port 443
    ProxyCommand connect.exe -H 127.0.0.1:1080 %h %p
    AddressFamily inet
```

测试

```
C:\Users\Administrator\Desktop
λ git clone git@github.com:nico1988/data-structure.git
Cloning into 'data-structure'...
remote: Enumerating objects: 582, done.
remote: Counting objects: 100% (582/582), done.
remote: Compressing objects: 100% (243/243), done.
remote: Total 582 (delta 246), reused 576 (delta 240), pack-reused 0
Receiving objects: 100% (582/582), 25.54 MiB | 958.00 KiB/s, done.
Resolving deltas: 100% (246/246), done.
Checking out files: 100% (427/427), done.
```
