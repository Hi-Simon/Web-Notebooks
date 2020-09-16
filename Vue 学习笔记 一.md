# Vue 学习笔记 一

## MVC和MVVM的区别

1.  $MVC$是后端开发的概念，$MVVM$是前端开发的概念。

2.  $M(model)$即数据，$V(view)$即视图，$C(controller)$即控制器。

2.  $M$是$model$即数据，$VM$是数据和视图之间的调度者，$V$是视图层，$VM$一个重要的功能是实现了双向数据绑定。

## Vue基本指令

```html
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<body>
<div id="app">
    <!--使用v-cloak能够解决 插值表达式闪烁问题 -->
    <p v-cloak>aa{{msg}}aa</p>  <!--显示：aa123aa-->
    <p v-text="msg">aaaaa</p> <!--显示：123-->
    <!--默认v-text 是没有闪烁问题的-->
    <!--v-text会覆盖元素中原本的内容，但是{{}}只会替换自己的占位符，不会把内容清空-->

    <!--能够将数据中的html解析出来,同时会把原本的数据清空-->
    <div v-html="msg2"></div>

    <!--v-bind:用于将数据绑定到元素，可以简写为:-->
    <!--v-bind中，可以写合法的JS表达式-->
    <input type="button" value="按钮" :title="mytitle+'123'">

    <!--Vue中提供了v-on: 事件绑定机制-->
    <!--click,mouseover 等-->
    <!--v-on: 的缩写是 @ -->
    <input type="button" value="按钮" :title="mytitle+'123'" v-on:click="show()">
</div>
<script>
    new Vue({
        el:'#app',
        data:{
            msg:'123',
            msg2:'<h1>hello world</h1>',
            mytitle:'这是一个自己定义的title',
        },
        methods:{
            show(){
                alert("123");
            }
        }
    })
</script>
</body>
```

## Vue实现跑马灯效果

```html
<body>
    <!--    创建一个要控制的区域-->
    <div id="app">{{msg}}
        <input type="button" value="浪起来" v-on:click="lang()">
        <input type="button" value="低调" v-on:click="stop()">
    </div>

<script>
    var vm = new Vue({
        el:'#app',
        data:{
            msg:'猥琐发育，别浪~~',
            intervalId: null,
        },
        /*
            vm实例，会监听到自己身上的data中的数据是否发生变化，只要数据一发生变化，就会自动把最新的 数据，从data上同步到页面上去
            好处：程序员只需要关心数据，不需要考虑如何重新渲染DOM页面
        */
        methods:{
            lang:function()  {
                var msgthis=this;
                //setInterval( () => {这种形式 就可以直接访问this.msg}

                if(this.intervalId!=null) return;
                this.intervalId = setInterval(function () {
                    var tmp=msgthis.msg.substr(0,1);
                    msgthis.msg=msgthis.msg.substr(1,msgthis.msg.length)+tmp;
                },400)
            },
            show:function() {
                alert("123");
            },
            stop:function () {
                //清除定时器
                clearInterval(this.intervalId);
                this.intervalId=null;
            }
        }
    })
</script>
</body>
```

## Vue的时间修饰符

### .stop阻止冒泡

```html
<body>
    <div id="app">
        <div class="inner" v-on:click="divHandler">
<!--            默认情况下点击按钮触发内部点击事件，然后自动触发外部点击事件-->
<!--            使用.stop 阻止冒泡-->
            <input type="button" value="戳他" v-on:click.stop="btnHandler">
        </div>
    </div>
    <script>
        var vm = new Vue({
            el:'#app',
            data:{},
            methods:{
                divHandler:function () {
                    console.log('触发了inner点击事件');
                },
                btnHandler:function () {
                    console.log('触发了内部btn点击事件');
                },
            },

        });
    </script>
</body>
```

### .prevent 阻止默认行为

```html
<body>
    <div id="app">
<!--        可以阻止跳转行为-->
        <a href="http://www.baidu.com" @click.prevent="linkClick">有问题去百度</a>
    </div>
    <script>
        var vm = new Vue({
            el:'#app',
            methods:{
                linkClick:function () {
                    console.log("slkjfalkjf");
                }
            }
        });
    </script>
</body>
```

### .capture捕获触发事件

