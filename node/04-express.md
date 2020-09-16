## express基础

### 安装express

```shell
mkdir express-demo
cd express-demo
npm init -y
npm install --save express
```

### 使用express

```javascript
//1. 引入包
var express = require('express');
//2. 创建服务器的应用程序
//   原生：http.creatServer();
var app = express();
// 通过以下方式，公开指定目录后，就可以直接通过网址/public/xx的 方式访问public目录中的所有资源
// 前面是指url，后面是本地目录
app.use('/public/',express.static('./public/'));
//4. 当服务器收到get请求'/'时，执行回调函数处理函数
app.get('/', function (req, res) {
    res.send('hello express');
});
app.get('/about', function (req, res) {
    res.send('about me');
});

//5. 相当于server.listen()
app.listen(3000, function () {
    console.log('app is running at port 3000.');
})
```

