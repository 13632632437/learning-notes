# 基于node和express的后台服务
## 1.搭建express服务
  +  空文件通过 npm init 命令为你的应用创建一个 package.json 文件，管理项目使用到的依赖。
  +  安装 Express 并将其保存到依赖列表中。
```
npm install express --save
```
## 2.启动一个服务
```
const express = require('express');
const app = express();
const port = 3001;
// 可以使用127.0.0.1或localhost
app.listen(port, "10.6.35.14", () => console.log(`服务已经启动在端口：${port}!`));
```
