---
title: 懒人实现一键上传
date: 2020-10-10 17:37:18
categories:
  - Mac,Linux,SSH
tags:
  - Mac,Linux,SSH
---

## SSHPASS

在命令行中想上传文件到远端主机都要先 ssh 登录，知识贫瘠的我手动了很多年今天终于知道了可以用 sshpass 命令先保存密码再做对应操作

```bash
# 将dist文件发送到远程
sshpass -p ${serverPass} scp -o stricthostkeychecking=no -r dist/ root@${serverIP}:/home/web/movie-trailer
```

然而 MacOS 上默认是不带这个工具的，Homebrew 也无法直接安装，stackoverflow 上找到下面 2 种方法

```bash
# 安装的版本要看对应维护的rb脚本指定的是啥
brew install hudochenkov/sshpass/sshpass
brew install http://git.io/sshpass.rb
brew install esolitos/ipa/sshpass

# 源码编译（未验证）
curl -O -L  https://sourceforge.net/projects/sshpass/files/sshpass/1.06/sshpass-1.06.tar.gz && tar xvzf sshpass-1.06.tar.gz
cd sshpass-1.06/
./configure
sudo make install
```

> 现在就可以愉快的上传了
