# jQuery学习笔记1

## JS与JQ的区别

$JQuery$是一款$JavaScript$库，用于简化$JS$操作。

$JQ1.x$支持$IE\ 678$，每个版本都有压缩版本和未压缩版本，压缩版本不利用阅读，但是没有冗余，执行起来快速，在日常学习时用未压缩版本。

原生$JS$固定写法：

```javascript
<script>
    window.onload = function(ev){
        //1、通过原生JS可以获得DOM元素
        var img = document.getElementsByTagName("img")[0];
        console.log(img);
        //2、通过原生JS可以获得DOM元素的宽高
        var width = window.getComputedStyle(img).width; //获取图像DOM宽度
        console.log(width);
	}
</script>
```

JQ的固定写法：

```javascript
    <script
            src="https://code.jquery.com/jquery-3.5.1.js"
            integrity="sha256-QWo7LDvxbWT2tbbQ97B53yJnYU3WhH/C8ycbRAkjPDc="
            crossorigin="anonymous">
    </script>
<script>
    //1、通过JQ可以获得DOM元素
    $(document).ready(function(){
        var $img = $('img')[0]; //获得img元素
        console.log(img) //输出元素内容

        //2、通过JQ入口函数不可以拿到DOM元素的宽高
        var $width = $img.width();
        console.log($width);

    });
</script>
```

只有按照上述格式写才能获得当前页面的所有元素。

1.  原生$JS$和$JQ$入口函数的加载模式不同，原生$JS$会等到$DOM$元素加载完毕，并且图片也加载完毕才会执行。

2.  $JQ$入口函数会等到$DOM$元素加载完毕，但不会等到图片也加载完毕就会执行。

3.  原生$JS$如果编写多个入口函数，后面编写会覆盖前面编写的

4.  $JQ$中编写多个入口函数，后面的不会覆盖前面的。

## JQ入口函数的其他写法

```javascript
//1、第一种写法
$(document).ready(function(){
    alert("hello Inj");
});

//2、第二种写法
jQuery(document).ready(function(){
    alert("hello Inj");
});

//3、第三种写法(推荐)
$(function(){
    alert("hello Inj");
});

//4、第四种写法
jQuery(function(){
    alert("hello Inj");
});
```

## JQ冲突问题

1.  如\$符号被其他框架使用，此时可以释放\$的使用权

```javascript
jQuery.noConflict(); //释放之后不能使用$，改用上述第四种方法。
```

2.  可以自定义访问符号

```javascript
var nj=jQuery.noConflict();
    nj(function(){
    alert("hello Inj");
});
```

## JQ的核心函数

```javascript
$();//就代表调用JQ的核心函数
```

1.  核心函数可以接受一个函数

```javascript
$(function(){
    alert("hello Inj");
});
```

2.  可以接受一个字符串

1.  可以接受一个字符串选择器

```javascript
$(function(){ //返回一个jQ对象，对象中保存了找到的DOM元素
    var $box1=$(".box1"); //找到对于class=box1的元素
    var $box2=$("#box2"); //找到对于id=box2的元素
});
```

2.  接受一个字符串代码片段

```javascript
$(function(){//返回一个jQ对象，对象中保存了创建的DOM元素
    val $p=$("<p>我是段落</p>"); //根据字符串创建元素
    $box1.append($p); //将元素放入class=box1的区域里
});
```

3.  接受一个$DOM$元素

```javascript
$(function(){//返回一个jQ对象，将DOM元素包装成jQ对象
    var span=document.getElementsByTagName("span")[0]; //通过原生js获取元素
    var $span=$(span); //会将DOM元素包装成一个jq对象返回给$span
});
```


## JQ对象

1.  $JQ$对象是一个伪数组。

## 静态方法和实例方法

```javascript
//1、定义一个类
function AClass(){

}
//2、给这个类添加一个静态方法，直接添加给类的就是静态方法
Aclass.staticMethod = function(){
    alert("staticMethod");
}
//静态方法通过类名调用
Aclass.staticMethod();
//3、给这个类添加一个实例方法(添加给原型)
AClass.prototype.instanceMethod = function(){
    alert("instanceMethod");
}
//4、实例方法通过类的实例调用
//创建一个实例（创建一个对象）
var a= new AClass();
a.instanceMethod(); 
```

### 静态方法each方法

