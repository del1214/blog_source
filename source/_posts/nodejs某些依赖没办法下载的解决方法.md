title: nodejs某些依赖没办法下载的解决方法
date: 2018-08-13 17:23:34
tags:
---

# 众所周知的原因,某些包不设置环境变量是下载不了的

## 方法一 使用淘宝镜像

> 解决方法

```bash
npm config set sass_binary_site=https://npm.taobao.org/mirrors/node-sass/
npm config set phantomjs_cdnurl=https://npm.taobao.org/mirrors/phantomjs/
npm config set electron_mirror=https://npm.taobao.org/mirrors/electron/
```

> 这样使用 npm i 时会从淘宝源下载

## 挂代理

```bash
export https_proxy="http://127.0.0.1:1087"
export http_proxy="http://127.0.0.1:1087"
```
