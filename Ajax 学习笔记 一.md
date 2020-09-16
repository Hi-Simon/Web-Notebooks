# Ajax 学习笔记1

## 什么是Ajax

Ajax是与服务器交换数据并更新部分网页的艺术，在不重新加载页面的情况下。

## Ajax的使用

```html
<script>
	window.onload = function(ev){
        var oBtn = document.querySelector("button");
        oBtn.onclick = function(ev1){
            //1、创建异步对象
            var xmlhttp = new XMLHttpRequest();
            //2、设置请求方式和请求地址
            //method：请求的类型：GET，POST
            //url: 文件在服务器上的位置
            //async: true(异步)，false(同步)
            xmlhttp.open("GET","后端文件地址",true);
            //3、发送请求
            xmlhttp.send();
            //4、监听状态变化
            xmlhttp.onreadystatechange = function(ev2){
                //5、处理返回结果
                if(xmlhttp.readyState===4){ //4请求完成
	               
                    //判单是否请求成功
                    if(xmlhttp.status>=200&&xmlhttp.status<300||xmlhttp.statux===304){
                       	console.log("接收到服务器发送的数据");	  
						alert(xmlhttp.responseText);//服务器返回的数据plain
                        alert(xmlhttp.responseXml); //Xml格式
                    }else{
                        console.log("没有接受到数据的数据");
                    }
                }

            }
        }
    }
</script>
```

## Ajax在IE中使用的问题

1.  $XMLHttpRequest$不兼容$IE5,IE6$，解决方法如下

    ```javascript
    if(window.XMLHttpRequest){
        xmlhttp=new XMLHttpRequest();
    }else{
        xhrhttp=new ActiveXObject("Microsoft.XMLHTTP");
    }
    ```

2.  在IE浏览器中，如果通过$Ajax$发送$GET$请求，那么$IE$浏览器认为同一个$URL$只有一个结果，解决如下

    ```javascript
    xmlhttp.open("GET","后端处理文件地址?t="+(new Date().getTime()),true);
    ```

## Ajax封装使用

```javascript
function obj2str(data){
    var res = [];
    /*
    	"userName":"Inj",
    	"userPwd":"123456"
    	"t":1242238429348
    */
    data.t = new Date().getTime();
    for(var key in data){
        //在URL中不能出现中文，只能出现字母、数组、下划线，ASCII码
        //encodeURIComponent()将内容中的中文进行处理
        res.push(encodeURIComponent(key)+"="+encodeURIComponent(data[key])); //[userName=Inj,userPwd=123456];
    }
    return res.join("&"); //用&将数组连接成字符串，userName=Inj&userPwd=123456
}
function ajax(option){
    var data=option.data;
    var type=option.type;
    var url=option.url;
    var timeout=option.timeout;
    //0、将对象转换为字符串
    var str = obj2str(data);
    //1、创建异步对象
    var xmlhttp,timer;
    if(window.XMLHttpRequest){
        xmlhttp=new XMLHttpRequest();
    }else{
        xhrhttp=new ActiveXObject("Microsoft.XMLHTTP");
    }
    //2、设置请求方式和请求地址
    //method：请求的类型：GET，POST
    //url: 后端处理文件在服务器上的位置
    //async: true(异步)，false(同步)
    if(type.toLowerCase==="GET"){
        xmlhttp.open(type,url+"?"+str,true);
        //3、发送请求
        xmlhttp.send();
    }else{
        xmlhttp.open(type,url,true);
        xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded");
        //3、发送请求
        xmlhttp.send(str);
    }

    //4、监听状态变化
    xmlhttp.onreadystatechange = function(ev2){
        //5、处理返回结果
        if(xmlhttp.readyState===4){ //4请求完成
            clearInterval(timer);
            //判单是否请求成功
            if(xmlhttp.status>=200&&xmlhttp.status<300||xmlhttp.statux===304){
                console.log("接收到服务器发送的数据");
                option.success(xmlhttp);
            }else{
                console.log("没有接受到数据的数据");
                option.error(xmlhttp);
            }
        }

    }
    //判断是否传入了超市时间
    if(timeout){
        timer=serInterval(function(){
            console.log("中断请求");
            xmlhttp.abort(); //中断请求
            clearInterval(timer); //清除定时器
        },timeout)
    }

}
```

### 调用测试

```html
<head>
    <script src="myAjax.js"></script> <!--上面函数的引用-->
    <script>
    	window.onload = function(ev){
            var OBtn = document.querySelector("button");
            oBtn.onclick = function(ev1){
                ajax({
                    type:"POST",
                    url:"后端处理文件地址"
                    timeout: 3000,
                    data:{
    					"userName":"Inj",
                    	"userPwd":"123456"
	                },
                    success: function(xhr){
						alert(xhr.responseText);
                    },
                    error: function(xhl){
                        alert(xhl.status);
                    }
                });
            }
        }
    </script>
</head>
```

## JQ中的Ajax

