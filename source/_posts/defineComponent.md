---
title: 微信小程序自定义组件
date: 2024-12-30 14:22:52
tags: [微信小程序自定义组件]
---

# 微信小程序自定义组件

>组件类似于页面，一个自定义组件由 json wxml wxss js 4个文件组成。要编写一个自定义组件，需要在json文件中component字段设为true。

```json
{
  "component": true
}
```

## 简单案例

> 1.components/my-components.json
```json
{
  "component": true,
  "usingComponents": {}
}
```

> 2.components/my-components.wxml

```html
<view class="inner">
  {{innerText}}
</view>
<slot></slot>
```
> 3.components/my-components.wxss

```css
.inner{
    color: red;
}
```

> 4.components/my-components.js

```js
Component({
  properties: {
    // 这里定义了innerText属性，属性值可以在组件使用时指定
    innerText: {
      type: String,
      value: 'default value',
    }
  },
  data: {
    // 这里是一些组件内部数据
    someData: {}
  },
  methods: {
    // 这里是一个自定义方法
    customMethod: function () { }
  }
})
```
> 5.在页面使用（index/index.json）
```json
{
  "usingComponents": {
    "my-component": "/components/my-component"
  }
}
```
```html
<view>
  <component-tag-name inner-text="Some text"></component-tag-name>
</view>
```



>6.注意事项

- 在组件wxss中不应使用ID选择器、属性选择器和标签名选择器。
- 自定义组件的标签名，只能是小写字母、中划线和下划线的组合
- 自定义组件也是可以引用自定义组件的，引用方法类似于页面引用自定义组件的方式（使用 usingComponents 字段）
- 自定义组件和页面所在项目根目录名不能以“wx-”为前缀，否则会报错。

## 组件中的slot

### 1.默认情况下，组件中只能只有一个slot。需要使用多solt时，需要在js组件中声明启用

```js
//components/my-component.js
Component({
  options: {
    multipleSlots: true // 在组件定义时的选项中启用多slot支持
  }
})
```
>组件中存在多个slot，需要name进行区分

组件内
```html
<view class="wrapper">
  <slot name="before"></slot>
  <view>这里是组件的内部细节</view>
  <slot name="after"></slot>
</view>
```

使用组件
```html
<view>
  <my-component>
    <view slot="header">Header Content</view>
    <view slot="footer">Footer Content</view>
  </my-component>
</view>
```

### 2.组样式隔离
默认情况下，自定义组件的样式只受自定义组件wxss。除非一下俩种情况。

- 指定特殊的样式隔离选项 styleIsolation 

- webview 渲染下，在 app.wxss 或页面的 wxss 中使用标签名选择器（或一些其他特殊选择器）来直接指定样式会影响到页面和全部组件。通常情况下这是不推荐的做法。

```js
options: {
    styleIsolation: 'isolated'
}
```
- isolated 表示启用样式隔离，在自定义组件内外，使用 class 指定的样式将不会相互影响（一般情况下的默认值）；
- apply-shared 表示页面 wxss 样式将影响到自定义组件，但自定义组件 wxss 中指定的样式不会影响页面；
- shared 表示页面 wxss 样式将影响到自定义组件，自定义组件 wxss 中指定的样式也会影响页面和其他设置了 apply-shared 或 shared 的自定义组件。

### 3.外部样式
> 有时，组件希望接受外部传入的样式类

```js
/* 组件 custom-component.js */
Component({
  externalClasses: ['my-class']
})
```
```html
<!--使用组件的页面-->
<custom-component class="my-class">这段文本的颜色由组件外的 class 决定</custom-component>
```
```css
/*使用组件的页面的样式 */
.my-class{
    color: red;
}
```
### 4.虚拟化组件节点

>默认情况下，自定义组件本身的那个节点是一个“普通”的节点，使用时可以在这个节点上设置 class style 、动画、 flex 布局等，就如同普通的 view 组件节点一样。

```html
<!-- 页面的 WXML -->
<view style="display: flex">
  <!-- 默认情况下，这是一个普通的节点 -->
  <custom-component style="color: blue; flex: 1">蓝色、满宽的</custom-component>
</view>
```

>但是自定义组件不希望本身可以设置样式，这种情况下，可以将这个自定义组件设置“虚拟的”

```js
Component({
  options: {
    virtualHost: true
  },
  properties: {
    style: { // 定义 style 属性可以拿到 style 属性上设置的值
      type: String,
    }
  },
  externalClasses: ['class'], // 可以将 class 设为 externalClas
})

```
可以将 flex 放入自定义组件内：

