---
title: border-image
date: 2025-03-09 15:06:56
tags: [css]
---

# border-image(边框图片)

> 在css中可以使用border-image属性，使用图片来作为元素的边框，来创建边框图片。border-image是五个border-image-&属性的组合

```css
border-image-source:源文件

border-image-slice：定义边框图像从什么位置开始分割

border-image-width:: 定义边框的厚度

border-image-outset: 定义边框图像的外延尺寸

border-image-repeat：定义边框图像的平铺方式
```

## 1.border-image-source

> 指定一个图片来替换边框的默认样式,当为none时，边框显示默认样式

```css
/*关键值*/

border-image-source:none

/* <image> 值 */
border-image-source: url(image.jpg);
border-image-source: linear-gradient(to top, red, yellow);
```

## 2.border-image-slice

> border-image-slice将引用划分多个区域，这些区域组成了一个元素边框

![分割示例图](border-image-slice.png)

- 区域 1-4 为角区域（corner region）。每一个都被用于组成最终边框图像的四个角。
- 区域 5-8 边区域（edge region）。在最终的边框图像中重复、缩放或修改它们以匹配元素的大小。
- 区域 9 为中心区域（middle region）。它在默认情况下会被丢弃，但如果设置了关键字 `fill`，则会将其用作元素的背景图像。

```css
border-image-slice: 30%;

/* vertical | horizontal */
border-image-slice: 10% 30%;

/* top | horizontal | bottom */
border-image-slice: 30 30% 45;

/* top | right | bottom | left */
border-image-slice: 7 12 14 5;

/* Using the `fill` keyword */
border-image-slice: 10% fill 7 12;
```

[案例](https://codepen.io/siwuxie/pen/NWQgjJz)

## 3.border-image-width.

> 定义边界图片的宽度

```css
/* 关键字 */
border-image-width: auto;

/* 长度 */
border-image-width: 1rem;

/* 百分比 */
border-image-width: 25%;

/* 数值 */
border-image-width: 3;

/* 垂直 | 水平 */
border-image-width: 2em 3em;

/* 上 | 横向 | 下 */
border-image-width: 5% 15% 10%;

/* 上 | 右 | 下 | 左 */
border-image-width: 5% 2em 10% auto;
```

## 4.border-image-outset

> 图像边框相对于边框边界向外偏移的距离

```css
/* <length> 值 */
border-image-outset: 1rem;

/* <number> 值 */
border-image-outset: 1.5;

/* 上、下 | 左、右 */
border-image-outset: 1 1.2;

/* 上 | 左、右 | 下 */
border-image-outset: 30px 2 45px;

/* 上 | 右 | 下 | 左 */
border-image-outset: 7px 12px 14px 5px;
```

## 5.border-image-repeat

> 如何填充使用 border-image-slice 属性分割的图像边框

```css
/* 关键字值 */
border-image-repeat: stretch; /*将被分割的图像使用拉伸的方式来填充满边框区域*/
border-image-repeat: repeat; /*将被分割的图像使用重 
border-image-repeat: round; /*与 repeat 关键字类似，不同之处在于，当背景图像不能以整数次平铺时，会根据情况缩放图像；*/
border-image-repeat: space;/*与 repeat 关键字类似，不同之处在于，当背景图像不能以整数次平铺时，会用空白间隙填充在图像周围。*/

/* 上、下 | 左、右 */
border-image-repeat: round stretch;
```
