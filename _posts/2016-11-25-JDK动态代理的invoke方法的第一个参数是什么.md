---
layout: post
title: JDK动态代理的invoke方法的第一个参数是什么
# date: 2016-11-25
# toc: true
# comments: true
keywords: java,动态代理
description: java,动态代理
categories: [java]
---
**本文由博主原创,转载请注明出处**
在知乎回答的一个问题,不过跑题了,但是还是有些价值的,搬到博客,原文链接
[java InvocationHandler invoke方法的第一个参数有什么用?](https://www.zhihu.com/question/52551525/answer/132978844)

java InvocationHandler invoke方法的第一个参数有什么用?
想知道它有什么用,就必须的先知道它到底是什么...
<!--more-->
那InvocationHandler invoke的第一个参数到底是什么呢?
这是 OpenJDK7中InvocationHandler.java接口的注释中的部分:
```
* @param   proxy the proxy instance that the method was invoked 
```
看样子是 代理类本身的一个实例.那代理类是什么梗?
其实代理类就是动态代理动态生成的类.
我自己写了一个动态代理,基本都差不多,就不全贴代码了,那么下面在InvocationHandler接口的invoke你增加点内容:
```java
 public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println(proxy.getClass().getName());
        return method.invoke(object,args);
    }
```
看下输出:\
![img](1.png)


是个:com.sun.proxy.$Proxy0类的实例.
那么这个类究竟是怎么来的呢?
动态代理这个黑盒子又到底做了什么?
生成的这个类为何叫做代理类的实例?
其实动态代理的具体实现(大部分)在ProxyGenerator.java类里.我这里说个大概套路.
其实本质做的事情就是合成你要的代理类,用什么合成,就是你传入的InvocationHandler接口的实现类和要代理的接口.这也是为什么咱们要自己保存要被代理的对象:
```java
public class MyProxy implements InvocationHandler{
    Object object;

    public MyProxy(Object object) {
        this.object = object;
    }
```
因为生成的代理类就是个空壳子,下面修改下invoke方法.咱们把生成的字节码保存起来
```java
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println(proxy.getClass().getName());
        byte[]  b= ProxyGenerator.generateProxyClass(proxy.getClass().getSimpleName(),proxy.getClass().getInterfaces());
        FileOutputStream out = new FileOutputStream("./"+proxy.getClass().getSimpleName()+".class");
        out.write(b);
        out.flush();
        out.close();
        return method.invoke(object,args);
    }
```
调用代理后,成功拿到$Proxy0.class\
![img](2.png)\
然后直接反编译,这个东西到底是什么...\
这是一个实现了我在生成代理传入的接口的class的类..\
然后看下如何找到这个invoke的
```java
  public final int get(String var1) throws  {
        try {
            return ((Integer)super.h.invoke(this, m4, new Object[]{var1})).intValue();
        } catch (RuntimeException | Error var3) {
            throw var3;
        } catch (Throwable var4) {
            throw new UndeclaredThrowableException(var4);
        }
    }
```
....super.h就是你实现的InvocationHandler具体实例,那么很明显第一个传入的第一个参数就是this喽.
this很明显就是生成代理类的实例,也就是楼主想要的知道的类型.

然后invoke里面的method是怎么拿到的?
在生成的代理类的static块你有如下部分:
```java
 m1 = Class.forName("java.lang.Object").getMethod("equals", new Class[]{Class.forName("java.lang.Object")});
 m2 = Class.forName("java.lang.Object").getMethod("toString", new Class[0]);
 m3 =.......
........你接口声明的方法++++.........
```
jdk动态代理只能代理接口怪我喽?


补充:我好像看跑题了,,,,,,第一个参数有什么用,第一个参数有什么用,第一个参数有什么用第一次
第一个参数卵用都没有,只有我这种闲的蛋疼的人....反编译看看...


=======================================
生成的代理类本身继承了Proxy,因此也一定能拿到传入的代理对象和自己写的Handle
```java
        Field f = proxy.getClass().getSuperclass().getDeclaredField("h");
        f.setAccessible(true);
        System.out.println(f.get(proxy).getClass() + "------" + f.get(proxy));
```

不过个人觉得纯属智障行为.


不过经过查阅资料,有传说:

（有点个人猜测）因为InvocationHandler是proxy对象的参数，在proxy调用某个目标对象的方法时，可能会采用回调机制调用InvocationHandler的invoke(Object proxy, Method method, Object[] args)方法，这个时候proxy对象则成为形式参数。（这方面的回调在jdk中比较多，例如filenamefilter，比较器，所以猜测了一下   ：） ）


