---
layout: post
title: 使用阿里矢量图标库在uniapp项目中使用字体图标
date: 2024-12-21 13:45:14
---


## 1.选择自己要使用的图标加入购物车

![选择图标加入购物车](image.png)

## 2.将选择的图标加入项目
![将图标加入项目](image2.png)

## 3.下载到本地
![下载到本地](image3.png)

## 4.将下载文件解压放在static文件中，并调准基本样式
![alt text](image4.png)

## 5.将样式在APP.vue中引入
```vue
<style>
	/*每个页面公共css */
	@import 'static/font/iconfont.css'
</style>
```

## 6.使用
```HTML
<text class="iconfont icon-sexw"></text>
```

## 7.效果展示
![效果展示](image5.png)
