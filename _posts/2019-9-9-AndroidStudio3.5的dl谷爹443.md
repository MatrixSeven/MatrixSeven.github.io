---
title: AndroidStudio3.5的dl谷爹443
toc: true
comments: true
date: 2019-9-9
tags: [andorid,google,443,app]
categories: 杂记
---
### 万恶的dl.google.com:443
万恶的网络，更新到AndroidStudio到3.5后又出现了`sync project dl.google.com:443`这个问题，之前版本有一个
setting Build, Execution, Deployment --> Gradle --> Android Studio 里勾选 Enable embedded Maven repository，但是在3.5里面已经没有这个选项,经过各种艰辛，发现修改`～/.gradle/gradle.properties就可以可。
原值：
```conf
systemProp.https.proxyPort=1080
systemProp.http.proxyHost=127.0.0.1
systemProp.https.proxyHost=127.0.0.1
systemProp.http.proxyPort=1080

```
修改为：
```conf
#systemProp.https.proxyPort=1080
systemProp.http.proxyHost=127.0.0.1
#systemProp.https.proxyHost=127.0.0.1
systemProp.http.proxyPort=1080

```
对，就是干掉这两个`https`，对了，要打开代理