```javascript
var arr=[1,3,5,7,9];
var obj={0:1,1:3,2:5,3:7,4:9, length:5};
//原生js遍历方法
//第一个参数：遍历到的元素
//第二个参数：当前遍历的索引
//注意点：原生js的forEach方法只能遍历数组，不能遍历伪数组
arr.forEach(function(value,index){
    console.log(index,value);
});
//以下语句执行时报错
obj.forEach(function(value,index){
    console.log(index,value);
});

//利用jq的each静态方法遍历数组
//第一个参数：要遍历的数组
//第二个参数：每遍历一个元素之后执行的回调函数
//回调函数的参数：
//	第一个参数：遍历到的索引
//	第二个参数：遍历到的元素
//注意点：jq的each可以遍历伪数组
$.each(arr,function(index,value){
    console.log(index,value);
});
$.each(obj,function(index,value){
    console.log(index,value);
});
```

### 静态方法map方法

```javascript
var arr=[1,3,5,7,9];
var obj={0:1,1:3,2:5,3:7,4:9, length:5};
//1、原生的js map方法遍历
//第一个参数：遍历到的元素
//第二个参数：当前遍历的索引
//第三个参数：当前被遍历的数组
//注意点：和原生js的forEach一样，不能遍历伪数组
arr.map(function(value,index,array){
    console.log(index,value,array);
});

//利用jq的map静态方法遍历数组
//第一个参数：要遍历的数组
//第二个参数：每遍历一个元素之后执行的回调函数
//回调函数的参数：
//	第一个参数：遍历到的元素
//	第二个参数：遍历到的索引
//注意点：和jq的each静态方法一样，可以遍历伪数组
$.map(arr,function(value,index){
    console.log(index,value);
});
$.map(obj,function(value,index){
    console.log(index,value);
});
```

### 静态方法each和map的区别

1.  each静态方法默认的返回值是就是，遍历谁就返回值

2.  map静态方法默认的返回值是一个空数组

3.  each静态方法不支持在回调函数中对遍历数组进行处理，而map方法可以，作为一个新的数组返回：

```javascript
var res = $.map(arr,function(value,index){
    console.log(index,value);
    return value+index;
});
```

### JQ中的其他静态方法

```javascript

var str="    Inj   ";
//静态方法trim
//作用：去除字符串两端的空格
//参数：需要去除空格的字符串
//返回值：去除空格后的字符串
var res = $.trim(str); 

//真数组
var arr=[1,3,5,7,9];
//伪数组
var arrlike={0:1,1:3,2:5,3:7,4:9, length:5};
//对象
var obj = {"name":"Inj","age":"33"};
//函数
var fn = function(){};
//window对象
var w = window;

//静态方法isWindow
//作用：判断传入的对象是否是window对象
//返回值：true/false
var res=$.isWindow(w);//返回true
console.log(res); 

//静态方法isArray
//作用：判断传入的对象是否是真数组
//返回值：true/false
var res=$.isArray(arr);//返回true
console.log(res); 

//静态方法isFunction
//作用：判断传入的对象是否是函数
//返回值：true/false
//jQ框架本质上是一个函数；
(function(window,undefined){

})(window);

var res=$.isFunction(fn);//返回true
console.log(res); 

var res=$.isFunction(jQuery);//返回true
```

### 静态方法holdReady方法

```html
<head>
<script>
    //holdReady作用是暂停ready执行
    //需要执行的时候进行回复
    $.holdReady(true);
    $(document).ready(function(){

    });
</script>
</head>

<button>
    回复ready事件
</button>
<script>
    var btn = document.getElementsByTagName("button")[0];
    bt.onclick = function(){
    //        alert("btn");
    $.holdReady(false);
    }
</script>

```

## 通过WebStorm简化操作

1.  打开$idea$设置

![image-20200524122828868](H:\GitHub\Web\打开idea设置.png)

2.  搜索$live$找到$live\ template$

![image-20200524123033480](H:\GitHub\Web\搜索live.png)

3.  在$HTML/XML$中添加一个模板

![image-20200524123147666](H:\GitHub\Web\选择添加模板.png)

4.  设置快捷命令$jq$，并添加如下代码

```html
<script>
    $(function(){
    // 编写jQ相关代码
    });
</script>
```

![image-20200524123458705](H:\GitHub\Web\设置添加模板.png)

5.  选择$Define$并选择$EveryWhere$

![image-20200524123655722](H:\GitHub\Web\选择Define.png)

​	![image-20200524123805802](H:\GitHub\Web\选择everywhere.png)

6.  保存并测试，在$html$文件里输入$jq$后按$[TAB]$按键，会自动生成模板。

![image-20200524123959470](H:\GitHub\Web\自动生成模板.png)

