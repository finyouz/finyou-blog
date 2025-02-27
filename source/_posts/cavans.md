---
title: cavans
date: 2025-02-27 17:57:48
tags:
---
# canvas

> ​`<canvas>` 是 `HTML5` 新增的，一个可以使用脚本(通常为 `JavaScript`) 在其中绘制图像的 `HTML` 元素。它可以用来制作照片集或者制作简单(也不是那么简单)的动画，甚至可以进行实时视频处理和渲染。

## 基本使用canvas

### 1.基本使用

> ​支持 `<canvas>` 的浏览器会只渲染 `<canvas>` 标签，而忽略其中的替代内容。不支持 `<canvas>` 的浏览器则 会直接渲染替代内容。

```html
<canvas>你的浏览器不支持 canvas，请升级你的游览器啊</canvas>
```

### 2.渲染上下文

> canvas会创建一个固定大小的画布，然后执行上下文就类似画笔，使用执行上下文来绘制和展示内容

```js

 //找到画布                
var canvas = document.getElementById("canvas");
//找到画布的上下文,获取画笔


if (canvas.getContext){
//浏览器支持
var ctx = canvas.getContext('2d');
       
} 
```

### 3.绘制正方形

```js

 //找到画布                
var canvas = document.getElementById("canvas");
//找到画布的上下文,获取画笔

if (canvas.getContext){
//浏览器支持
var ctx = canvas.getContext('2d');
    // x , y  width height
    ctx.fillRect(40,40,60,60);       
} 
```

## 绘制形状

网页平面直角坐标系

