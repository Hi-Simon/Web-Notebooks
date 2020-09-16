## 异步的常见情况

-   定时器
-   ajax
-   事件处理（绑定onclick等）
-   nodejs读取文件

## async和await的作用

```javascript
methods:{
    //原生promise做法
    click1(){  
        axios().then(function(res){
            console.log(res);
        })
    }
    //async修饰的做法
    async cilck2(){
        let res = await axios(); //会等待，拿到返回值后才会继续执行。
        console.log(res);
    }
}
```

-   可以省略.then，简单快捷。
-   原理就是Generator+yield，但是将Generator进行语义化为async和await。

## Generator

```javascript
// Generator方式的函数里面的代码是分段执行的，看到yield就暂停。
function* helloWorldGenerator(){
    yield:'hello';
    yield:'world';
    return ending;
}
var hw = helloWorldGenerator(); //hw目前是一个对象
hw.next();
console.log(hw); //输出{value:‘hello',done:'false'}

hw.next();
console.log(hw); //输出{value:‘world',done:'false'}

hw.next();
console.lgo(hw); //输出{value:‘ending',done:'true'}
```