```html
<body>
<div id="app">
    <div class="inner" v-on:click.capture="divHandler">
        <!--            默认情况下先触发内部点击事件，然后自动触发外部点击事件-->
        <!--            使用.stop 阻止冒泡-->
        <!--        再加上.capture方法，就可以主动捕捉内部按钮的点击事件-->
        <!-- 		并且若不加.stop, 外部事件也能先于 内部事件发生-->
        <input type="button" value="戳他" v-on:click.stop="btnHandler">
    </div>
</div>
<script>
    var vm = new Vue({
        el:'#app',
        data:{},
        methods:{
            divHandler:function () {
                console.log('触发了inner点击事件');
            },
            btnHandler:function () {
                console.log('触发了内部btn点击事件');
            },
        },
    });
</script>
</body>
```

### .self自我触发事件

```html
<body>
<div id="app">
<!--    默认情况下，外部事件是被冒泡执行的，使用.self时只有点击当前元素时才触发-->
    <div class="inner" v-on:click.self="divHandler">
        <input type="button" value="戳他" v-on:click="btnHandler">
    </div>
</div>
<script>
    var vm = new Vue({
        el:'#app',
        data:{},
        methods:{
            divHandler:function () {
                console.log('触发了inner点击事件');
            },
            btnHandler:function () {
                console.log('触发了内部btn点击事件');
            },
        },

    });
</script>
</body>
```

### .once一次触发事件

```html
<body>
    <div id="app">
			<!--        只可以阻止第一次点击时的跳转行为，并且其前后顺序没有关系-->
        <a href="http://www.baidu.com" @click.prevent.once="linkClick">有问题去百度</a>
    </div>
    <script>
        var vm = new Vue({
            el:'#app',
            methods:{
                linkClick:function () {
                    console.log("slkjfalkjf");
                }
            }
        });
    </script>
</body>
```

注意点：$.self$只会阻止自己行为的冒泡，不会影响其他元素。

## 唯一双向数据绑定命令v-model

```html
<body>
    <div id="app">
        <h1 v-text="msg"></h1>
<!--        v-bind 只能实现数据的单向绑定，从M(Model) 自动绑定到 V(Vision)，无法实现数据的双向绑定-->
<!--            使用v-model指令，可以说可以实现 表单元素 和 model 中的数据双向数据绑定-->
<!--                注意： v-model只能使用在表单元素中如： input(radio,text,address,email....) select checkbox textarea -->
        <input type="text" v-model:value="msg" style="width:100vh">
    </div>
    <script>
        var vm = new Vue({
            el:"#app",
            data:{
                msg:"大家都是好学生，爱思考，爱学习！",
            },
            methdos:{
            },
        });
    </script>
</body>
```

## 简易计算器案例

```html
<body>
    <div id="app">
        <input type="text" v-model:value="n1">
        <select name="aa" id="bb" v-model:value="opt">
            <option value="+">+</option>
            <option value="-">-</option>
            <option value="*">*</option>
            <option value="/">/</option>
        </select>
        <input type="text" v-model:value="n2">
        <input type="button" value="=" v-on:click="run()">
        <input type="text" v-model:value="n3">
    </div>
    <script>
        var vm = new Vue({
            el:"#app",
            data:{
                n1:0,
                n2:0,
                n3:0,
                opt:'*',
            },
            methods:{
                run:function () {
                    //方式一
                    // switch (this.opt) {
                    //     case'+':
                    //         this.n3=parseInt(this.n1)+parseInt(this.n2);
                    //         break;
                    //     case'-':
                    //         this.n3=parseInt(this.n1)-parseInt(this.n2);
                    //         break;
                    //     case'*':
                    //         this.n3=parseInt(this.n1)*parseInt(this.n2);
                    //         break;
                    //     case'/':
                    //         this.n3=parseInt(this.n1)/parseInt(this.n2);
                    //         break;
                    // }
                    //方式二，正式开发中，尽量少用
                    //eval的用于是解析字符串并尝试执行
                    var codeStr = 'parseInt(this.n1)'+this.opt+'parseInt(this.n2)';
                    this.n3=eval(codeStr);
                }
            },
        });
    </script>
</body>
```

## class使用样式

```html
<head>
	<style>
        .red{
            color: red;
        }
        .thin{
            font-weight: 200;
        }
        .italic{
            font-style: italic;
        }
        .active{
            letter-spacing:0.5em;
        }
    </style>
</head>
<body>
    <div id="app">
<!--        第一种使用方式，直接传送一个数组，注意：这里的class需要使用v-bind做数据绑定-->
        <h1 v-bind:class="['red','thin']"   >这是一个很大很大的h1，达到你无法想象</h1>

<!--        在数组中使用三元表达式-->
        <h1 v-bind:class="['red','thin', flag?'italic':' ']"   >这是一个很大很大的h1，达到你无法想象</h1>

<!--        通过对象替换三元表达式，提高代码的可读性-->
        <h1 v-bind:class="['red','thin', {'active':flag} ]"   >这是一个很大很大的h1，达到你无法想象</h1>

<!--    在为class 使用v-bind绑定 对象的时候，对象的属性是类名，对象的属性可带引号，也可不带引号，属性的值是一个标志符-->
        <h1 v-bind:class="classObj"   >这是一个很大很大的h1，达到你无法想象</h1>
    </div>
    <script>
        var vm = new Vue({
            el:"#app",
            data:{
                flag:true,
                classObj:{red:true,thin:true,italic:true,active:true},
            },
            methods:{},
        })
    </script>
</body>
```