![](https://www.runoob.com/wp-content/uploads/2018/12/Canvas_default_grid.png)

### 1.绘制矩形的方法

- fillRect(x,y,width,height):绘制一个填充的矩形
  
- strokeRect(x,y,width,height):绘制一个矩形边框
  
- clearRect(x,y,width,height):清除指定区域的矩形，然后这块区域变透明
  

## 绘制路径

> 路径绘制图形的一般步骤
> 
> 1.创建路径起始点
> 
> 2.调用绘制方法绘制出路径
> 
> 3.将路径封闭
> 
> 4.路径生成通过描边或者填充实现渲染图片

```js
  var ctx = canvas.getContext('2d');
  //绘制矩形
  ctx.beginPath()
  ctx.moveTo(50, 50); //创建路径起始点
  ctx.lineTo(1000, 50);  //.调用绘制方法绘制出路径
  //闭合路径。会拉一条从当前点到path起始点的直线。如果当前点与起始点重合，则什么都不做
  ctx.closePath();
  ctx.stroke(); //绘制路径。
```

### 绘制三角形

```js
 var canvas = document.getElementById("canvas");
        //找到画布的上下文,获取画笔
        if (canvas.getContext) {
            //浏览器支持
            var ctx = canvas.getContext('2d');
            //绘制矩形
            ctx.beginPath()  //开始绘制路径
            ctx.moveTo(50, 50); //把画笔移动到指定的坐标
            ctx.lineTo(80,50);
            ctx.lineTo(80,90)
            ctx.closePath() //闭合路径
            ctx.fill()
} 
```

### 方法

> 1.beginPath:新建一条路径，路径一旦创建成功，图形绘制命令被指向到路径上生成路径。
> 
> 2.moveTo(x, y):把画笔移动到指定的坐标`(x, y)`。相当于设置路径的起始点坐标。
> 
> 3.closePath():闭合路径之后，图形绘制命令又重新指向到上下文中。
> 
> 4.stroke():通过线条来绘制图形轮廓。
> 
> 5.fill():通过填充路径的内容区域生成实心的图形。

## 绘制圆弧

> 1、arc(x, y, r, startAngle, endAngle, anticlockwise)：以`(x, y)` 为圆心，以`r` 为半径，从 `startAngle` 弧度开始到`endAngle`弧度结束。`anticlosewise` 是布尔值，`true` 表示逆时针，`false` 表示顺时针(默认是顺时针)。
> 
> 2、arcTo(x1, y1, x2, y2, radius): 根据给定的控制点和半径画一段圆弧，最后再以直线连接两个控制点。

### 1. arc

```js
function draw() {
            var canvas = document.getElementById('canvas');
            if (!canvas.getContext) return;
            var ctx = canvas.getContext("2d");
            ctx.beginPath();
            ctx.arc(50, 50, 40, 0, Math.PI / 2, false);
            ctx.stroke();
}
  draw();
```

![效果图](https://www.runoob.com/wp-content/uploads/2018/12/3832141455-5b74dd8f658df_articlex.png)

### 2.arcTo

```js
 function draw() {
            var canvas = document.getElementById('canvas');
            if (!canvas.getContext) return;
            var ctx = canvas.getContext("2d");
            ctx.beginPath();
            ctx.moveTo(50, 50);
            //参数1、2：控制点1坐标   参数3、4：控制点2坐标  参数4：圆弧半径
            ctx.arcTo(200, 50, 200, 200, 100);
            ctx.lineTo(200, 200)
            ctx.stroke();

            ctx.beginPath();
            ctx.rect(50, 50, 10, 10); //起始点
            ctx.rect(200, 50, 10, 10) //控制点1
            ctx.rect(200, 200, 10, 10) //控制点2
            ctx.fill()
}
draw();
```

![效果图](https://www.runoob.com/wp-content/uploads/2018/12/3556678928-5b74dd8f1bd2a_articlex.png)

## 贝塞尔曲线

> 贝塞尔曲线(Bézier curve)，又称贝兹曲线或贝济埃曲线，是应用于二维图形应用程序的数学曲线。

#### 1.一次贝塞尔曲线

![效果图](https://www.runoob.com/wp-content/uploads/2018/12/240px-b_1_big.gif)

#### 2.二次贝塞尔曲线

![效果图](https://www.runoob.com/wp-content/uploads/2018/12/b_2_big.gif)

![效果图](https://www.runoob.com/wp-content/uploads/2018/12/1544764428-5713-240px-BC3A9zier-2-big.svg-.png)

### 3.三次贝塞尔曲线

![效果图](https://www.runoob.com/wp-content/uploads/2018/12/b_3_big.gif)

![效果图](https://www.runoob.com/wp-content/uploads/2018/12/1544764428-2467-240px-BC3A9zier-3-big.svg-.png)

### 4.绘制贝塞尔曲线

> 二次贝塞尔曲线
> 
> quadraticCurveTo(cp1x, cp1y, x, y)
> 
> - 参数1和2：控制点坐标
>   
> - 参数3和4：结束点坐标
>   

```js
function draw(){
    var canvas = document.getElementById('tutorial');
    if (!canvas.getContext) return;
    var ctx = canvas.getContext("2d");
    ctx.beginPath();
    ctx.moveTo(10, 200); //起始点
    var cp1x = 40, cp1y = 100;  //控制点
    var x = 200, y = 200; // 结束点
    //绘制二次贝塞尔曲线
    ctx.quadraticCurveTo(cp1x, cp1y, x, y);
    ctx.stroke();
 
    ctx.beginPath();
    ctx.rect(10, 200, 10, 10);
    ctx.rect(cp1x, cp1y, 10, 10);
    ctx.rect(x, y, 10, 10);
    ctx.fill();
 
}
draw();
```

![效果图](https://www.runoob.com/wp-content/uploads/2018/12/274915666-5b74dd8ecb2e2_articlex.png)

> 三次贝塞尔曲线
> 
> bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)
> 
> - 参数 1 和 2：控制点 1 的坐标
> - ​ 参数 3 和 4：控制点 2 的坐标
> - ​ 参数 5 和 6：结束点的坐标

```js
function draw(){
    var canvas = document.getElementById('tutorial');
    if (!canvas.getContext) return;
    var ctx = canvas.getContext("2d");
    ctx.beginPath();
    ctx.moveTo(40, 200); //起始点
    var cp1x = 20, cp1y = 100;  //控制点1
    var cp2x = 100, cp2y = 120;  //控制点2
    var x = 200, y = 200; // 结束点
    //绘制二次贝塞尔曲线
    ctx.bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y);
    ctx.stroke();
 
    ctx.beginPath();
    ctx.rect(40, 200, 10, 10);
    ctx.rect(cp1x, cp1y, 10, 10);
    ctx.rect(cp2x, cp2y, 10, 10);
    ctx.rect(x, y, 10, 10);
    ctx.fill();
 
}
draw();
```

![效果图](https://www.runoob.com/wp-content/uploads/2018/12/3947786617-5b74dd8ec8678_articlex.png)

## 添加样式颜色

> fillStyle:设置图形的填充颜色
> 
> strokeStyle:设置图形轮廓的颜色

### 1.fillStyle

```js
ctx.fillStyle= "red";
```

### 2.strokeStyle

```js
ctx.strokeStyle = "red";
```

## 透明度

> **globalAlpha = transparencyValue**: 这个属性影响到 canvas 里所有图形的透明度，有效的值范围是 0.0 （完全透明）到 1.0（完全不透明），默认是 1.0

```js
ctx.globalAlpha = 0.1;
```

## line style

### 1.线宽

> lineWidth:线宽。只能是正值。默认是 1.0。

```js
ctx.lineWidth = 10;
```

### 2.线条末端样式

- butt:线段末端以方形结束
  
- round：线段末端以圆形结束
  
- `square`：线段末端以方形结束，但是增加了一个宽度和线段相同，高度是线段厚度一半的矩形区域。
  

```js
var lineCaps = ["butt", "round", "square"];

 ctx.lineCap = lineCaps[i];
```

!["效果图"](https://www.runoob.com/wp-content/uploads/2018/12/3380216230-5b74dd8e97e85_articlex.png)

### 3.线条连接样式

- round 通过填充一个额外的，圆心在相连部分末端的扇形，绘制拐角的形状。 圆角的半径是线段的宽度。
  
- `bevel` 在相连部分的末端填充一个额外的以三角形为底的区域， 每个部分都有各自独立的矩形拐角
  
- `miter`(默认) 通过延伸相连部分的外边缘，使其相交于一点，形成一个额外的菱形区域。
  

```js
 var lineJoin = ['round', 'bevel', 'miter'];
 
 ctx.lineJoin = lineJoin[i];
```

![效果图](https://www.runoob.com/wp-content/uploads/2018/12/1584506777-5b74dd8e82768_articlex.png)

### 4.虚线

> 用 `setLineDash` 方法和 `lineDashOffset` 属性来制定虚线样式。 `setLineDash` 方法接受一个数组，来指定线段与间隙的交替；`lineDashOffset`属性设置起始偏移量。

```js
function draw(){
    var canvas = document.getElementById('tutorial');
    if (!canvas.getContext) return;
    var ctx = canvas.getContext("2d");
 
    ctx.setLineDash([20, 5]);  // [实线长度, 间隙长度]
    ctx.lineDashOffset = -0; //正 逆时针 负 顺时针
    ctx.strokeRect(50, 50, 210, 210);
}
draw();
```

![效果图](https://www.runoob.com/wp-content/uploads/2018/12/2805401035-5b74dd8e6833c_articlex.png)

## 绘制文本

### 填充文本

> fillText(text, x, y [, maxWidth]) 在指定的 (x,y) 位置填充指定的文本，绘制的最大宽度是可选的。

```js
ctx.font = "100px sans-serif"
ctx.fillText("天若有情", 10, 100);
```

### 轮廓文本

> strokeText(text, x, y [, maxWidth]) 在指定的 (x,y) 位置绘制文本边框，绘制的最大宽度是可选的。

```js
 ctx.font = "100px sans-serif"
 ctx.strokeText("天若有情", 10, 200)
```

![效果图](https://www.runoob.com/wp-content/uploads/2018/12/404304980-5b74dd8e7499e_articlex.png)

### 给文本添加样式

> `font = value` 当前我们用来绘制文本的样式。这个字符串使用和 `CSS font` 属性相同的语法。 默认的字体是 `10px sans-serif`。

```js
 ctx.font = "100px sans-serif"
```

> `textAlign = value` 文本对齐选项。 可选的值包括：`start`, `end`, `left`, `right` or `center`。 默认值是 `start`。

```js
 ctx.textAlign = "left"
```

> `direction = value` 文本方向。可选的值包括：`ltr`, `rtl`, `inherit`。默认值是 `inherit`。

```js
 ctx.direction = "ltr"
```

> textBaseline = value 基线对齐选项，可选的值包括：`top`, `hanging`, `middle`, `alphabetic`, `ideographic`, `bottom`。默认值是 `alphabetic。`。

```js
ctx.textBaseline = "top";
```

## 绘制图片

> 我们也可以在 `canvas` 上直接绘制图片。

```js
function draw(){
    var canvas = document.getElementById('tutorial');
    if (!canvas.getContext) return;
    var ctx = canvas.getContext("2d");
    var img = document.querySelector("img");
    ctx.drawImage(img, 0, 0);
}
document.querySelector("img").onclick = function (){
    draw();
}
```

> drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)

### 1.位置

- sx:横坐标
  
- sy:纵坐标
  

```js
 ctx.drawImage(img, 0, 0);
```

### 2.绘制图片大小

- sWidth：宽度
  
- sHeight：高度
  

```js
ctx.drawImage(img, 0, 0, 400, 200)
```

### 3.切片

> 前 4 个是定义图像源的切片位置和大小，后 4 个则是定义切片的目标显示位置和大小。

![切片](https://www.runoob.com/wp-content/uploads/2018/12/2106688680-54566fa3d81dc_articlex.jpeg)

## 状态的保存和恢复

> `save` 和 `restore` 方法是用来保存和恢复 `canvas` 状态的，都没有参数。
> 
> 关于 save() ：Canvas状态存储在栈中，每当save()方法被调用后，当前的状态就被推送到栈中保存,**可以调用任意多次 `save`方法**(类似数组的`push()`)。
> 
> 状态
> 
> 1.当前应用的变形（即移动，旋转和缩放）
> 
> 2.`strokeStyle`, `fillStyle`, `globalAlpha`, `lineWidth`, `lineCap`, `lineJoin`, `miterLimit`, `shadowOffsetX`, `shadowOffsetY`, `shadowBlur`, `shadowColor`, `globalCompositeOperation 的值
> 
> 3.当前的裁切路径（`clipping path`）

> 关于restore()：每一次调用 restore 方法，上一个保存的状态就从栈中弹出，所有设定都恢复(类似数组的 `pop()`)。

## 变形

### 1.translate

> 用来移动 `canvas` 的**原点**到指定的位置
> 
> 注意：`translate` 移动的是 `canvas` 的坐标原点(坐标变换)。

```js
function draw(){
    var canvas = document.getElementById('tutorial');
    if (!canvas.getContext) return;
    var ctx = canvas.getContext("2d");
    ctx.save(); //保存坐原点平移之前的状态
    ctx.translate(100, 100);
    ctx.strokeRect(0, 0, 100, 100)
    ctx.restore(); //恢复到最初状态 原点恢复到0，0
    ctx.translate(220, 220);
    ctx.fillRect(0, 0, 100, 100)
}
draw();
```

### 2.旋转

> 旋转坐标轴,它是顺时针方向的，以弧度为单位的值。

```js
function draw(){
  var canvas = document.getElementById('tutorial');
  if (!canvas.getContext) return;
  var ctx = canvas.getContext("2d");
 
  ctx.fillStyle = "red";
  ctx.save();
 
  ctx.translate(100, 100);
  ctx.rotate(Math.PI / 180 * 45);
  ctx.fillStyle = "blue";
  ctx.fillRect(0, 0, 100, 100);
  ctx.restore();
 
  ctx.save();
  ctx.translate(0, 0);
  ctx.fillRect(0, 0, 50, 50)
  ctx.restore();
}
draw();
```

### 3.scale

> 我们用它来增减图形在 `canvas` 中的像素数目，对形状，位图进行缩小或者放大。scale(x, y)

​ 默认情况下，`canvas` 的 1 单位就是 1 个像素。举例说，如果我们设置缩放因子是 0.5，1 个单位就变成对应 0.5 个像素，这样绘制出来的形状就会是原先的一半。同理，设置为 2.0 时，1 个单位就对应变成了 2 像素，绘制的结果就是图形放大了 2 倍。

### 4. transform (变形矩阵)

> transform(a, b, c, d, e, f)

- a ：水平缩放。
- b ：水平偏斜。
- c ：垂直倾斜。
- d ：垂直缩放。
- e ：水平移动。
- f ：垂直移动。

```js
function draw(){
    var canvas = document.getElementById('tutorial');
    if (!canvas.getContext) return;
    var ctx = canvas.getContext("2d");
    ctx.transform(0.5, 0, 0, .5, 0, 10);
    // ctx.transform(1, 1, 0, 0, 1, 0);
    ctx.fillRect(0, 0, 100, 100);
}
draw();
```

## 合成

> 我们总是将一个图形画在另一个之上，对于其他更多的情况，仅仅这样是远远不够的。比如，对合成的图形来说，绘制顺序会有限制。不过，我们可以利用 globalCompositeOperation 属性来改变这种状况。

```js
globalCompositeOperation = type
```

### source-over

> 这是默认设置，新图像会覆盖在原有图像。

![](https://www.runoob.com/wp-content/uploads/2018/12/1858023544-5b74dd8e0813d.png)

### source-in

> 仅仅会出现新图像与原来图像重叠的部分，其他区域都变成透明的

![](https://www.runoob.com/wp-content/uploads/2018/12/2183873141-5b74dd8e02a4a_articlex.png)

### source-out

> 仅仅显示新图像与老图像没有重叠的部分，其余部分全部透明

![](https://www.runoob.com/wp-content/uploads/2018/12/2402253130-5b74dd8dd7621_articlex.png)

### source-atop

> 新图像仅仅显示与老图像重叠区域。老图像仍然可以显示。

![](https://www.runoob.com/wp-content/uploads/2018/12/1206278247-5b74dd8dd9036_articlex.png)

### destination-over

> 新图像会在老图像的下面

![](https://www.runoob.com/wp-content/uploads/2018/12/2492190378-5b74dd8dca608_articlex.png)

### destination-in

> 仅仅新老图像重叠部分的老图像被显示，其他区域全部透明。

![](https://www.runoob.com/wp-content/uploads/2018/12/284693590-5b74dd8dc7f3e_articlex.png)

### destination-out

> 仅仅老图像与新图像没有重叠的部分。 注意显示的是老图像的部分区域。

![](https://www.runoob.com/wp-content/uploads/2018/12/1921976761-5b74dd8daba2d_articlex.png)

### destination-atop

> 老图像仅仅仅仅显示重叠部分，新图像会显示在老图像的下面

![](https://www.runoob.com/wp-content/uploads/2018/12/4055109887-5b74dd8db283c_articlex.png)

### lighter

> 新老图像都显示，但是重叠区域的颜色做加处理。

![](https://www.runoob.com/wp-content/uploads/2018/12/1200224117-5b74dd8d9453e_articlex.png)

### darken

> 保留重叠部分最黑的像素。(每个颜色位进行比较，得到最小的)
> 
> blue: #0000ff
> red: #ff0000
> 
> 所以重叠部分的颜色：#000000。

![](https://www.runoob.com/wp-content/uploads/2018/12/3835256030-5b74dd8d92ba5_articlex.png)

### lighten

> 保证重叠部分最量的像素。(每个颜色位进行比较，得到最大的)
> 
> blue: #0000ff
> red: #ff0000
> 
> 所以重叠部分的颜色：#ff00ff。

![](https://www.runoob.com/wp-content/uploads/2018/12/1617768463-5b74dd8d99843_articlex.png)

### xor

> 重叠部分会变成透明。

![](https://www.runoob.com/wp-content/uploads/2018/12/2521026104-5b74dd8d6abd6_articlex.png)

### copy

> 只有新图像会被保留，其余的全部被清除(边透明)。

![](https://www.runoob.com/wp-content/uploads/2018/12/2454891415-5b74dd8d67aec_articlex.png)

## 裁剪

> clip()
> 
> 把已经创建的路径转换成裁剪路径。
> 
> ​
> 
> 裁剪路径的作用是遮罩。只显示裁剪路径内的区域，裁剪路径外的区域会被隐藏。
> 
> ​
> 
> 注意：clip() 只能遮罩在这个方法调用之后绘制的图像，如果是 clip() 方法调用之前绘制的图像，则无法实现遮罩。

```js
 function draw(){
    var canvas = document.getElementById('tutorial');
    if (!canvas.getContext) return;
    var ctx = canvas.getContext("2d");
 
    ctx.beginPath();
    ctx.arc(20,20, 100, 0, Math.PI * 2);
    ctx.clip();
 
    ctx.fillStyle = "pink";
    ctx.fillRect(20, 20, 100,100);
}
draw();
```

> 原文作者： 做人要厚道2013
> 
> 原文地址：https://blog.csdn.net/u012468376/article/details/73350998
> 
> 素材：https://www.runoob.com/