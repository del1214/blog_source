title: 使用mac共享$$给其他设备
date: 2018-08-04 11:08:52
categories:
- ShadowSocks
tags:
- 翻墙
- ShadowSocks
- privoxy
---

1.下载privoxy

```bash
brew install privoxy
```

2.修改/usr/local/etc/privoxy/config

- 搜索到“forward-socks5t   /”（不含双引号）那一行，去掉注释的符号，把端口改为1086（系SS的SOCKS5端口）

  `forward-socks5t   /               127.0.0.1:1086 .`

- 搜索到“listen-address  127.0.0.1:8118”（不含双引号）那一行，去掉注释的符号，把127.0.0.1改为0.0.0.0（否则只能作用于本机），端口号默认或选择一个未占用的端口

  `listen-address  0.0.0.0:8118`

3.在终端中运行

```bash
cd /usr/local/sbin/
./privoxy --no-daemon /usr/local/etc/privoxy/config
```

4.使用
将设备和Mac接入同一个局域网，并在设备的Wi-Fi设置里开启手动代理，代理服务器主机名是Mac的局域网地址，端口是刚才在config里面设置的端口号(8118)。 
可以在浏览器中打开ip.cn查看当前IP，如果是SS服务器的IP，则成功。