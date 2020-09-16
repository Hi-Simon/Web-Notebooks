# Vue 学习笔记 三

## Vue中获取DOM元素和组件

```html
<body>
    <div id="app">
        <input type="button" value="获取元素"  @click="getElement" class="btn btn-primary" >
        <h3 id="myh3" ref="myh3">今天天气太好了</h3>
        <login ref="my"></login>
    </div>
    <script>
        var vm = new Vue({
            el:"#app",
            data:{},
            methods:{
                getElement(){
                    // console.log(document.getElementById('myh3').innerText);
                    // ref是英文单词 【reference】
                    console.log(this.$refs.myh3.innerText);
                    console.log(this.$refs.my.msg);
                    this.$refs.my.show();
                }
            },
            components:{
                login:{
                    template:'<h1>登陆组件</h1>',
                    data(){
                        return {
                            msg:'son msg'
                        }
                    },
                    methods:{
                        show(){
                            console.log('调用了子组件的方法');
                        }
                    }
                },
            }
        })
    </script>
</body>
```

## 什么是路由

-   后端路由：对于普通的网站，所有的超链接都是URL地址，所有的URL地址都对应服务器上的资源
-   前端路由：对于单页面应用程序来说，主要通过URL中的hash（#号）来实现不同页面之间的切换，同时hash有一个特点：HTTP请求中不会包含hash相关的内容，所有，单页面程序中的页面跳转主要用hash实现。
-   在单页面应用程序中，这种通过hash改变切换页面的方式，称为前端路由。



### 路由的基本使用

```html
<html>
<head>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<%--    this.$http--%>
    <script src="https://cdn.bootcdn.net/ajax/libs/vue-resource/1.5.1/vue-resource.js"></script>
<%--    <script src="https://cdn.bootcdn.net/ajax/libs/bootstrap-vue/2.15.0/bootstrap-vue-icons.common.js"></script>--%>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css" integrity="sha384-9aIt2nRpC12Uk9gS9baDl411NQApFmC26EwAOH8WgZl5MYYxFfc+NcPb1dKGj7Sk" crossorigin="anonymous">
<%--   1.0导入 vue-router--%>
    <script src="https://cdn.staticfile.org/vue-router/2.7.0/vue-router.min.js"></script>
    <title>$Title$</title>
    <style>
        /*利用router-link进行跳转的组件上都会自动加上一个默认类，通过改变这个类的style可以实现高亮当前选择的组件*/
        .router-link-active{
            color:red;
            font-weight:800;
            font-style:italic;
            font-size:80px;
            text-decoration: underline;
            background-color: pink;
        }
        /*通过配置VueRouter构造参数，改变默认类为自己指定的类名*/
        .myactive{
            color:red;
            font-weight:800;
            font-style:italic;
            font-size:80px;
            text-decoration: underline;
            background-color: pink;
        }
    </style>
</head>
<body>
    <div id="app">
<%--        原生使用方法--%>
        <a href="#/login">登陆</a>
        <a href="#/register">注册</a>
<%--        vue提供的方法--%>
        <router-link to="/login" tag="span">登陆</router-link>
        <router-link to="/register">注册</router-link>

<%--   4.0 vue-router提供的元素，专门用来当作占位符，路由规则匹配到的组件会展示到router-view中去--%>
        <router-view></router-view>
    </div>
    <template id="tmp1">
        <h1>登陆组件</h1>
    </template>
    <template id="tmp2">
        <h1>注册组件</h1>
    </template>
    <script>

        var login={
            template:"#tmp1",
        };
        var register={
            template:"#tmp2",
        };
        //2.0创建一个路由对象，当导入vue-router后，在window全局对象中，就有一个全局构造函数VueRouter
        //在new路由对象的时候，可以为构造函数，传递一个配置对象
        var routerObj = new VueRouter({
            //这个配置对象中的route表示 [路由匹配规则]的意思
            routes:[
                //每个路由规则都是一个对象，这个规则对象，有两个必须的属性：
                //属性1是path，表示监听那个路由链接地址
                //属性2是component，表示如果路由前面是匹配到的path，则展示component属性对应的组件
                //注意：component的属性值，必须是一个组件的模板对象，不能是组件的名称
                {path:'/', redirect: '/login'}, //这里的redirect和Node中的redirect完全不同
                {path:'/login',component:login},
                {path:'/register',component: register}
            ],
            linkActiveClass:'myactive',

        });
        var vm = new Vue({
            el:"#app",
            data:{},
            methods:{},
            //3.0将路由规则对象注册到vm实例上，用来监听URL地址变化，然后展示对应的组件
            router: routerObj,
        })
    </script>
</body>
</html>
```