## 使用内联属性绑定style

```html
<body>
    <div id="app">
        <h1 v-bind:style=[obj,obj2]>this is a h1</h1>
    </div>
    <script>
        var vm = new Vue({
            el:'#app',
            data:{
                obj:{color:'red','font-weight':200},
                obj2:{'font-style':'italic'},
            },
            methods:{}
        })
    </script>
</body>
```

## v-for 循环指令

```
<body>
    <div id="app">
<!--        遍历普通数组-->
        <p v-for="(item,i) in list">{{i}}---{{item}}</p>
<!--        遍历对象数组-->
        <p v-for="item in list2">{{item}}</p>
<!--        遍历对象-->
        <p v-for="item in user">{{item}}</p>
<!--        迭代数字，count从1开始-->
        <p v-for="count in 10">{{count}}</p>
    </div>
    <script>
        var vm = new Vue({
            el:'#app',
            data:{
                list:[1,2,3,4,5,6],
                list2:[
                    {id:1,name:'zs1'},
                    {id:2,name:'zs2'},
                    {id:3,name:'zs3'},
                    {id:4,name:'zs4'},
                ],
                user: {
                    id:1,
                    name:'tony',
                    gender:'male'
                },
            },
            methods:{}
        })
    </script>
</body>
```

### v-for循环中key属性的使用

```html
<body>
    <div id="app">
        <div>
            <label>
                ID:
                <input type="text"v-model="id">
            </label>
            <label for="">
                Name:
                <input type="text" v-model="name">
            </label>
            <input type="button" value="submit" v-on:click="add">
        </div>
<!--        注意： v-for循环的时候，key属性只能用number或string-->
<!--        注意: key在使用的时候，必须使用v-bind属性绑定的形式，指定key的值-->
<!--        组件中使用v-for循环的时候，或者在一些特殊情况中，如果v-for有问题，必须在使用v-for的同时-->
<!--        唯一值等 字符串/数字类型的 key值，作用是将 组件与key进行绑定-->
        <p v-for="item in list" v-bind:key="item.id">
            <input type="checkbox">
            {{item.id}}---{{item.name}}
        </p>
    </div>
    <script>
        var vm = new Vue({
            el:"#app",
            data:{
                list: [
                    {id: 1, name: '里斯'},
                    {id: 2, name: '嬴政'},
                    {id: 3, name: '赵高'},
                    {id: 4, name: '韩非'},
                    {id: 5, name: '荀子'},
                ],
                id: '',
                name: '',
            },
            methods:{
                add:function () {
                    this.list.unshift({id:this.id,name:this.name});
                }
            },

        })
    </script>
</body>
```

## v-if和v-show的区别

```html
<body>
    <div id="app">
        <input type="button" value="click" v-on:click="flag^=1">
<!--        v-if的特点是每次都会删除或创建元素，其有较高的切换性能消耗-->
<!--        如果元素设计频繁的切换最好不要用v-if-->
<!--        如果元素很小的可能别显示出来，可以用v-if-->
        <h3 v-if="flag">this is control by v-if</h3>
<!--        v-show每次不会重新进行dom的删除和创建操作，只是会切换元素的dispaly:none样式-->
<!--        v-show有较高的初始化性能消耗-->
        <h3 v-show="flag">this is control by v-show</h3>
    </div>
    <script>
        var vm = new Vue({
            el:"#app",
            data:{
                flag:true,
            },
            methods:{
                change:function () {
                    this.flag^=1;
                }
            },
        })
    </script>
</body>
```

## Vue中的过滤器

