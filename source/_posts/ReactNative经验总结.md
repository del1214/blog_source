---
title: ReactNative 经验总结
date: 2020-09-09 14:58:32
tags:
---

## 什么是 ReactNative

### ReactNative 提供组件，可以使用 javascript 和 css 编写原生移动应用

### ReactNative 有平台差异性，相同组件样式不同、交互不同、api 不同

### ReactNative 仅仅是绘制界面，绝大部分功能（地图、音乐、视频、文件处理、社会化分享、支付）都需要依赖原生程序的支持

### ReactNative 支持 CodePush，可以向已经发布的程序推送 js 包实现热更新

## 推荐使用的库

- _[react-navigation](https://github.com/react-navigation/react-navigation)_  
  路由导航组件，更新较快，4.x 不支持动态组件。安装这个库随之必装的是 react-native-gesture-handler，react-native-reanimated，react-navigation-stack，react-navigation-tabs
- _[react-native-vector-icons](https://github.com/oblador/react-native-vector-icons)_  
  图标控件

- _[react-native-swiper](https://github.com/leecade/react-native-swiper)_  
  Swiper 组件

- _[react-native-webview](https://github.com/react-native-community/react-native-webview)_  
  WebView 组件

- _[@react-native-community/async-storage](https://github.com/react-native-community/async-storage)_  
  本地缓存组件

- _[react-native-svg](https://github.com/react-native-community/react-native-svg)_  
  配合*react-native-svg-transformer*可以编写 svg 组件

- _[react-native-syan-image-picker](https://github.com/syanbo/react-native-syan-image-picker)_  
  图片选择、拍照组件, 因为 react-native-file-selector 中引用的 FileBrowser 有问题，暂时弃用

- _axios_  
  通信组件，RN 官方推荐的 Fetch 不支持拦截器

- _rn-fetch-blob_
  网络传输文件

- _jpush_ _jshare_ _jcore_  
  极光推送，极光分享， 有相对详细的 react-native 集成文档

- _[teaset](https://github.com/rilyu/teaset)_  
  国人写的 UI 组件

- _react-native-wechat_
  微信登录、支付、分享

- _react-native-audio_  
  播放音频文件

- _react-native-video_  
  播放视频文件

- _react-native-sound_  
  控制音量

- _react-native-orientaion_  
  控制屏幕方向

- [react-native-image-zoom-viewer](https://github.com/ascoders/react-native-image-viewer)  
  点击图片预览、放大、手势关闭

- [lottie-react-native](https://github.com/react-native-community/lottie-react-native)  
  播放 AE 动效

- [react-native-swipe-list-view](https://github.com/jemise111/react-native-swipe-list-view)  
  侧滑列表

- [rn-placeholder](https://github.com/mfrachet/rn-placeholder)  
  骨架屏

## 环境配置

- MAC OS 10.15
- Xcode11
- AndroidStudio 3.5
  按照[指南](https://reactnative.cn/docs/getting-started.html)完成环境配置

难点

- ios
  [CocoaPods](https://mirror.tuna.tsinghua.edu.cn/help/CocoaPods/) 的仓库国内很难访问到，可以用先下载到本地的方式完成配置

- android
  在 android/gradle.properties 文件中配置如下内容

```properties
# 使用代理
systemProp.http.proxyHost=127.0.0.1
systemProp.https.proxyHost=127.0.0.1
systemProp.http.proxyPort=1087
systemProp.https.proxyPort=1087
```

> 总之做 React Native 开发需要十分稳定的\$\$代理

### 编译问题

#### ios

使用 CocoaPods 的项目需要在 ios 目录下执行 pod install 安装全部依赖
然后打开.xcworkspace 文件编译

#### android

android 打包需要下载对应版本的 gradle 和 jar 包，强烈依赖翻墙

### 依赖库

#### react-native-fast-image

[AppGlideModule 问题](https://github.com/DylanVann/react-native-fast-image/blob/master/docs/app-glide-module.md)
在 build.gradle 中加入下面代码

```gradle
project.ext {
    excludeAppGlideModule = true
}
```

#### react-native-vector-icons

字体安装

##### ios

- 在 Xcode 中新建 Group(新建的 Group 并不是一个真实的文件夹)，然后将字体文件拉进去
- 在 Info.plist 文件中添加 key，Fonts provided by application
- 上面的 key 内添加项，每项的值都是字体名字,如 Ionicons.ttf,AntDesign.ttf

##### android

将字体放到 app/src/main/assets/fonts 目录下

#### react-native-amap3d

[安卓模拟器蓝屏问题](http://lbsbbs.amap.com/forum.php?mod=viewthread&tid=42965)  
 [解决方法](https://blog.csdn.net/weixin_38202389/article/details/80782861)

### 权限

#### ios

编辑 Info.plist 加入下列内容

```plist
<key>NSLocationWhenInUseUsageDescription</key>
<string>启用定位更加准确</string>
<key>NSCameraUsageDescription</key>
<string>需要打开您的相机</string>
<key>NSPhotoLibraryUsageDescription</key>
<string>需要打开您的相册</string>
```

#### android

编辑 app/src/main/AndroidManifest.xml

```xml
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.CAMERA" />
```

> 需要深入理解

#### react-native-gesture-handler

TouchableOpacity 在安卓下没有 onPress 事件，因为要引用 react-native 中的同名类，自动引用总是引错

### 启动图片设置

[点我观看](https://medium.com/handlebar-labs/how-to-add-a-splash-screen-to-a-react-native-app-ios-and-android-30a3cec835ae)

### app 保持 portrait 模式

#### ios

Xcode 选择 TARGETS 项目,General 下的 Deployment Info 下的 DeviceOrientation 只勾选 Portrait

#### android

AndroidManifest.xml 中为 MainActivity 添加 android:screenOrientation="portrait"

### 开源项目 example 的问题

很多开源项目都带有 example 目录方便查看效果，但因为 react-native 版本更新迭代相当快，如果不满足当前 react-native 版本的基本无法运行

### react-native 版本更迭问题

react-native 更新非常快，应保持项目依赖的 react-native 大版本稳定在半年至一年更新，包括 android 和 ios 项目格式的更新

# shuttle_bus_app_passenger 端

> 班车 APP 乘客

## 运行方法

```bash
# 安装依赖包
yarn

# 启动jsbundle服务器
yarn start

# 启动ios端
# 等与 react-native run-ios
yarn ios

# 启动android端
yarn android

# 清除安卓编译缓存
yarn clean:android

# 构建安卓
# 构建结果在android/app/build/outputs/apk/release/中
yarn build:android

# 构建ios
# 构建完成后将ios/bundle拖入xcode ${project_name}中
yarn build:ios

# 调试
yarn devtool
# 先运行devtool后运行模拟器可以进行界面调试
# 也可以自行安装后运行
# npm i -g react-devtools@3.6.3
# 安装也许会遇到electron无法下载的问题，先用npx方式完成缓存加载之后也许可安装或者挂代理

# 执行测试用例
yarn test

# 执行语法lint
yarn lint

# 切换开发环境到STAGE环境
STAGE=xxx yarn env:dev

# 切换打包环境到STAGE环境
STAGE=xxx yarn env:stage

# 给bin/下所有sh执行权限
yarn chmod

# 用工具给项目打版本号，git不提交但会自动打tag
yarn version
```

## 终极清缓存秘籍

```bash
# 可以清除修改了js构建方案后之前的顽固缓存
watchman watch-del-all && rm -rf $TMPDIR/react-native-packager-cache-* && rm -rf $TMPDIR/metro-bundler-cache-* && npm cache clean --force && npm start -- -- reset-cache
```

## react-native-splash-screen

集成后安卓闪退问题，需要配置 res 下的 xml 文件

## 安卓 keystore 生成

```gradle
    signingConfigs {
        release {
            storeFile file('./android/app/keystore')
            storePassword '12345678'
            keyAlias 'passenger'
            keyPassword '12345678'
        }
    }
```

```bash
# 生成keystore
keytool -genkey -alias shuttle_bus_app_passenger -keyalg RSA -keysize 2048 -validity 36500 -keystore keystore
# 查看keystore
keytool -list -v -keystore release.keystore
```

## react-native-wechat 集成问题

> 版本 1.9.12

### ios

因为 ReactNative0.59 已经支持自动 link 不需要进行手动链结动态库进来
其他步骤照旧

- 手动链结库（无需配置）
- 配置 URL Types
- 配置 Info.plist
- 配置 AppDelegate.m

### android

因为自动 link，不需要编辑 1、2 步

- 配置 android/settings.gradle（无需配置）
- 配置 android/app/build.gradle（无需配置）
- 配置 proguard-rules.pro
- 配置 MainApplication.java
- 在自己的子 package 下建立 wxapi 包
- 配置微信分享，新建 WXEntryActivity，配置 AndroidManifest.xml
- 配置微信支付，新建 WXPayEntryActivity，配置 AndroidManifest.xml

> [The modules ['node_modules-react-native-wechat-lib-android-RCTWeChat', 'node_modules-react-native-wechat-lib-android-react-native-wechat-lib~1'] point to the same directory in the file system.
> Each module must have a unique path.](https://github.com/little-snow-fox/react-native-wechat-lib/issues/13)不要配置上面的 1、2 步
> [安卓打包提示 WeChatModule.java:7: 错误: 程序包 android.support.annotation 不存在](https://github.com/yorkie/react-native-wechat/issues/537) 进入 WeChatModule 包修改文件

```java
//import android.support.annotation.Nullable;
import androidx.annotation.Nullable;
```

## jpush-react-native

> jcore-react-native 1.7.5
> jpush-react-native 2.7.5

按文档完全可配置，如何调用需测试

## 自定义图标字体

[教程](https://www.jianshu.com/p/c4dfc85d3009)

> xcode 要将 icommon.ttf 拖拽到项目根路径, 在 info.plist 中添加 Fonts provided by application > icomoon.ttf

## 弹窗制作

方法 1: 高阶组件套壳
通过高阶组件在外部定义弹窗组件通过 redux state 触发渲染
优点：稳定
缺点：state 变更后会触发大面积的高阶组件重绘

方法 2: 使用 react-native-root-siblings
优点：快，直接提供了插入到根路径的功能，方便编写
缺点：hack 了 AppRegistry.registerComponent 方法，不知道新版本绘有何问题。所有组件需做好退回 hoc 编写的方案上

我选择快[Doge]

## aliyun-oss-react-native

当前版本：1.0.0-alpha.7

### ios 安装

ios 目录下 pod install

### android 安装

在 android/settings.gradle 添加代码

```gradle
include ':aliyun-oss-react-native'
project(':aliyun-oss-react-native').projectDir = new File(rootProject.projectDir, '../node_modules/aliyun-oss-react-native/android')
```

在 android/app/build.gradle 里增加代码

````gradle
dependencies {
    implementation project(':aliyun-oss-react-native')
}

遇到这种报错的
```bash
Manifest merger failed : Attribute application@allowBackup value=(false) from AndroidManifest.xml:16:7-34
	is also present at [com.aliyun.dpa:oss-android-sdk:2.9.3] AndroidManifest.xml:18:9-35 value=(true).
	Suggestion: add 'tools:replace="android:allowBackup"' to <application> element at AndroidManifest.xml:7:5-117 to override.
````

在 AndroidManifest.xml 中添加

```xml
<manifest
    xmlns:android="http://schemas.android.com/apk/res/android"
+   xmlns:tools="http://schemas.android.com/tools"
  package="com.ccbuluo.shuttle_bus_app_passenger">

<application
    android:name=".MainApplication"
    android:label="@string/app_name"
    android:icon="@mipmap/ic_launcher"
    android:roundIcon="@mipmap/ic_launcher_round"
    android:allowBackup="false"
    android:theme="@style/AppTheme"
+   tools:replace="android:allowBackup">

```

[./gradlew assembleRelease 失败 #49](https://github.com/aliyun/aliyun-oss-react-native/issues/49)
Execution failed for task ':aliyun-oss-react-native:verifyReleaseResources'.

修改

```xml
xmlns:tools="http://schemas.android.com/tools">
    tools:replace="android:allowBackup"
    android:allowBackup="true"
```

修改 node_modules/aliyun-oss-react-native/android/build.gradle

```gradle
android {
    <!-- 改成和自己的版本一致 -->
    compileSdkVersion 28
    buildToolsVersion '28.0.3'
}
```

## Lottie

### android

lottie 中带图片的需要在安卓对应目录下拷贝资源文件到[PROJECT FOLDER]/android/app/src/main/assets，在 react prop 中对应其子目录 imageAssetsFolder={'lottie/animation_1'}

### ios

在 Xcode 中左侧文件夹右键 "Add file to [Project]" 将文件添加进去重新编译即可

> 文件名需和 json 中的文件一致

## 编译打包

### android

安卓虚拟机堆栈爆满问题
Expiring Daemon because JVM heap space is exhausted

在 android/app/build.gradle 中添加

```gradle
android {
    dexOptions {
       javaMaxHeapSize "3g"
    }
    ...
}
```

在 android/gradle.properties 中添加

```properties
org.gradle.daemon=true
org.gradle.jvmargs=-Xmx2560m
```

<!-- 安卓真机调试 -->

adb logcat AndroidRuntime:E \*:S

手机插上线，打开 Android Studio logcat 面板 过滤 I/ReactNativeJS 级别 debug

启动图标

安卓启动图（内容尽量靠中间）
2560\*1440
安卓图标
圆形，圆角
192x192
144x144
96x96
72x72
48x48

ios

### ios xcode 升级带来的问题

#### Module compiled with Swift 5.1.3 cannot be imported by the Swift 5.2 compiler

这个问题是因为升级了新的编译器，之前已经用旧编译器生成过结果需要进入/Users/\${username}/Library/Developer/Xcode/DerivedData 将数据清除

也可以清除 Pods/目录，重新 pod install 一下
