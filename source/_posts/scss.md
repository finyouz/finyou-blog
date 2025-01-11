---
title: sass入门学习
date: 2025-01-11 09:14:36
tags: [scss]
---

# Sass 入门学习

>Sass是一个Css的预处理器，在CSS基础上增加变量、嵌套、导入、混合等高级功能。

## 1 变量声明

>Sass使用$符号声明变量(老版本!),为什么采用该符号?，CSS中并无他用，不会导致与现存或未来的css语法冲突。

### 1-1 变量的使用
```scss
$highlight-color: #F90;


.header {
  color: $highlight-color;
}

//编译后
.header {
  color: #F90;
}
```

## 2 嵌套CSS

>css中重复写选择器是非常恼人的。如果要写一大串指向页面中同一块的样式时，往往需要 一遍又一遍地写同一个ID

```scss
#content {
  article {
    h1 { color: #333 }
    p { margin-bottom: 1.4em }
  }
  aside { background-color: #EEE }
}

//编译之后
#content article h1 { color: #333 }
#content article p { margin-bottom: 1.4em }
#content aside { background-color: #EEE }
```

### 2-1父选择器的标识符&

>使用伪类

```scss
.header a{
  color: #fff;
  &:hover{
    color: #000;
  }
}

//编译后
.header a {
  color: #fff;
}
.header a:hover {
  color: #000;
}
```

### 2-2群组选择器的嵌套
>.button button会命中button元素和类名为.button的元素。这种选择器称为群组选择器。

```scss
.button, button {
  margin: 0;
}

```


```scss
.container {
  h1, h2, h3 {margin-bottom: .8em}
}

//编译之后
.container h1, .container h2, .container h3 { 
    margin-bottom: .8em 
}

```

### 2-3子组合选择器和同层组合选择器:>、+ 和~

1.子组合选择器>、+

子组合选择器>选择一个元素的直接子元素
```scss
article > section {
   border: 1px solid #ccc;
}
```

2.同层组合选择器
```scss
article ~ article { 
    border-top: 1px dashed #ccc;
}
```

### 2-4嵌套属性

>当你需要设置一个边框的样式时，需要设置他的颜色、宽度、样式等，border-*等重复麻烦。

```scss
nav {
  border: {
  style: solid;
  width: 1px;
  color: #ccc;
  }
}

//转换之后
nav {
  border-style: solid;
  border-width: 1px;
  border-color: #ccc;
}
```

### 3.导入Sass文件
> 在css文件中，我们不推荐在css文件中使用@import导入css文件，因为只有执行到@import时，浏览器才会去下载其他css文件，这导致页面加载起来特别慢。


sass也有一个@import规则，但不同的是，sass的@import规则在生成css文件时就把相关文件导入进来。这意味着所有相关的样式被归纳到了同一个css文件中，而无需发起额外的下载请求。

### 3-1 使用sass部分导入文件
