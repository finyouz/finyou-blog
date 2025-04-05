---
title: useState
date: 2025-04-05 23:48:24
tags: [react-hooks]
---
# hooks函数组件

## useState

> 作用：在函数组件中，修改状态值，让函数组件更新
> 
> 语法：const [num,setNum] = useState(10)
> 
>   返回一个state,   以及更新state的函数

```jsx
import { useState } from 'react'
import { Button } from 'antd'
export default function Home() {
  const [num, setNum] = useState(0)
  const handleAdd = () => {
    setNum(num+1)
  }
  return (
    <div>
      <span>{num}</span>
      <Button type="primary" onClick={handleAdd}>增加</Button>
    </div>
  )
}
```

### useState运行机制

> 函数组件每一次渲染【或更新】，都是将函数重新执行，会产生一个全新的“私有上下文”！
> 
> - 内部代码也需要重新执行
>   
> - 涉及的函数需要重新构建（这些函数执行的上下文，每次执行DEMO产生的闭包）
>   
> - 每次执行demo函数，也会把useState重新执行
>   
>   - 执行useState，只有第一次，设置的初始值生效，其他使用修改的值
>     
>   - 返回修改状态的方法，每次都是返回一个新的
>     

![](WechatIMG281.jpg)

   useState ~~ ~~原理

```js
var _state;

function useState(initialValue){
    if(typeof _state === 'unfefined') _state = initialValue

    var setState  = function setState(value){
        _state = value
        //通知视图更新
    }
    return [_state,setState]   
}
```

### useState处理和同步和异步

#### 处理数据

> 当state存取是对象时，当改变值时setState，函数组件和类组件有区别

```jsx
const [state,setState] = useState({
     supNum:10,
     oppNum:5
})


//改变值时
setState({
  ...state,
  supNum:state.supNum + 1
})

//类组件中
this.setState({
    supNum:this.state.supNum + 1
})
```

#### 同步和异步

##### 异步

> 在react18中，通过useState创建的方法，他们执行是异步的
> 
> - 原理：基于异步操作+更新队列
>   

```jsx
import React,{useState} from 'react'
import { flushSync } from 'react-dom'
export default function Demo2() {
  const [x,setX] = useState(0)
  const [y,setY] = useState(0)
  const [z,setZ] = useState(0)
  console.log("reder");
  
  const handleAdd = ()=>{
    setX(x+1)
    setY(y+1)
    setZ(z+1)
  }
  return (
    <div>
        <div>
            <div className="x">{x}</div>
            <div className="y">{y}</div>
            <div className="z">{y}</div>
        </div>
        <button onClick={handleAdd}>增加</button>
    </div>
  )
}
```

##### 同步

> 如何让setState先执行前俩个，让其渲染俩次

```jsx
 flushSync(()=>{
   setX(x+1)
   setY(y+1)
 })
 setZ(z+1)
```

> 在react16中,setState在合成事件，周期函数中是异步在定时器是同步的
