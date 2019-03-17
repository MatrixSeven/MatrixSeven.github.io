---
title: 搭建cljsRN开发环境
toc: true
comments: true
date: 2019-3-11
tags: [clj,cljs,clojureScript,ReactNative,app]
categories: Clojue学习
---

## 使用cljs开发RN App
###  1. 使用Expo作为脚手架
使用[Expo](https://expo.io/)作为脚手架,这样不涉及原生部分可以在win/mac/linux下进行开发, 开发当然用[clojurescript](https://clojurescript.org/index).更多个方式可以到[http://cljsrn.org/](http://cljsrn.org/)
查看

### 2. 安装lein
[Leiningen](https://leiningen.org/)是Clojure的项目生命周期管理工具，就像Maven在Java中的地位一样。
Mac OSX可以使用 
```shell
brew install leiningen
```
linux下可以直接
```
$ cd /usr/local/bin
$ wget https://raw.githubusercontent.com/technomancy/leiningen/stable/bin/lein
$ chmod 755 lein
$ lein
```
然后等待一会,lein命令就可以使用了
其他系统可以到 [https://leiningen.org/#install](https://leiningen.org/#install)查看.

### 3. 创建项目
咱们使用reagent来桥接cljs和js,所以直接到自己要创建的项目的目录下,在命令行里直接输入
```
> lein new expo myapp +reagent

Generating fresh Expo project.
README.md contains instructions to get you started.
If you have any questions, you can ask us on Clojurians #cljsrn channel. http://clojurians.net
```
其中myapp为工程名称,可自由更改.

### 4. 启动项目
在项目根目录执行命令安装相关依赖并启动expo服务
```shell
> npm install
##或者使用 yarn/cnpm
> expo start

There is a new version of expo-cli available (2.11.7).
You are currently using expo-cli 2.6.5
Run `npm install -g expo-cli` to get the latest version
[11:51:25] Starting project at /Users/evs/demo/myapp
[11:51:26] Expo DevTools is running at http://localhost:19002
[11:51:26] Opening DevTools in the browser... (press shift-d to disable)
[11:51:33] Starting Metro Bundler on port 19001.
[11:51:33] Metro Bundler ready.
```

使用emacs打开项目 ,会发现已经生成好了项目骨架,其中src/myapp/core.cljs为入口
![1](/images/posts/搭建cljsRN开发环境/1.png)
然后启动项目repl
![1](/images/posts/搭建cljsRN开发环境/2.png)
启动完成后依次在repl中输入
```clojure
(use 'figwheel-sidecar.repl-api)
(start-figwheel!)
(cljs-repl)
```
如图:
![1](/images/posts/搭建cljsRN开发环境/4.png)
这个样子就已经切换到cljs-repl了.

`start-figwheel!`会自动构建cljs到js,第一次启动会很慢所以有可能等待编译加载构建
![1](/images/posts/搭建cljsRN开发环境/6.png)

这是打开手机端expo app,会自动发现局域网内的expo服务,或者直接返回到`expo start`的终端,会看到一个二维码,扫描一下即可加载.
![1](/images/posts/搭建cljsRN开发环境/8.png)
点击后就看到咱们的cljs-rn界面了
![1](/images/posts/搭建cljsRN开发环境/7.png)

### 5. 简单尝试
在repl窗口执行了`(cljs-repl)`后,咱们就可动态的做一些状态上的改变,而这些完全不用完全热加载/编译整个Js文件.
下面是官方给出的默认界面.咱们可以使用类似于发布订阅的机制来刷新UI改变状态,当然,在改布局情况下还是重新加载的.
```clojure
(defn alert [title]
    (.alert Alert title))

(comment
    (alert "hi cljs-rn"))
```
执行后将在弹出 `"hi cljs-rn"`. 


### 6. 相关链接
[http://cljsrn.org/](http://cljsrn.org/)

[https://expo.io/](https://expo.io/)

[https://github.com/bhauman/lein-figwheel](https://github.com/bhauman/lein-figwheel)

[https://github.com/bhauman/lein-figwheel](https://github.com/bhauman/lein-figwheel)

[https://github.com/reagent-project/reagent](https://github.com/reagent-project/reagent)

[https://github.com/seantempesta/expo-cljs-template](https://github.com/seantempesta/expo-cljs-template)