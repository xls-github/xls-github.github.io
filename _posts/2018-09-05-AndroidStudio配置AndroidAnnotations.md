---
layout: post
title: AndroidStudio配置AndroidAnnotations 
tag: 客户端
---

![这里写图片描述](https://img-blog.csdn.net/20180905101212344?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI3OTI4NTg1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

 1. 加入annotationProcessor 'org.androidannotations:androidannotations:4.4.0'
 2. 加入依赖implementation 'org.androidannotations:androidannotations-api:4.4.0'
 3. 将AndroidManifest里的Activity的名字后面加上\_（下划线），例如：MainActivity改成MainActivity\_