```html
<body>
    <div id="app">
        <p>{{msg | msgFormat('疯狂','123') | msgFormat2}}</p>
    </div>
    <script>
        /*
        过滤器调用时候的格式 {{name | filter_name }}
        定义语法：Vue.filter('filter_name',function(data){});
        过滤器中的function，第一个参数永远都是过滤器管道符 前面传递过来的数组
        过滤器的作用是在不改变元数据的情况下，改变实际输出内容
        过滤器可以不传递参数，也可以传递多个参数，且能够使用多个过滤器
        */
        Vue.filter("msgFormat",function (msg,arg1,arg2) {
            //replace 第一个参数可以输入字符串，或者一个正则表达式
            return msg.replace(/单纯/g,arg1+arg2);
        });
        Vue.filter('msgFormat2',function (msg) {
            return msg+"==";
        })
        var vm =new Vue({
            el: "#app",
            data: {
                msg: '曾经，我也是一个单纯的少年，傻傻的问，谁是世界上最单纯的人'
            },
            methods: {},
        });
        
        var vm2 = new Vue({
            el:"#app2",
            data:{
                dt: new Date(),
            },
            //定义私有过滤器 过滤器有两个条件 【过滤器名称和处理函数】
            //过滤器调用的时候，采用的是就近原则，如果私有过滤器和全局过滤器名称一致，这时候优先调用私有过滤器
            filters: {
                dataFormate: function (msg) {
                    var dt = new Date(msg);
                    var y = dt.getFullYear();
                    //padStart用于字符串填充
                    var n = (dt.getMonth() + 1).toString().padStart(2,'0');
                    var d = dt.getDate().toString().padStart(2,'0');
                    return y + '-' + n + '-' + d + '~~~';
                }
            }
        });
    </script>
</body>
```

## Vue中的按键响应事件

```html
<div>
    <input type="text" class="form-control" v-model="name" v-on:keyup.f2="add">
</div>
<script>
//自定义按键修饰符，通过找到f2的按键码113，将其命名为f2,即可通过按键f2触发事件
Vue.config.keyCodes.f2 = 113
</script>
```

## Vue中自定义全局指令

```html
<div>
    <input type="text" class="form-control" v-model="keywords" v-focus v-color="'blue'">
</div>
<script>
    //使用Vue.directive()定义全局指令 v-focus
    //其中：参数1：指令的名称，注意：定义的时候不需要添加v-前缀
    //但是：调用的时候需要添加前缀v-
    //参数2： 是一个对象，这个对昂身上有一些指令相关的函数，这些函数在特定阶段，执行指定的操作
    Vue.directive('focus',{
        bind:function (el) {
            //每当指令绑定到元素的时候，会执行这个bind函数
            //每个函数的第一个参数表示被绑定的那个元素，是一个原生JS对象
            //第二个参数是一个类，里面有name（指令名称），value（传给指令的表达式的值），expression（传给指令的表达式）等属性。
            //一个元素，只有插入DOM中后，才能获取焦点
        },
        //表示元素插入到DOM中的时候，执行inserted函数
        inserted:function (el) {
            el.focus();
        },
        //表示VNnode更新的时候，会执行updated函数
        updated:function () {

        }
    });
    Vue.directive('color',{
        //样式只要通过指令绑定给了元素，不管这个元素有没有被插入到页面中去，这个元素肯定有了这个内联样式
        //将来元素肯定会显示到页面中
        bind:function (el,binding) {
            //和样式相关的可以定义在bind当中
            el.style.color = 'red';
            el.style.color = binding.value;
        },
        inserted:function (el) {
            //和js行为相关的需要定义在inserted中，防止未加载到DOM中，从而无效
        }
    });
</script>
私有指令定义格式与过滤器类似
```



## 上述综合案例

