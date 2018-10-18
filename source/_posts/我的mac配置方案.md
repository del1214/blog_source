---
title: 我的mac配置方案
date: 2015-08-05 10:18:36
categories:
- Mac
tags:
- Mac
- Homebrew
- Homebrew cask
- proxychains4
- node
---

# 这是一篇快速配置全新Macbook到前端开发环境的文章。我参考了很多配置macbook的文章，在此就不一一致谢了。

## 触摸板

### 光标与点按

* 轻拍来点按
* 辅助点按
* 查找
* 三指拖移（配备Force Touch的电脑此选项已被转移至辅助功能 > 鼠标与触摸板）

### 滚动缩放

* 默认即可

### 更多手势

* 默认即可

---

## Dock

将默认放在Dock上的app全部脱出，保持最精简

---

## Finder

### Finder > 显示

* 显示路径栏

### Finder > 偏好设置 > 通用

* 开启新Finder窗口时打开：Home『用户名』目录

### Finder > 偏好设置 > 边栏

* 勾选Home『用户名』目录
* 创建工作目录到侧边栏

### Finder > 偏好设置 > 高级

* 显示所有文件扩展名（勾选）
* 更改扩展名之前显示警告（不勾选）
* 清倒废纸篓之前显示警告（勾选）
* 安全清倒废纸篓（不勾选）
* 执行搜索时：搜索当前文件夹

### 系统偏好设置 > 键盘 > 快捷键

* 全键盘控制：所有控制
* 输入源：选择上一个输入源  command空格键
* Spotlight: 显示Spotlight搜索 contral空格键

---

## Xcode

* 快速安装command line tool，在终端中运行：

```bash
xcode-select --install
```

* 从App store 安装Xcode，不完整安装Xcode某些node包在编译时会报错

---

## Homebrew

Mac的包管理工具，安装前要确保 command line tool安装完成

在终端中输入如下命令：

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

---

## Fxck GFW

### 安装你的Shadowsocks翻墙工具

### 安装Proxychains-ng（终端翻墙）

```bash
brew install proxychains-ng
```

### 配置Proxychains-ng

```bash
vi /usr/local/etc/proxychains.conf
#最后一行改为 socks5 ip port
socks5 127.0.0.1 9742
```

### 使用Proxychains-ng

```bash
proxychains4 curl www.twitter.com
```

---

## Homebrew Cask

不建议使用此君安装图形app，装几个插件即可

### 文件预览插件

```bash
brew cask install qlcolorcode
brew cask install qlstephen
brew cask install qlmarkdown
brew cask install quicklook-json
brew cask install qlprettypatch
brew cask install quicklook-csv
brew cask install betterzipql
brew cask install webp-quicklook
brew cask install suspicious-package
```

### 偏好面板

```bash
brew cask install launchrocket  服务管理
brew cask install hosts   host管理
```

## 终端

### 安装[iTerm2](http://iterm2.com/ "iTerm2")

### 安装[prezto](https://github.com/sorin-ionescu/prezto "prezto")

---

## Git,Node

命令行中输入：

```bash
brew update
brew install git
brew install node
brew install python
brew install ruby
```

## 必备软件

* 开发工具：
  * Google Chrome
  * Sublime Text 3
  * WebStorm
  * Parallels Desktop
  * Photoshop
  * Sketch
* 生产力工具：
  * Alfred 2
  * Dash
  * 1Password
  * Manico
  * Moom
  * Mou
  * MS Office