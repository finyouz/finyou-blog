---
title: ract-router
date: 2025-04-01 15:26:27
tags: [react]
---
# react-router

> React Router 是一款适用于 React 的多策略路由器，你既可以将它最大限度地作为一个 React 框架来使用，也可以在自己的架构中把它当作一个库来最低限度地使用(在react当初一个路由库进行跳转)。

## 一、当成库使用，在react中使用

```bash
npm i react-router
```

### 1.首次使用

路由配置

```jsx
//router/index.jsx
import Home from '../page/Home.jsx'
import Nav from '../page/Nav.jsx'
import { createBrowserRouter } from 'react-router-dom'

import React from 'react'
const router = createBrowserRouter([
    {
        path:'/',
        element: <Home />,
    },
    {
        path:'/nav',
        element: <Nav />,
    }
])


export default router
```

添加路由

```jsx
//src/App.jsx
import {RouterProvider,Outlet} from 'react-router-dom'
import router from './router/index.jsx'
function App() {
  return (
    <RouterProvider router={router}>
       <Outlet />
    </RouterProvider>
  )
}

export default App
```

### 2.  嵌套路由

   > 路由可以被嵌套在父路由中。子路由通过父路由组件中的 `<Outlet/>` 渲染,可以通过嵌套路由实现二级路由

```jsx
const router = createBrowserRouter([
    {
        path:'/',
        element: <Home />,
        children:[
            {
                path:'/user',
                element:<User/>
            },
            {
                path:'/setting',
                element:<Setting/>
            }
        ]
    },
    {
        path:'/nav',
        element: <Nav />,
    }
])
```

```jsx
import React from 'react'
import { NavLink ,Outlet} from "react-router-dom";
export default function Home() {
  return (
    <div>
      <span>home</span>
      <NavLink to="/user">user</NavLink>
      <br/>
      <NavLink to="/setting">setting</NavLink>

      <Outlet/>
    </div>
  )
}

```

### 3.动态传参

> 如果路由的 path 包含 `:`，那么它会被视为动态参数，当路由被匹配时，动态参数将被解析并提供给其他路由 API 接口，比如 `useParams` 。

```jsx
    path: '/user/:id?', //问号添加到参数后面代表可选
```

### 4.通配符

也被称为 `“捕获所有”` 和 `*` 路由，如果一个路由模式以 `/*` 结尾，后面的任意字符串都会匹配到，也包括其他的 `/`。可以使用其捕获404

```jsx
{
    path: '*',
    element: <div>404</div>
}
```

## 二、导航

### 1.NavLink

> 当 `<NavLink>` 处于激活状态时，它会自动拥有一个 `.active` 类名，以便于使用 CSS 轻松设置样式：

```jsx
 <NavLink to="/user/123">user</NavLink>
 
 <a href="/user/123" class="active">user</a>
```

### 2.Link

> 不需要激活样式时，可以使用 `<Link>` 组件。

```jsx
 <Link to="/user">user</Link>
 
 <a href="user">user</a>
```

### 3.useNavigate

> 这个钩子（hook）允许程序在无需用户交互的情况下将用户导航至新页面。前俩种也叫声明式导航，后者也叫编程式导航

```jsx
import { useNavigate } from "react-router"
function Nav() {
  const navigate = useNavigate()
  return (
    <div onClick={()=>{
      navigate('/user')
    }}>
      nav
    </div>
  )
}
export default Nav
```

## 三、URL传参

> 路由参数从路由中获取

### 1.params

> /user/:id

```jsx
let {id} = useParams()
```

### 2.query

> /user?id= 2&name=finyou

```jsx
let {id,}
```

```jsx
import { useNavigate } from "react-router"
import {useSearchParams} from 'react-router-dom'
function Nav() {
  const navigate = useNavigate()
  const [searchParams,setSearchParams]= useSearchParams();
  
  return (
    <div onClick={()=>{
      navigate('/user')
    }}>
      {searchParams.get('id')}
    </div>
  )
}

export default Nav

```

### 3.Location对象

> React Router 会创建一个自定义 `location` 对象，该对象包含一些有用的信息，可通过 `useLocation` 来获取这些信息。
> 
> ```jsx
> import { useNavigate,useLocation  } from "react-router"
> import {useSearchParams} from 'react-router-dom'
> function Nav() {
>   const navigate = useNavigate()
>   const [searchParams,setSearchParams]= useSearchParams();
>   const location = useLocation()
> 
>   console.log(location);
>   return (
>     <div onClick={()=>{
>       navigate('/user')
>     }}>
>       {searchParams.get('id')}
>     </div>
>   )
> }
> 
> export default Nav
> /**
> {pathname: '/nav', search: '?id=2&name=finyou', hash: '', state: null, key: 'default'}
> hash: ""
> key:"default"
> pathname: "/nav"
> search: "?id=2&name=finyou"
> state: null
> [[Prototype]]: Object
> 
> */
> ```

## 四、路由模式

> 像react和都为单页面应用，在项目开发和构建过程中只存在一个html文件。想vu-router和react-react将项目中的组件和URL路径进行绑定，而当切换URL时，只是组件之间的切换，而不是html文件的重新加载。

router提供俩种模型

- hash模式:在url后面加上#，如http://127.0.0.1:5500/home/#/page1
  

> `hash` 值改变，触发全局 `window` 对象上的 `hashchange` 事件。所以 `hash` 模式路由就是利用 `hashchange` 事件监听 `URL` 的变化，从而进行 `DOM` 操作来模拟页面跳转

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>hash模式</title>
    <style>
        .page{
            height: 200px;
            width: 200px;
            display: none; 
        }
        .index{
            background-color: red; 
            
        }
        .home{
            background-color: blue;
           
        }
    </style>
</head>
<body>
    <a href="#/index">首页</a>
    <a href="#/home">home</a>
    <div class="page index">
        首页
    </div>
    <div class="page home">
        home
    </div>

    <script>
        // 监听hashchange事件
        window.addEventListener('hashchange',function(event){

            
            let newUrl = event.newURL.split('#/')[1];
            let oldUrl = event.oldURL.split('#/')[1];

            const newPage = document.querySelector(`.${newUrl}`);
            const oldPage = document.querySelector(`.${oldUrl}`);
            newPage.style.display = 'block';
            oldPage.style.display = 'none';
            console.log(newUrl,oldUrl);
            
        })
    </script>
</body>
</html>
```

-     history模式:允许操作浏览器的曾经在标签页或者框架里访问的会话历史记录
  

> history模式不存在#,在视觉上更加美观，history是通过history对象中的pushState函数重写了url路径，可在触发重新加载的情况下变更URL路径。(需要服务器支持)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>history模式</title>
    <style>
        .page{
            height: 200px;
            width: 200px;
            display: none; 
        }
        .index{
            background-color: red; 
            
        }
        .home{
            background-color: blue;
           
        }
    </style>
</head>
<body>
    <a onclick="jump('/index')">首页</a>
    <a onclick="jump('/home')">home</a>
    <div class="page index">
        首页
    </div>
    <div class="page home">
        home
    </div>

    <script>
        function jump(path){
            //重写URL路径为超链接传入的名称
            history.pushState(null,'page',path)

            //获取页面组件名称
            const pages = document.querySelectorAll('.page')

            //获取指定跳转的目标对象
            const newPage = document.querySelector(path.replace('/','.'))

            //遍历所有页面组件
            pages.forEach(page=>{
                //隐藏所有页面组件
                page.style.display = 'none'
            })

            //显示指定的页面组件
            newPage.style.display = 'block'
        }
    </script>
</body>
</html>
```