```html
<head>
    <script src="https://code.jquery.com/jquery-3.2.1.min.js"></script> <!--jquery的引用-->
    <script>
    	window.onload = function(ev){
            var OBtn = document.querySelector("button");
            oBtn.onclick = function(ev1){
                $.ajax({
                    type:"POST",
                    url:"后端处理文件地址"
                    data:"userName=Inj&userPwd=124454",
                    sucess: function(msg){
						alert(msg);
                    },
                    error: function(xhl){
                        alert(xhl.status);
                    }
                });
            }
        }
    </script>
</head>
```

## JS中的JSON

```javascript
var res=JSON.parse() //将json字符串转换为 js中json对象
JSON.stringify() //将json对象，封装成json字符串
```

1.  $json$在低版本中不兼容，需要下载$json2.js$来兼容低版本$ie$
2.  $JSON.parse()$只能解析标准的$json$字符串
3.  标准的$json$字符串：`{ "name":"simon", "sex":"male" }`
4.  非标准的$json$字符串：`{ name:simon, sex:male }`

解决方法：

```javascript
var str='{ name:simon, sex:male }';
var obj = eval( "(" +str+ ")" ); //得到的obj就是json对象
//eval方法同时能够解析标准的json的字符串
```

## Cookie



注意点：

1.  $Cookie$默认不会保存任何数据

2.  $Cookie$不能一次保存多条数据，要想保存多条数据，只能一条一条的设置

3.  $Cookie$有大小和个数的限制

    个数限制：$20\sim 50$

    大小限制：$4KB$左右

作用范围：

1.  同一个浏览器的同一个路径下访问

    如果在同一个浏览器中，默认情况下下一级路径可以访问

    如果在同一个浏览器中，想让上一级路径也能访问，那么需要将$Cookie$保存到根路径

    ```javascript
    document.cookie = "name=zs;path=/;";
    ```

2.  默认情况下只能在一个域名下访问，若要想在同一个$domain$下访问需要设置$domain$属性

    ```javascript
    document.cookie = "name=zs;paht=/;domain=xxx.com;";
    ```

3.  默认$Cookie$的过期时间是一次会话，即关闭浏览器后$Cookie$就被清空，因此若要长时间保存需要设置时间

    ```javascript
    var date = new Date();
    date.setDate(date.getDate()+1); //Cookie过期时间为明天
    document.cookie = "key=value;expires="+date.toGMTString()+";path=/;";
    ```

    ### Cookie封装

    #### 添加Cookie

    ```javascript
    window.onload = function(ev){
        function addCookie(key,value,day,path,domain){
            var index = window.location.pathname.lastIndexOf("/");
            var currentPath = window.location.pathname.slice(0,index);
            path = path || currentPath;
            domain = domain || document.domain;
            if(!day){
                document.cookie = key+"="+value+";path="+path+";domain="+domain+";";
            }else{
                var date = new Date();
                date.setDate(date.getDate()+day);
                document.cookie = key+"="+value+";expires="+date.toGMTString()+";path="+path+";domain="+domain+";";
            }
        }
    }
    ```

    #### 获取Cookie

    ```javascript
    window.onload = function(ev){
        function getCookie(key){
            var res = document.cookie.split(";");
            for(var i =0;i<res.length;i++){
                var temp = res[i].split("=");
                if(temp[0].trim()===key){
                    return temp[1];
                }
            }
        }
    }
    ```

    #### 删除Cookie

    ```javascript
    window.onload = function(ev){
        //默认情况下只能删除默认路径下的Cookie，所以需要传入path删除指定路径下的path
    	function delCookie(key,path){
            addCookie(key,getCookie(key),-1,path); //将Cookie过期时间设置为昨天即可
        }
    }
    ```

    #### 统一封装成JQ

    ```javascript
    ;(function($,window){
        $.extend({
            addCookie:function(key,value,day,path,domain){
                var index = window.location.pathname.lastIndexOf("/");
                var currentPath = window.location.pathname.slice(0,index);
                path = path || currentPath;
                domain = domain || document.domain;
                if(!day){
                    document.cookie = key+"="+value+";path="+path+";domain="+domain+";";
                }else{
                    var date = new Date();
                    date.setDate(date.getDate()+day);
                    document.cookie = key+"="+value+";expires="+date.toGMTString()+";path="+path+";domain="+domain+";";
                }
            },
            getCookie:function(key){
                var res = document.cookie.split(";");
                for(var i =0;i<res.length;i++){
                    var temp = res[i].split("=");
                    if(temp[0].trim()===key){
                        return temp[1];
                    }
                }
            },
            delCookie:function(key,path){
                addCookie(key,getCookie(key),-1,path); //将Cookie过期时间设置为昨天即可
            }
        });
    })(jQery,window);
    ```

    ## Web Hash技术

    主要用于记录目前访问的页码，即在网址后面通过$\#page$的方式得到当前页面

    与$Cookie$技术相比，能够将当前的页码保存在$Url$当中，发送$Url$可以同步访问同样的页码

    ```javascript
    window.location.hash=3;//第三页
    console.log(window.location.hash);
    ```

    