### 路由参数传递1

```html
<body>
    <div id="app">
<%--        如果在路由中，使用查询字符串，给路由传递参数，不需要修改路由规则的path--%>
        <router-link to="/login?id=10&name=zhangsan">登陆</router-link>
        <router-link to="/register">注册</router-link>
        <router-view></router-view>
    </div>
    <template id="tmp1">
<%--        可以通过组件中的route获取链接中的参数--%>
        <h1>登陆组件---{{$route.query.id}}---{{$route.query.name}}</h1>
    </template>
    <template id="tmp2">
        <h1>注册组件</h1>
    </template>
    <script>
        var login = {
            template:"#tmp1",
            created(){
                console.log(this.$route.query.id);
                console.log(this.$route.query.name);
            }
        };
        var register = {
            template:"#tmp2",
        };
        var routerObj = new VueRouter({
            routes:[
                {path:'/login',component:login},
                {path:'/register',component: register},
            ]
        })
        var vm = new Vue({
            el:"#app",
            data:{},
            methods:{},
            router:routerObj,
        })
    </script>
</body>
```

### 路由参数传递2

```html
<body>
    <div id="app">
<%--        在路由中，给路由传递参数，需要修改路由规则的path--%>
        <router-link to="/login/10/zhangsan">登陆</router-link>
        <router-link to="/register">注册</router-link>
        <router-view></router-view>
    </div>
    <template id="tmp1">
<%--        可以通过组件中的route获取链接中的参数--%>
        <h1>登陆组件---{{$route.params.id}}---{{$route.params.name}}</h1>
    </template>
    <template id="tmp2">
        <h1>注册组件</h1>
    </template>
    <script>
        var login = {
            template:"#tmp1",
            created(){
                console.log(this.$route.params.id);
                console.log(this.$route.params.name);
            }
        };
        var register = {
            template:"#tmp2",
        };
        var routerObj = new VueRouter({
            routes:[
                //通过冒号占位符 表示需要传进一个名字为id的参数
                {path:'/login/:id/:name',component:login},
                {path:'/register',component: register},
            ]
        })
        var vm = new Vue({
            el:"#app",
            data:{},
            methods:{},
            router:routerObj,
        })
    </script>
</body>
```

### 路由嵌套

```html
<body>
    <div id="app">
        <router-link to="/account">Account</router-link>
        <router-view></router-view>
    </div>
    <template id="tmp">
        <div>
            <h1>这是Account组件</h1>
            <router-link to="/account/login">登陆</router-link>
            <router-link to="/account/register">注册</router-link>
            <router-view></router-view>
        </div>
    </template>

    <template id="tmp1">
        <h1>登陆组件</h1>
    </template>

    <template id="tmp2">
        <h1>注册组件</h1>
    </template>

    <script>
        var account = {
            template:"#tmp",
        };
        var login = {
            template:"#tmp1",
        };
        var register = {
            template:"#tmp2",
        };
        var routerObj = new VueRouter({
            routes:[
                {
                    path:'/account',
                    component:account,
                    //使用children属性，实现子路由，同时子路由path前面不要带‘/’,否则永远以根路径开始请求，这样不方便理解url地址
                    children:[
                        {path:'login',component:login},
                        {path:'register',component: register},
                    ]
                },
                // {path:'/account/login',component: login},
                // {path:'/account/register',component:register},
            ]
        });
        var vm = new Vue({
            el:"#app",
            data:{},
            methods:{},
            router:routerObj,
        })
    </script>
</body>
```

## 命名视图实现经典视图（同一页面同时放置多个组件)

