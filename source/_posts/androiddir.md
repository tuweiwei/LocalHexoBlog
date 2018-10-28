---
title: 安卓存储卡目录
date: 2018-9-25 22:50:21
categories: "android"
tags:
    - android
    - 移动开发 安卓
description: 安卓SD目录

---

在此输入正文

app独立文件 ： app卸载了  文件希望继续保留  比如图片 
         Environment.getExternalStorageDirectory();
         Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_PICTURES);
app专属文件 ： app卸载，同时与之关联的文件也会卸载 
         存储在internal storage
         getFilesDir
         
         
         存储在external storage
         getExternalFilesDir(null); 这里参数为null 默认会存储在 files下 也可以指定目录
         File externalFilesDir = getExternalFilesDir("Caches");  表示存储在files下的 caches目录
         
         
         
         
有些时候手机没有安装SD卡，所以我们使用前需要判断一下：
if(Environment.getExternalStorageState().equals(Environment.MEDIA_MOUNTED)) {
        //SD卡已装入
    }