```html
<html>
<head>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<%--    <script src="https://cdn.bootcdn.net/ajax/libs/bootstrap-vue/2.15.0/bootstrap-vue-icons.common.js"></script>--%>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css" integrity="sha384-9aIt2nRpC12Uk9gS9baDl411NQApFmC26EwAOH8WgZl5MYYxFfc+NcPb1dKGj7Sk" crossorigin="anonymous">
    <title>$Title$</title>
</head>
<body>
<div id="app">

    <div class="panel panel-primary">
        <div class="panel-heading">
            <h3 class="panel-title">添加品牌</h3>
        </div>
        <div class="panel-body form-inline">
            <label>
                ID:
                <input type="text" class="form-control" v-model="id">
            </label>
            <label>
                Name:
                <input type="text" class="form-control" v-model="name" v-on:keyup.f2="add">
            </label>
            <%--            在Vue中，使用事件绑定机制，为元素指定处理函数的时候，如果加了小括号，就可以给函数传参--%>
            <input type="button" value="添加" class="bnt btn-primary" v-on:click="add">
            <label>
                搜索关键字:
                <input type="text" class="form-control" v-model="keywords" v-focus v-color="'blue'">
            </label>
        </div>
    </div>

    <table class="table table-hover table-bordered table-striped">
        <thead>
        <tr>
            <th>Id</th>
            <th>Name</th>
            <th>Ctime</th>
            <th>Opertion</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="item in search(keywords)" v-bind:key="item.id">
            <td v-text="item.id"></td>
            <td v-text="item.name"></td>
            <td>{{item.ctime | dataFormat}}</td>
            <td><a href="" v-on:click.prevent="del(item.id)">删除</a></td>
        </tr>
        </tbody>
    </table>
</div>
<div id = "app2">{{dt | dataFormate}}</div>
<script>
    //使用Vue.directive()定义全局指令 v-focus
    //其中：参数1：指令的名称，注意：定义的时候不需要添加v-前缀
    //但是：调用的时候需要添加前缀v-
    //参数2： 是一个对象，这个对昂身上有一些指令相关的函数，这些函数在特定阶段，执行指定的操作
    Vue.directive('focus',{
        bind:function (el) {
            //每当指令绑定到元素的时候，会执行这个bind函数
            //每个函数的第一个参数都是 el ，表示被绑定的那个元素，el参数是一个原生JS对象
            //一个元素，只有插入DOM中后，才能获取焦点
        },
        //表示元素插入到DOM中的时候，执行inserted函数
        inserted:function (el) {
            el.focus();
        },
        //表示VNnode更新的时候，会执行updated函数
        updated:function () {

        }
    });
    Vue.directive('color',{
        //样式只要通过指令绑定给了元素，不管这个元素有没有被插入到页面中去，这个元素肯定有了这个内联样式
        //将来元素肯定会显示到页面中
        bind:function (el,binding) {
            //和样式相关的可以定义在bind当中
            el.style.color = 'red';
            el.style.color = binding.value;
        },
        inserted:function (el) {
            //和js行为相关的需要定义在inserted中，防止未加载到DOM中，从而无效
        }
    });
    //自定义按键修饰符，通过找到f2的按键码113，将其命名为f2,即可通过按键f2触发事件
    Vue.config.keyCodes.f2 = 113
    Vue.filter('dataFormat',function (msg) {
        var dt = new Date(msg);
        var y = dt.getFullYear();
        var n = dt.getMonth()+1;
        var d = dt.getDate();
        return y+'-'+n+'-'+d;
        <%--return "${y}-${n}-${d}";--%>
    });
    var vm = new Vue({
        el: "#app",
        data: {
            list:[
                {id: 1, name: '奔驰', ctime: new Date() },
                {id: 2, name: '宝马', ctime: new Date() },
            ],
            id: '',
            name: '',
            keywords: '',
        },
        methods:{
            add:function () {
                var tmp={id: this.id, name: this.name, ctime: new Date()};
                this.list.push(tmp);
                this.id=this.name = '';
            },
            del:function (id) {
                // this.list.some((item,i)=>{
                //     if(item.id==id){
                //         // 在数组的some方法中，如果return true，就会立即终止这个数组的后续循环
                //         this.list.splice(i,1); //将从i开始，长度为1的数组内容截掉
                //         return true;
                //     }
                // })
                var index = this.list.findIndex(item=>{
                    if(item.id == id){
                        return true;
                    }
                });
                this.list.splice(index,1);
            },
            search:function (data) {
                var newList=[]
                // this.list.forEach(item =>{
                //     if(item.name.indexOf(data)!=-1){
                //         newList.push(item);
                //     }
                // });
                // return newList;

                // 注意：
                // forEach 循环无法被中断
                // some循环，return true 时结束循环
                // filter 返回满足条件的数组
                // findIndex找到满足条件的数组下标

                return this.list.filter(item=>{
                    //ES6中，为字符串提供了一个新方法，叫做String.prototype.includes('')
                    //如果包含返回true，否则返回false
                    if(item.name.includes(data)){
                        return item;
                    }
                });
            }
        },
    });
    var vm2 = new Vue({
        el:"#app2",
        data:{
            dt: new Date(),
        },
        //定义私有过滤器 过滤器有两个条件 【过滤器名称和处理函数】
        //过滤器调用的时候，采用的是就近原则，如果私有过滤器和全局过滤器名称一致，这时候优先调用私有过滤器
        filters: {
            dataFormate: function (msg) {
                var dt = new Date(msg);
                var y = dt.getFullYear();
                //padStart用于字符串填充
                var n = (dt.getMonth() + 1).toString().padStart(2,'0');
                var d = dt.getDate().toString().padStart(2,'0');
                return y + '-' + n + '-' + d + '~~~';
            }
        }
    });
</script>
</body>
</html>

```

