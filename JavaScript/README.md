## 什么是JavaScript

-   JavaScript简称js，是前端开发的一门脚本语言（解释性语言）

## JavaScript的组成

1.  ECMAScript：JavaScript的语法标准

    -   ECMA是欧洲计算机制造商协会的简称：European Computer Manufactures Association。

    -   ECMAScript是ECMA制定的脚本语言标准，规定了一种脚本语言实现应该包含的基本内容。

2.  DOM（Document Object model）：JavaScript操作页面上的元素的API。
3.  BOM（Browser Object model）：JavaScript操作浏览器的部分功能的API。

## JavaScript的书写格式

1.  行内样式
2.  内嵌样式，写在head标签中
    -   会导致，DOM元素还未渲染之前就操作DOM元素。
    -   若要使用内嵌样式，需要调用`window.onload = function(){操作界面元素的代码}`。
3.  外链样式。
    -   外链样式也需要调用`window.onload`

## JavaScript的输出方式

1.  通过弹窗的形式输出。
    -   `alert('hello')`  只有按钮
    -   `confirm('hello')` 有两个按钮
    -   `prompt('hello')` 带有输入框
2.  通过网页内容的形式来输出。
3.  通过开发者工具控制台来输出。

