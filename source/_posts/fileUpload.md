---
title: 文件上传
date: 2025-01-15 15:37:51
tags: [第三方库]
---

# 文件上传

## 1.文件上传

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


## 2.大文件上传



## 3.阿里云上传