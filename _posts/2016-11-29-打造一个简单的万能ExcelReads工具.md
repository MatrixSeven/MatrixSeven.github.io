---
title: 打造一个简单的万能ExcelReads工具
date: 2016-11-29
toc: true
comments: true
tags: [POi,Excel,工具]
categories: 造轮子
---

**本文由博主柒。原创,转载请注明出处**
完整源码下载地址 [https://github.com/MatrixSeven/ExcelReads)
之前有段时间要读取Excel，而且是那种简单格式的。就是表头，行列二维的数据。
索性写了个工具，专门应付这类Excel，基于POI，完美应付xlsx xls。
嗯，这个东西诞生了，也许设计的不是很优美，但是这里还是和大家分享，纪录下自己的进步。
<!--more-->
## 1.已经完成内容
1. 能够读取行列二维的Excel
2. 能够分别读取sheel，结束开始行数
3. 能够自动映射为Map类型和自定义对象类型（目前属性都为String类型）
4. 能够支持直接映射对象实体属性
5. ~~能够支持正则匹配行数据(可能对效率有损伤，已经实现，但不建议使用)~~
7. 支持以某个列字段为Key生成Map<String,T>类型数据
8. 能够单独处理某一行数据，能够标记时候放弃某一列，能够根据列内容判断时候保留数据
9. 请使用jdk6以上版本运行

## 2.可能会添加的内容
1. 自动处理Excel映射到自定义类型的字段属性
2. 扩展为读取丰富表格内容
3. 根据HashMap或者List<T>来生成简单格式的Excel
4. 分离丰富类型的格式模板，依据布局读/生成丰富类型的Excel文件


## 3.设计思路
善用Java三大特性中的多台，善用组合，善用泛型。

## 4.继承关系
基本上累的继承关系不会有太大变动，但是不保证后期结构

![ExcelReadshua](/imgage/posts/打造一个简单的万能ExcelReads工具/关系.png)

## 5.读取例子
```java
 List<Map<String,String>> data=ExcelFactory.getBeans(System.getProperty("user.dir").concat("\\测试.xls"),
                new ResWrapperMap() {
                    @Override//配置Excel属性
                    protected void LoadConfig(Config config) {
                        config.setContent_row_start(3);
                        config.setTitle_row(2);
                    }
                }).//这里能够处理每一行数据
                Process((HashMap<String, String> o) -> System.out.println(o + "\n")
                //这里能够处理时候过滤某一列
                ).FilterCol(() -> new String[]{}
                //这里能根据某一行的某一列的内容来取舍这行数据
        ).Filter((HashMap<String, String> o) -> o.get("创建人") != null && o.get("创建人").length() > 5
                //排序
        ).Sort((o1, o2) -> o1.hashCode()>o2.hashCode()?1:hashCode()==o2.hashCode()?0:-1).Create();

//使用 .CreateMap(key_v) 生成Map<Key,Map>类型数据

```
生成对应自定义类型的写法

```java
Map<String,Seven> map=new ResWrapperObj(Seven) {
                    @Override
                    protected void LoadConfig(Config config) {
                        config.setContent_row_start(3);
                        config.setTitle_row(2);
                    }
                }).
                Process((HashMap<String, String> o) -> {}
                ).FilterCol(() -> new String[]{}
        ).Filter((HashMap<String, String> o) -> o.get("创建人") != null &&
                o.get("创建人").length() > 4).<Map>CreateMap("创建人"));
```
 ## 其他截图
![ExcelReadshua](/imgage/posts/打造一个简单的万能ExcelReads工具/效果.png)
## 实体类截图
![ExcelReadshua](/imgage/posts/打造一个简单的万能ExcelReads工具/实体类.png)


# 欢迎大家集思广益，多提建议，一起完成