```html
<!-- 页面的 WXML -->
<view style="display: flex">
  <!-- 如果设置了 virtualHost ，节点上的样式将失效 -->
  <custom-component style="color: blue">不是蓝色的</custom-component>
</view>
```

组件内
```html
 <view style="flex: 1">
  满宽的
  <slot></slot>
</view>
```

## Component构造器

```js
Component({
  properties: {
    //属性名
    innerText: {
      type: String,
      value: 'default value',
    }
    myProperty2: String // 简化的定义方式
  },
  data: {},
  options: {
    multipleSlots: true, // 在组件定义时的选项中启用多slot支持
     virtualHost: true, // 虚拟组件
     styleIsolation: 'isolated', // 样式隔离
  },
  externalClasses: ['my-class'],// 外部样式
})

```

## 组件通信和事件

>组件间的通信
- 父组件向子组件通信（properties）
- 子组件向父组件传递数据，可以传递任意数据（事件）
- 父组件可以通过this.selectComponent()获取子组件实例，从而调用子组件的方法。


### 监听事件

```html
<!-- 当自定义组件触发“myevent”事件时，调用“onMyEvent”方法 -->
<component-tag-name bindmyevent="onMyEvent" />
<!-- 或者可以写成 -->
<component-tag-name bind:myevent="onMyEvent" />
```

```js
Page({
  onMyEvent: function(e){
    e.detail // 自定义组件触发事件时提供的detail对象
  }
})
```

### 触发事件

```html
<button bindtap="onTap">点击这个按钮将触发“myevent”事件</button>
```
```js
Component({
  properties: {},
  methods: {
    onTap: function(){
      var myEventDetail = {} // detail对象，提供给事件监听函数
      var myEventOption = {} // 触发事件的选项
      this.triggerEvent('myevent', myEventDetail, myEventOption)
    }
  }
})
```

### 自定义的组件实例获取结果
> 若需要自定义 selectComponent 返回的数据，可使用内置 behavior: wx://component-export

```js
// 自定义组件 my-component 内部
Component({
  behaviors: ['wx://component-export'],
  export() {
    return { myField: 'myValue' }
  }
})
```

```html
<!-- 使用自定义组件时 -->
<my-component id="the-id" />
```

```js
<!-- 使用自定义组件时 -->
// 父组件调用
const child = this.selectComponent('#the-id') // 等于 { myField: 'myValue' }
```

## 组件生命周期

>组件的生命周期，指的是组件自身的一些函数，这些函数在特殊的时间点或遇到一些特殊的框架事件时被自动触发。

- create 组件数据 this.data 就是在 Component 构造器中定义的数据 data 。 此时还不能调用 setData
- attached this.data 已被初始化为组件的当前值。

- detached,组件离开页面节点后,该生命周期被触发。退出一个页面时，如果组件还在页面节点树中，则 detached 会被触发。

## 定义生命周期的方法

```js
Component({
  lifetimes: {
    attached: function() {
      // 在组件实例进入页面节点树时执行
    },
    detached: function() {
      // 在组件实例被从页面节点树移除时执行
    },
  },
  // 以下是旧式的定义方式，可以保持对 <2.2.3 版本基础库的兼容
  attached: function() {
    // 在组件实例进入页面节点树时执行
  },
  detached: function() {
    // 在组件实例被从页面节点树移除时执行
  },
  // ...
})

```

|生命周期|参数|描述|
|----|-----|---------|
|created|无|在组件实例刚刚被创建时执行|
|attached|无|在组件实例进入页面节点树时执行|
|ready|无|在组件在视图层布局完成后执行|
|moved|无|在组件实例被移动到节点树另一个位置时执行|
|detached|无|在组件实例被从页面节点树移除时执行|
|error|无|每当组件方法抛出错误时执行|


### 组件所在页面的生命周期
|生命周期|参数|描述|
|----|-----|---------|
|show|无|组件所在的页面被展示时执行|
|hide|无|组件所在的页面被隐藏时执行|
|resize|Object Size|组件所在的页面尺寸变化时执行|
|routeDone|无|组件所在页面路由动画完成时执行|

```js
Component({
  pageLifetimes: {
    show: function() {
      // 页面被展示
    },
    hide: function() {
      // 页面被隐藏
    },
    resize: function(size) {
      // 页面尺寸变化
    }
  }
})
```
> 自定义 tabBar 的 pageLifetime 不会触发。
