# Vue 学习笔记 二

## Vue实例的声明周期

生命周期：从Vue实例创建，运行，到销毁期间，总是伴随着各种各样的事件，这些事件，统称为生命周期。

生命周期钩子=生命周期函数=生命周期事件

-    创建期间的生命周期函数

    ```javascript
        var vm = new Vue({
            el:"#app",
            data:{
                msg:"ok"
            },
            methods:{
                show(){
                    console.log('执行了show函数')
                }
            },
            //第一个生命周期函数，表示实例被完全创建之前，会执行它
            beforeCreate(){
                // console.log(this.msg);
                // this.show();
                //注意：在beforeCreate生命函数执行的的时候，data和methods中的数据都还未被初始化 
            },
            //第二个生命周期函数
            created(){
                console.log(this.msg);
                this.show();
                //在created中，data和methods都已经初始化好了
            },
            //第三个生命周期函数
            //模板已经在内存中编辑完成了，但是尚未把模板渲染到页面中去
            beforeMount(){
                //在beforeMount执行的时候，页面中的元素，还没有被真正的替换，只是页面中的字符串 
            },
            //第四个生命周期函数
            //内存中的模板已经挂载到了页面中去了，用户可以看到渲染好的页面
            mounted(){
                //mounted 是实例创建期间的最后一个生命周期函数，当执行完之后，实例就已经完全创建好了
            }
        });
    
    ```

    

-   运行期间的生命周期函数

    ```javascript
    
    ```

    

-   销毁期间的生命周期函数

## Vue中的Ajax

```html
<head>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<%--    this.$http--%>
    <script src="https://cdn.bootcdn.net/ajax/libs/vue-resource/1.5.1/vue-resource.js"></script>
<%--    <script src="https://cdn.bootcdn.net/ajax/libs/bootstrap-vue/2.15.0/bootstrap-vue-icons.common.js"></script>--%>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css" integrity="sha384-9aIt2nRpC12Uk9gS9baDl411NQApFmC26EwAOH8WgZl5MYYxFfc+NcPb1dKGj7Sk" crossorigin="anonymous">
    <title>$Title$</title>
</head>
<body>
<div id="app">
    <input type="button" value="get请求" v-on:click="getInfo">
    <input type="button" value="post请求" v-on:click="postInfo">
    <input type="button" value="jsonp请求" v-on:click="jsonpInfo">
</div>
<script>
    var vm = new Vue({
        el:"#app",
        data:{

        },
        methods:{
            //发起get请求
            getInfo(){
                //当发起get(url,[option])请求后，通过.then(success,fail)来设置成功和失败的回调函数
                this.$http.get('http://jsonplaceholder.typicode.com/users').then(function (result) {
                    //通过result.body拿到服务器返回成功的数据
                    console.log(result);
                })
            },
            //发起post请求
            postInfo(){
                //手动发起的post(url,[body],[option])请求，默认没有表单格式,所以有些服务器处理不了
                //通过post方法的第三个参数，设置提交类型为 普通表单格式
                this.$http.post('http://jsonplaceholder.typicode.com/users',{},{emulateJSON: true}).then(result=>{
                    console.log(result);
                })
            },
            jsonpInfo(){
                this.$http.jsonp('http://jsonplaceholder.typicode.com/users').then(result=>{
                    console.log(result);
                })
            }
        },
    })
</script>
</body>
```

## JSONP原理

-   由于浏览器安全性限制，不允许Ajax访问协议不同、域名不同、端口号不同的数据接口，浏览器认为这种访问不安全。
-   可以通过动态创建script标签的形式，把script标签的src属性，指向数据接口的地址，因为script标签不存在跨域限制，这种数据获取方式JSONP。（注意：根据JSONP实现原理，JSONP只支持Get请求）

## 定义Vue组件

组件的出现就是为了拆分Vue实例的代码量，能够让我们以u痛的组件，来划分不同的模块，将来我们需要什么样功能，就可以去调用对应的组件即可。

```html
<body>
<div id="app">
    <!--        如果要使用组件，直接把组建的名称以HTML标签的形式引入页面中即可-->
    <my-com1></my-com1>
    <mycom2></mycom2>
    <mycom3></mycom3>
</div>
<%--    3.1组件定义位置，在被控制的 #app 外面，使用template元素，定义组件的模板结构--%>
<template id="tmp1">
    <div>
        <h1>这是使用tempalte元素，在外部定义的组件结构</h1>
    </div>
</template>
<script>
    //1.1 使用Vue.extend来创建全局Vue组件
    var com1 = Vue.extend({
        template:'<h3>这是使用Vue.extend创建的组件</h3>' //通过template属性，制定了组件要展示的HTML结构
    });
    //1.2 使用Vue.component('组件的名称‘，创建出来的组件模板对象)
    // 引用组件时需要在大写字母前加'-'，若名称中无大小写，则直接引用即可

    Vue.component('myCom1',com1);
    //2.1 直接使用Vue.component()
    Vue.component('mycom2',{
        template: '<h3>这是直接使用Vue.component创建的组件</h3>'
    });
    //3.1
    Vue.component('mycom3',{
        template:'#tmp1'
    });
    var vm = new Vue({
        el:"#app",
        data:{

        },
        methods:{

        }
    });

</script>
</body>
```

### 组件化和模块化的区别

模块化：是从代码逻辑的角度进行划分的，方便代码分层开发

组件化：是从UI界面的角度进行划分的，前端的组件化，方便UI组件的重用。

### 组件中的data和methods

