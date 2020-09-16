# Vue 学习笔记（四）

## Promise

1.  Promise是用来解决回调地狱问题，并不能帮助减少代码量。
    -   回调地狱是指，函数的回调函数嵌套多层的回调函数。

2.  Promise是一个构造函数。
3.  在Promise上，有两个函数分别叫resolve（成功之后的回调函数）和reject（失败后的回调函数）。
4.  在Promise构造函数的Prototype属性上，有一个`.then()`方法，也就是说，只要是Promise构造函数创建的实例，都可以访问`.then()`方法。
5.  Promise表示一个异步操作，每当`new`一个Promise实例，这个实例就表示一个具体的异步操作。因此只有两种状态：
    1.  状态1：异步执行成功了，需要在内部调用resolve函数把结果返回给调用者。
    2.  状态2：异步执行失败了，需要在内部调用reject函数把结果返回给调用者。
    3.  由于Promise的实例，是一个异步操作，所以内部拿到操作的结果后，无法通过`return`把结构返回调用者；此时只能通过回调函数的形式把结果返回给调用者。
6.  可以在new出来的Promise实例上，调用`.then()`方法，预先为这个Promise异步操作，指定成功(resolve)和失败(reject)的回调函数。

```javascript
/* promise 的基本调用 */
const fs = require('fs');
//每当new 一个Promise 实例的时候，就会立即执行这个异步操作中的代码
var promise = new Promise(function () {
    //函数内部写具体的异步操作
    fs.readFile('./1.txt', 'utf-8', (err, dataStr) => {
        if (err) throw err;
        console.log(dataStr);
    })
});



function getFileByPath(fpath) {
    return new Promise(function (resolve, reject) {
        //函数内部写具体的异步操作
        fs.readFile('./1.txt', 'utf-8', (err, dataStr) => {
            // if (err) throw err;
            // console.log(dataStr);
            if (err) return reject(err);
            resolve(dataStr);
        })
    });
}
getFileByPath('./1.txt').then(function (data) {
    console.log(data);
}, function (err) {
    console.log(err.message);
})
```

```javascript
/* 解决回调地狱问题 */
const fs = require('fs');

function getFileByPath(fpath) {
    return new Promise(function (resolve, reject) {
        //函数内部写具体的异步操作
        fs.readFile(fpath, 'utf-8', (err, dataStr) => {
            // if (err) throw err;
            // console.log(dataStr);
            if (err) return reject(err);
            resolve(dataStr);
        })
    });
}
//如果前面的Promise执行失败，我们不想让后续的Promise被中止，需要为每个Promise指定失败的回调函数
getFileByPath('./11.txt').then(function (data) {
    console.log(data);
    return getFileByPath('./2.txt');
}, function (err) {
    console.log(err);
    return getFileByPath('./2.txt')
}).then(function (data) {
    console.log(data);
    return getFileByPath('./3.txt');
}, function (err) {
    console.log(err);
    return getFileByPath('./3.txt');
}).then(function (data) {
    console.log(data);
}, function (err) {
    console.log(err);
});
//如果前面的Promise执行失败，需要立即终止，可以在最后通过.catch捕获错误。
getFileByPath('./11.txt').then(function (data) {
    console.log(data);
    return getFileByPath('./2.txt');
}).then(function (data) {
    console.log(data);
    return getFileByPath('./3.txt');
}).then(function (data) {
    console.log(data);
}).catch(function(err){
    console.log(err);
});
```

### jQuery中的Promise

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://code.jquery.com/jquery-3.5.1.min.js"
        integrity="sha256-9/aliU8dGd2tb6OSsuzixeV4y/faTqgFtohetphbbj0=" crossorigin="anonymous"></script>
</head>

<body>
    <input type="button" value="获取数据" id="btn">
    <script>
        $(function () {
            $('#btn').on('click', function () {
                $.ajax({
                    url: './data.json',
                    type: 'get',
                    dataType: 'json',
                    // success: function (data) {
                    //     console.log(data);
                    // }
                }).then(function (data) {
                    console.log(data);
                })
            })
        })
    </script>
</body>

</html>
```

### axios封装

```javascript
function axios(){
    return new Promise(function(resolve,reject){
        $.ajax({
            url: './data.json',
            success:function(res){
                resolve(res);
            }
        })
    })
}
axios().then(function(res){
    console.log(res);
})
```