```html
<html>
<head>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<%--    this.$http--%>
    <script src="https://cdn.bootcdn.net/ajax/libs/vue-resource/1.5.1/vue-resource.js"></script>
<%--    <script src="https://cdn.bootcdn.net/ajax/libs/bootstrap-vue/2.15.0/bootstrap-vue-icons.common.js"></script>--%>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css" integrity="sha384-9aIt2nRpC12Uk9gS9baDl411NQApFmC26EwAOH8WgZl5MYYxFfc+NcPb1dKGj7Sk" crossorigin="anonymous">
<%--   1.0导入 vue-router--%>
    <script src="https://cdn.staticfile.org/vue-router/2.7.0/vue-router.min.js"></script>
    <title>$Title$</title>
    <style>
        #app{
            margin: 0;
            padding: 0;
        }
        .container{
            margin: 0;
            padding: 0;
            display: flex;
            height: 100%;
        }
        .head{
            background-color: orange;
            height: 80px;
        }
        .left{
            background-color: lightgreen;
            flex:20%;
        }
        .right{
            background-color: lightpink;
            flex:80%;
        }
        h1{
            margin: 0;
            padding: 0;
            font-size: 16px;
        }
        html body{
            margin: 0;
            padding: 0;
        }
    </style>
</head>
<body>
    <div id="app">
        <router-view></router-view>
        <div class="container">
            <router-view name="left"></router-view>
            <router-view name="right"></router-view>
        </div>
    </div>
    <template id="head">
        <h1 class="head">头部区域</h1>
    </template>
    <template id="left">
        <h1 class="left">left区域</h1>
    </template>
    <template id="right">
        <h1 class="right">right区域</h1>
    </template>
    <script>
        var header = {
            template:"#head"
        };
        var leftbox = {
            template:"#left"
        };
        var rightbox = {
            template:"#right"
        };

        var routerObj = new VueRouter({
            routes:[
                {
                    path:'/',
                    components:{
                        'default': header,
                        'left':leftbox,
                        'right':rightbox,
                    }
                },
            ]
        });

        var vm = new Vue({
            el:"#app",
            data:{},
            methods:{},
            router:routerObj,
        })
    </script>
</body>

</html>
```

## 使用keyup监听文本数据变化

```html
<body>
    <div id="app">
        <input type="text" v-model="firstname" @keyup="getFullName">+
        <input type="text" v-model="lastname" @keyup="getFullName">=
        <input type="text" v-model="fullname">
    </div>
    <script>
        var vm = new Vue({
            el:"#app",
            data:{
                firstname:'',
                lastname:'',
                fullname:'',
            },
            methods:{
                getFullName(){
                    this.fullname=this.firstname+'-'+this.lastname;
                }
            },
        })
    </script>
</body>
```

## 使用vue的watch监听数据变化

```html
<body>
    <div id="app">
        <input type="text" v-model="firstname" >+
        <input type="text" v-model="lastname">=
        <input type="text" v-model="fullname">
    </div>
    <script>
        var vm = new Vue({
            el:"#app",
            data:{
                firstname:'',
                lastname:'',
                fullname:'',
            },
            methods:{
            },
            //使用这个属性，可以监视data中指定数据的变化，然后触发watch中对应的函数
            watch:{
                firstname: function (newVal,oldVal) {
                    this.fullname=this.firstname+'-'+this.lastname;
                },
                lastname: function (newVal,oldVal) {
                    this.fullname=this.firstname+'-'+this.lastname;
                }
            }
        })
    </script>
</body>
```

## 使用watch监听路由的改变

```html
<body>
<div id="app">

    <router-link to="/login" tag="span">登陆</router-link>
    <router-link to="/register">注册</router-link>

    <router-view></router-view>
</div>
<template id="tmp1">
    <h1>登陆组件</h1>
</template>
<template id="tmp2">
    <h1>注册组件</h1>
</template>
<script>

    var login={
        template:"#tmp1",
    };
    var register={
        template:"#tmp2",
    };

    var routerObj = new VueRouter({
        routes:[
            {path:'/', redirect: '/login'},
            {path:'/login',component:login},
            {path:'/register',component: register}
        ],
        linkActiveClass:'myactive',

    });
    var vm = new Vue({
        el:"#app",
        data:{},
        methods:{},
        router: routerObj,
        watch:{
            '$route.path':function (newVal,oldVal) {
                if(newVal == '/login'){
                    console.log('欢迎进入登陆页面')
                }else{
                    console.log('欢迎进入注册页面')
                }
            }
        }
    })
</script>
</body>
```

## 使用Vue中的computed监听数据变化

```html
<body>
<div id="app">
    <input type="text" v-model="firstname" >+
    <input type="text" v-model="lastname">=
    <input type="text" v-model="fullname">
</div>
<script>
    var vm = new Vue({
        el:"#app",
        data:{
            firstname:'',
            lastname:'',
            // fullname:'',
        },
        methods:{
        },
        //在computed中，可以定义一些属性，叫做【计算属性】，计算属性的本质就是一个方法，
        //只不过我们在使用这些计算属性的时候，是把它们的名称直接当作属性来使用的，并不会把计算属性当作方法去调用。
        computed:{
            //注意：1、计算属性在引用的时候，一定不要加‘()'去调用，直接当作普通属性去使用
            //     2、只要计算属性这个function中所用到的数据改变，都会自动重新调用这个function
            //     3、计算属性的求值结果会被缓存起来，方便下次使用。如果计算属性方法中所有数据都没有发生变化，不会重新计算求值。
            'fullname':function () {
                return this.firstname+'-'+this.lastname;
            }
        }
    })
</script>
</body>
```