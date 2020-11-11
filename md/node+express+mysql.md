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
## 3.建立数据库连接与操作数据库
  + 下载mysql
  ```
  npm install mysql --save
  ```
  + 使用
  ```
  //连接数据库
  var db = mysql.createConnection({ host: 'localhost', port: '3306', user: "root", password: '123456', database: 'test_db' });
db.connect();
// 查询数据
db.query('SELECT * from user', function (error, results, fields) {
    if (error) throw error;
    console.log('results:', results);
    app.get('/', (req, res) => res.send(...results))
});
//插入数据
db.query("insert into user(username,password) values('ww','456789')",function (error,rows) {
    if (error) throw error;
    console.log('插入成功');
    console.log(rows);
})
  ```