```html
<body>
    <div id="app">
        <mycom1></mycom1>
        <mycom1></mycom1>
    </div>
    <template id="tmp1">
        <div>
            <h1>这是一个组件</h1>
            <input type="button" value="+1" @click="increament">
            <h3>{{count}}</h3>
        </div>
    </template>
    <script>
        //1. 组件可以有自己的data数据
        //2. 组件的data和实例的data不一样，组件中的data必须是一个 方法，且必须返回一个对象
        //3. 组件中data的使用和实例中完全一样
        //4. 组件中的data设计成方法的形式，是为了同一组件之间数据的独立性
        Vue.component("mycom1",{
            template:"#tmp1",
            data: function () {
                return {
                    count: 0
                }
            },
            methods: {
                increament(){
                    this.count++;
                }
            }
        });
        var vm = new Vue({
            el: "#app",
            data:{},
            methods:{

            }
        });
    </script>
</body>
```

### 组件切换实例

```html
<body>
    <div id="app">
<%--        方法1--%>
<%--        <a href="" @click.prevent="flag=true">登陆</a>--%>
<%--        <a href="" @click.prevent="flag=false">注册</a>--%>
<%--        <login v-if="flag"></login>--%>
<%--        <register v-else="flag"></register>--%>

<%--        方法2--%>
<%--        Vue提供了component，来展示对应名称的组件--%>
        <a href="" @click.prevent="comName='login'">登陆</a>
        <a href="" @click.prevent="comName='register'">注册</a>
<%--        component是一个占位符，：is属性可以用来指定要展示的组件的名称--%>
        <component :is="comName"></component>
    </div>
    <template id="log">
        <h3>登陆组件</h3>
    </template>
    <template id="reg">
        <h3>注册组件</h3>
    </template>
    <script>
        Vue.component("login",{
            template:"#log"
        });
        Vue.component("register",{
            template:"#reg"
        })
        var vm = new Vue({
            el: "#app",
            data:{
                flag: true,
                comName:'login',
            },
            methods:{

            }

        })
    </script>
</body>
```

### 父子组件直接传递数据

```html
<body>
    <div id="app">
<%-- 父组件可以在引用子组件的时候，通过属性绑定的形式，把需要传递给子组件的数据，以属性绑定的形式传递到子组件内部--%>
        <com1 :parentmsg="msg"></com1>
    </div>
    <script>
        var vm = new Vue({
            el:"#app",
            data:{
                msg:'---父组件中的数据'
            },
            components:{
                //组件中子组件无法直接使用父组件中的数据和方法
                com1:{
                    template:'<h1>这是一个私有子组件{{parentmsg}}</h1>',
                    //父组件的数据传递的数据名称需要现在props里面定义才能使用。
                    //组件中所有props中的数据，都是通过父组件的子组件传递的。
                    props:['parentmsg'],
                    //注意：子组件中data的数据，并不是通过父组件传递过来的，而是子组件私有的，比如通过Ajax传递的
                    //且props的数据只是可读的，data中的数据是可读可写的
                    data:function () {
                        return {}
                    }
                },
            }
        })
    </script>
</body>
```

### 父子组件传递函数

```html
<body>
    <div id="app">
<%--        父组件向子组件传递方法，使用的事件绑定机制,当我们自定义个事件属性之后，子组件就能够调用--%>
        <com1 @func="show"></com1>
    </div>
    <template id="tmp1">
        <div>
            <h1>这是子组件</h1>
            <input type="button" value="子组件中的按钮，点击触发父组件传递过来的方法" @click="myclick">
        </div>
    </template>
    <script>
        var vm = new Vue({
            el:"#app",
            data:{
                dataFromSon:null,
            },
            methods:{
                show(data1){
                    console.log(data1);
                    this.dataFromSon=data1;
                }
            },
            components:{
                com1:{
                    template:"#tmp1",
                    data(){
                        return {
                            sonmsg:{name:'小头儿子',age:6}
                        }
                    },
                    methods: {
                        myclick(){
                            this.$emit('func',this.sonmsg)
                            // console.log("123");
                        }
                    }
                },
            }
        })
    </script>
</body>
```

### 组件实例

```html
<body>
    <div id="app">

        <comment-box @func="load"></comment-box>
        <ul class="list-group">

            <li class="list-group-item" v-for="item in list" :key="item.id">
                {{item.content}}
                <span class="badge" >
                    评论人：{{item.user}}
                </span>
            </li>
        </ul>
    </div>

    <template id="tmp1">
        <div>
            <div class="form-group">
                <label for="">评论人：</label>
                <input type="text" class="form-control" v-model="user">
            </div>
            <div class="form-group">
                <label for="">评论内容：</label>
                <textarea class="form-control" v-model="content"></textarea>
            </div>
            <div class="form-group">
                <input type="button" value="发表评论" class="btn btn-primary" @click="post">
            </div>
        </div>
    </template>
    <script>
        var vm =new Vue({
            el:"#app",
            data:{
                list:[
                    {id:Date.now(),user:'李白',content:'天生我才必有用'},
                    {id:Date.now()+1,user:'小马',content:'我姓马'},
                    {id:Date.now()+2,user:'江小白',content:'劝君更进一杯酒'},
                ]
            },
            methods: {
                load(){
                    var list=JSON.parse(localStorage.getItem('cmts')||'[]');
                    this.list = list;
                }
            },
            created(){
                this.load();
            },
            components:{
                commentBox:{
                    template:"#tmp1",
                    data:function () {
                        return {user:'',content:''}
                    },
                    methods:{
                        post(){
                            var comment = {id:Date.now(),user:this.user,content:this.content};
                            var list=JSON.parse(localStorage.getItem('cmts')||'[]');
                            list.unshift(comment);

                            localStorage.setItem('cmts',JSON.stringify(list));
                            this.user=this.content='';
                            this.$emit('func');
                        }
                    }
                }
            }
        })
    </script>
</body>
```