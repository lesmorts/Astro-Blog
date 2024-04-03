---
title: Android Studio环境部署
published: 2024-03-29
comment: false 
tags: [百度地图AndroidSDK, Android Studio, java]
category: 百度地图SDK
audio: false
---
> 注：本文的前置条件为已安装Android Studio、版本适配的java和完成百度地图开放平台的开发者认证

# 创建Android Studio项目

1. 项目选择Empty Views Activity
2. 编辑项目名称、包名、存储位置和语言(默认Java)

   ![](https://lesmorts.github.io/picx-images-hosting/1.7smzblu6zo.webp)
3. 其余项默认

# [在百度地图开放平台申请应用](https://lbsyun.baidu.com/apiconsole/key/create#/home)

1.应用类型选择Android SDK

2.[获取SHA1码](https://lbsyun.baidu.com/faq/api?title=androidsdk/guide/create-project/ak#获取SHA1)

3.填写创建的Android Studio项目的包名

# 在项目中集成BaiduMapSDK

> 本文采用本地集成的方式

## 下载开发包

功能选择：

- 定位SDK：全量定位
- 地图SDK：基础地图，检索POI
- 导航SDK：驾车导航

**选择AAR包格式**

## 添加arr文件

打开解压后的文件夹将 `NaviTts.aar`,`onsdk_all.aar`和libs文件夹中的 `BaiduLBS_Android.aar`复制到项目中

![](https://lesmorts.github.io/picx-images-hosting/2.3ye7snc92d.webp)

在app/中的 `build.gradle.kts`文件中的Dependencies添加引入aar包

```kts
implementation(fileTree("libs"))
```

![](https://lesmorts.github.io/picx-images-hosting/3.6f0g7kj4yu.webp)

## 添加so文件

复制解压后的文件夹中libs文件夹中的这些文件夹

![](https://lesmorts.github.io/picx-images-hosting/4.b8o54ggkj.webp)

在app/src/main中添加名为**jniLibs**的文件夹，将复制的文件夹粘贴于此

4.在jniLibs文件夹中添加assets文件夹

将 `BaiduLBS_Android.aar`作为压缩包打开（可改名.zip）复制其中assets内容到新添加的assets文件夹中

完成配置！
