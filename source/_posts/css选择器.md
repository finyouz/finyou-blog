---
title: css选择器
date: 2025-02-05 09:04:48
tags: [css]
---

# CSS选择器

## 1.常用选择器

### 1.1 通用选择器
> 将匹配文档的所有元素

```css
*{
    magin:0;
    padding:0;
}
```

### 1.2 元素选择器
> 按照给定的节点名称，选择所有匹配的元素

```css
div{
    color:red;
}
```

### 1.3 类选择器
> 选择所有拥有给定类名的元素
```css
.box{
    color:red;
}
```

### 1.4 ID选择器
> 按照 id 属性选择一个与之匹配的元素。需要注意的是，一个文档中，每个 ID 属性都应当是唯一的。

```css
#box{
    color:red;
}
```

### 1.5 属性选择器
> 按照给定的属性和属性值，选择所有匹配的元素。

```css
/* 选择所有具有 `type` 属性的元素 */
[type] {
  border: 1px solid red;
}

/* 选择所有 `type` 属性等于 `text` 的元素 */
[type="text"] {
  background-color: yellow;
}

/* 选择所有 src 属性以 .jpg 结尾的图片 */
[src$=".jpg"] {
  border: 2px solid blue;
}

/* 选择所有 `href` 属性以 `https` 开头的链接 */
[href^="https"] {
  text-decoration: none;
}

/* 选择所有 `lang` 属性是 `en` 或者以 `en-` 开头的元素 */
[lang|="en"] {
  color: green;
}

/* 选择属性值中包含 `button` 的元素 */
[class~="button"] {
  font-weight: bold;
}

/* 选择所有 `title` 属性中包含 `tutorial` 的元素 */
[title*="tutorial"] {
  font-style: italic;
}

```

### 1.6 伪类选择器

>CSS伪类是用来添加一些选择器的特殊效果。


>基本语法

```CSS
selector:pseudo-class {property:value;}

selector.class:pseudo-class {property:value;}
```


```css
a:link {color:#FF0000;} /* 未访问的链接 */
a:visited {color:#00FF00;} /* 已访问的链接 */
a:hover {color:#FF00FF;} /* 鼠标划过链接 */
a:active {color:#0000FF;} /* 已选中的链接 */
```

>伪类和css配合使用
```CSS
a.red:visited {color:#FF0000;}
 
<a class="red" href="css-syntax.html">CSS 语法</a>
```

>匹配第一个p元素

```CSS
p:first-child
{
    color:blue;
}
```

[菜鸟教程伪类](https://www.runoob.com/css/css-pseudo-classes.html)

### 1.7 伪元素选择器

>CSS 伪元素是一种特殊的选择器，它可以在不改变 HTML 结构的情况下对页面元素的特定部分进行样式设置。

- 基本语法

```css
selector::pseudo-element {
    property: value;
}

selector.class::pseudo-element {
    property: value;
}
```

- ::after

>在所选元素的内容之后插入内容。

```css
p::after {
  content: " - 这是在段落之后插入的内容";
}
```

- ::before
>在所选元素的内容之前插入内容。
```css
p::before {
  content: "这是在段落之前插入的内容 - ";
}
```



## 2.选择器组合

### 2.1分组选择器
> 选择器分组是指使用逗号分隔的多个选择器，它们具有相同的样式规则。

```css
/**p标签和span标签字体颜色都为红色 */
p,span{
  color:red
}
```

### 2.2组合器

#### 2.2.1 后代选择器
> “ ”（空格）组合器选择前一个元素的后代节点。
```css
/**选择p标签中所有span标签 */
p span{
  color:red
}
```

#### 2.2.2 直接子代选择器
> “>” 组合器选择前一个元素的直接子代的节点。

```css
/**选择div标签中所有span标签 */
div> span{
  color:red
}
```

#### 2.2.3 相邻兄弟选择器
> “+” 组合器选择前一个元素的直接相邻兄弟节点。

```css
div + p{
  color:red
}
```

#### 2.2.4 通用兄弟选择器
> “~” 组合器选择前一个元素的所有兄弟节点。

```css
p ~ span {
  color: red;
}
```
## 3.选择器优先级

第一等级：代表内联样式，如style=""，权值为 1000

第二等级：代表id选择器，如#content，权值为100

第三等级：代表类，伪类和属性选择器，如.content，权值为10

第四等级：代表标签选择器和伪元素选择器，如div p，权值为1

注意：通用选择器（*），子选择器（>），和相邻同胞选择器（+）并不在这个等级中，所以他们的权值为0