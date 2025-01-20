---
title: 文件上传
date: 2025-01-15 15:37:51
tags: [第三方库]
---

# 文件上传

## 1.文件上传(Multer库)

### 1.1 前端
```vue
<template>
  <input type="file" @change="handleFile"/>
  <button @click="handleUpload">
    上传
  </button>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";
const file = ref(null)
const handleUpload = async()=>{
   let result = await axios({
     url:'http://localhost:3000/upload',
     method:'post',
     data:{
       file:file.value
     },
     headers:{
        //后端使用中间件multer
       'Content-Type':'multipart/form-data'
     }
   })
}

const handleFile = (e)=>{
  file.value = e.target.files[0]
}

</script>
```

### 1.2 后端

>后端使用Multer 是一个 node.js 中间件，用于处理 multipart/form-data 类型的表单数据，它主要用于上传文件。

```js
const express = require('express');
const cors = require('cors');
const multer = require('multer');
var fs = require('fs');
const app = express();
const upload = multer({
    dest: 'uploads/',
})
app.use(cors());

app.post('/upload', upload.single('file'), (req, res) => {

    var filePath = './' + req.file.path;


    var temp = req.file.originalname.split('.');
    var fileType = temp[temp.length - 1];
    var lastName = '.' + fileType;
    // 构建图片名
    var fileName = './uploads/'+ Date.now() + lastName;
    
    res.send({
        message: 'File uploaded successfully',
    })
    
    //将二进制文件命名源文件
    fs.rename(filePath, fileName, function (err) {
        
        if (err) {
            console.log('重命名失败'); 
        } 
        

        // 10s后删除源文件
        setTimeout(() => {
            fs.unlink(fileName, function (err) {
                if (err) {
                    console.log('删除失败');
                } else {
                    console.log('删除成功');
                }
            });
        }, 10000);

    })



})

app.listen(3000, () => {
    console.log('服务器启动成功');
})

```


## 2.文件上传(multiparty)

### 2.1前端
```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title></title>

</head>

<body>
    <input type="file">
    <button>上传</button>

</body>
<script>
    const btn = document.querySelector('button');
    const input = document.querySelector('input');
    btn.addEventListener('click', async (e) => {
        let fromData = new FormData();
        fromData.append('file', input.files[0]);
        let res = await fetch('http://localhost:3000/upload', {
            method: 'post',
            body: fromData
        })
        let result = await res.json();
        console.log('result', result);
    })
</script>

</html>
```
### 2.2后端
```js
const express = require('express');
const cors = require('cors');
const multiparty = require('multiparty');
const app = express();
const fs = require('fs');
const port = 3000;
app.use(cors());
app.use('/static', express.static('public'))
app.post("/upload", (req, res) => {
  let from = new multiparty.Form();
  from.uploadDir = './public';


  from.parse(req, (err, fields, files) => {

    let filePath = files.file[0].originalFilename.split(".");
    let fileType = filePath[filePath.length - 1];
    let filename = Date.now() + '.' + fileType;
    let newName = './public/' + filename
    fs.renameSync(files.file[0].path, newName, (err) => {
      if (err) {
        console.log("重命名失败");
      }
    })
    res.send({
      code: 200,
      msg: "上传成功",
      data:`http://localhost:3000/static/${filename}`
    })
  })
})
app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
```

## 3.