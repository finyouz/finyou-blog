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


### useState函数更新和优化机制

#### 函数更新
> React 会对同一个事件循环内的多个 setState（或 setCount）进行批量合并[更新队列]，最终只执行一次更新。因此，即使你循环调用 setCount 100 次，React 只会计算最后一次的结果。
```jsx
const handleAdd = () => {

    for (let i = 0; i < 100; i++) {      
        setCount(count + 1)
    }
  }
//页面更新一次
```

> 第一次更新：由 flushSync 强制同步执行 setCount，count 从 0 → 1。\
第二次更新：React 可能由于某些内部优化（如事件系统或 flushSync 的调度机制）再触发一次更新检查。
```jsx
const handleAdd = () => {

    for (let i = 0; i < 100; i++) {
      flushSync(() => {
        setCount(count + 1)
      })
    }
  }
//页面更新二次
```

> 页面重新渲染一个，count增加0到100
```jsx
const handleAdd = () => {
    for (let i = 0; i < 100; i++) {
      setCount((prvCount)=>prvCount + 1)
    }   
}
//页面更新一次
```

> useState 初始化为函数时,为避免重复创建初始状态\
你传递的是 createInitialTodos 函数本身，而不是 createInitialTodos() 调用该函数的结果。如果将函数传递给 useState，React 仅在初始化期间调用它。

```jsx
 const [todos, setTodos] = useState(createInitialTodos);
```

#### 优化机制

- 每一次修改状态值的时候，会拿最新要修改的值和之前的状态值做比较【基于Object.is做比较】

- 如果值没有发生变化，那么就修改状态，也不会让视图更新。类似于PureComponent,在shouldComponentUpdate做了浅比较
