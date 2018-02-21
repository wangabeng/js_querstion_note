# js_querstion_note
js相关问题记录

# 需要反复看的JS书
javascript高级程序设计
javascript算法与结构

计算机专业书
<<算法导论>>

//
js中, apply方法简单理解：
Function.apply(obj,args)方法能接收两个参数

obj：这个对象将代替Function类里this对象。此语句声明后 obj对象就可以调用Function这个方法

args：这个是数组，它将作为参数传给Function（args–>arguments）


JavaScript 中, apply 的第一个参数应该怎么理解？
比如这段：
var values = [1,3,5,7,9,33,57];
var max = Math.max.apply(Math,values);
我自己测试的时候将Math 改为了window this 都能够输出正确答案，好像感觉只要传入一个已被定义的参数都能得到答案，是这样么？

Math.max内部不依赖this，所以你的例子没有差别；下面的代码就不是这样了
http://img.blog.csdn.net/20160312103011365

第一个参数是你调用这个函数的对象，在es5 的严格模式下，调用函数必须指定调用对象。不是严格模式没要求。你传math就相当于math对象调用这个方法，你传window相当于window调用这个方法。其实这里没区别，你直接传null也是可以的。

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply

# 数组的forEach方法遍历 获取index索引值的方法
var arr = ['a', 'b', 'c'];
arr.forEach((item) => {
	console.log(item, arr.indexOf(item));
}) // a 0
      b 1
      c 2
# jQuery方法扩展
1 注册 
	$.fn.extend({
		tab: function () {
			console.log('data');
			// 加上this把自身返回 即把调用此方法的对象返回 this指 $('.test') 返回后即可实现链式调用 
			// return this;  
		}
	});
2 调用 比如一个div.test调用（加上return后 可以实现链式调用）
$('.test').tab().css('background', 'red');
等价于
$('.test').tab();
$('.test').css('background', 'red');

# sublime设置 tab为2个空格
针对已经完成的文件设置方法：
查看-把使用空格缩进 勾选 标签宽度为2勾选
通用的设置方法：
1. 菜单栏里点击 Preferences-> Setting-User
2. 在弹出来的文本里，添加如下两行:
{

    // 注意只有一个大括号，如果之前有属性，如在之前的属性前确保有 ，(逗号)

    //把 tab 转换成4个空格
    "tab_size": 4,

    //把tab 转换成 空格
    "translate_tabs_to_spaces": true
}

# encodeURL及encodeURLComponent的区别：
1 非转移字符：
	字母：52个，十进制数字10个，ASCII标点符号8个 - _ . ! ~ * ' ( )
2 除了㈠㈢的其他字符 包括空格 \ 等等
3 用于分隔URI组件的标点符号共11个，包含10个保留字符 : ; / ? @ & = + $ , 1个井号 #

encodeURL需要转义的包括㈡
encodeURLComponent需要转义的包括 ㈡㈢
实际中encodeURLComponent用得比较多

#fiddler正则匹配应用：
比如 在开发的时候 本地测试url是 localhost:8080 开发时想怎样匹配
http://localhost:8080/product/ 匹配 http://localhost:8080/product/
在fildder中应该这样配置：
REGEX:http://localhost:8080/product/*
http://www.happymmall.com/product/
就匹配到所有 http://www.happymmall.com/product/ 这个路径下的文件了
http://localhost:8080/product/list.do?pageNum=1&pageSize=10&orderBy=default&categoryId=100009
就匹配到下面网址的内容了：
http://www.happymmall.com/product/list.do?pageNum=1&pageSize=10&orderBy=default&categoryId=100008

# 解决html文件更改 不能刷新的问题
参考： http://coding.imooc.com/learn/questiondetail/6266.html
1 webpack.config.json文件的 output对象配置
 publicPath: '/dist' 
2 package.json配置script：
 "server": "webpack-dev-server --inline --port 8080"
3 删掉webpack.config.json的配置
/*  devServer: {
    contentBase: './dist/view',
    historyApiFallback: true,
    inline: true,
    port: 8080,
    noInfo : true
  },*/

# 解决bash命令行ctrl+c不能杀死node进程的问题（2个进程 ctrl+c只能杀死一个进程，导致再次启动dev-server的时候无法开启服务）
解决方法 用系统自带的命令行工具 ctrl+c即可停止node进程

# (?<=exp)也叫零宽度正回顾后发断言 在js中不支持
(?<=exp)也叫零宽度正回顾后发断言，它断言自身出现的位置的前面能匹配表达式exp。比如(?<=\bre)\w+\b会匹配以re开头的单词的后半部分(除了re以外的部分)，例如在查找reading a book时，它匹配ading。

# 一个函数理清call和apply的区别
用法  
oneDefinedFuntionName.apply(this[or some object], arguments[is an array]) // call用法
oneDefinedFuntionName.call(this[or some object], arguments[0], arguments[1]) // apply用法
function test (a, b) {
  return a + b ;
}
function sum (arr) {
  console.log(test.apply(this, arr));
} 
sum([5, 6]); // 11

function test (a, b) {
  return a + b ;
}
function sum (c, d) {
  console.log(test.apply(this, c, d));
} 
sum(5, 6); // 11

# 关于设备像素比
http://yunkus.com/physical-pixel-device-independent-pixels/
设备像素比的实际意义
Dec 17, 2016
设备像素比devicePixelRatio简单介绍一文专业介绍了设备像素比，摘抄精简为以下：
设备像素比（devicePixelRatio）其实指的是window.devicePixelRatio, 被所有WebKit浏览器以及Opera所支持，随着显示器的发展，这个属性也慢慢登上了前端技术的舞台。
window.devicePixelRatio是设备上物理像素和设备独立像素(device-independent pixels (dips))的比例。

 设备像素比 = 物理像素 / 设备独立像素
设备独立像素（这是Android中的叫法，就是web开发中CSS像素）与屏幕密度(硬件)有关。可以用来辅助区分视网膜设备还是非视网膜设备。
所有非视网膜屏幕的iphone在垂直的时候，宽度为320物理像素，当你使用

<meta name="viewport" content="width=device-width">
的时候，会设置视窗布局宽度（不同于视觉区域宽度，不放大显示情况下，两者大小一致）为320px, 于是，页面很自然地覆盖在屏幕上。
而对于视网膜屏幕的iphone，如iphone4s, 纵向显示的时候，屏幕物理像素640像素。同样，当用户设置

<meta name="viewport" content="width=device-width">
的时候，其视区宽度并不是640像素，而是320像素，这是为了有更好的阅读体验 – 更合适的文字大小。

这样，在视网膜屏幕的iphone上，屏幕物理像素640像素，独立像素还是320像素，因此，window.devicePixelRatio等于2

从以上现象得出的结论是：
UI设计师按照手机物理像素出设计稿，切图时根据其设备像素比来换算设备独立像素（CSS像素），比如视网膜手机iPhone6，物理像素750px×1334px，由于其设备像素比为2，CSS切图时需要将设计稿的所有尺寸除以2，才是正确CSS像素值。

https://www.quirksmode.org/blog/archives/2010/04/a_pixel_is_not.html

# 正则中^的含义
http://blog.csdn.net/sufubo/article/details/50990684
正则表达式中的"^"这个符号的一些思考
1 /^A/会匹配"An e"中的A，但是不会匹配"ab A"中的A
2 
[^a]表示“匹配除了a的任意字符”。
[^a-zA-Z0-9]表示“找到一个非字母也非数字的字符”。
[\^abc]表示“找到一个插入符或者a或者b或者c”。
[^\^]表示“找到除了插入符外的任意字符”。（呕！）
只要是”^”这个字符是在中括号”[]”中被使用的话就是表示字符类的否定，如果不是的话就是表示限定开头。我这里说的是直接在”[]”中使用，不包括嵌套使用。 
其实也就是说”[]”代表的是一个字符集，”^”只有在字符集中才是反向字符集的意思。

(^|&)匹配字符串开头或者&字符

[]表示并列关系
var str='abc';
var reg = /[^ac]/; // 匹配非a 且非c的字符
var sum = str.match(reg);
console.log(sum); //["b", index: 1, input: "abc"]

匹配url中的查询参数
var str='http://runjie.benkid.cn/api/find?contentName=service&curPage=1&pageCapacity=8&sort=id#sdk';
var reg = /\?(\w+=\w+($|&)?)+(#|$)?/g;
var sum = str.match(reg);
console.log(sum); // ["?contentName=service&curPage=1&pageCapacity=8&sort=id#"]

#秒懂原型链
http://blog.csdn.net/u012468376/article/details/53121081
http://blog.csdn.net/u012468376/article/details/53127929

# 嵌入百度地图api
http://api.map.baidu.com/lbsapi/creatmap/

# 基本数据类型
Undefined Null Boolean Number String 
复杂数据类型
object

# js内置对象 
(Number String Boolean 基本包装类型)  Array Object Function 
RegExp Global Date Math 

# null和undefined区别
都是基本数据类型 区别是 
null 是一个空的对象指针，是空值 typeof null // object
可以自动转化为0
Number(null); //返回值为 0

undefined 表示“缺少值”，即此处应有一个值，但还没有定义；转为数值时为NaN（非数字值的特殊值）。
Number(undefined); //NaN
5+undefined; //Nan
典型的出现场景如下：
(1)变量被声明了，但没有赋值；
(2) 调用函数时，应提供的参数未提供
(3) 对象没的属性未定义；
(4) 对象没有返回值，则默认返回undefined
var i;
i; //undefined

  function f(x) {
      console.log(X);
  }
  f(); //undefined

  var o = new Object();
  o.name; //undefined

  var x = f();
  x;  //undefined

# js中new的作用
1 创建了一个实例对象
2 这个实例对象的_proto_属性指向这个实例的原型
3 改变原型方法里的this指向 this指向这个实例

# js中给对象添加属性的2种方式
1 直接添加到该对象上
2 给该对象的prototype模式给对象添加属性和方法

# 手写ajax实现
// 1 创建一个XHR对象
var xhr = new XMLHttpRequest();
// 2 打开网页
xhr.open('GET', '/api', false) // false为异步请求 
// 3 定义状态改变的执行函数
xhr.onreadystatechange = function () {
  if (xhr.readyState == 4) {
    if (xhr.status == 200) {
      console.log(xhr.responseText);
    }
  }
}
// 4 发送请求
xhr.send(null);

// 关于readyState
0 未初始化 没有调用send方法
1 已经调用send方法 正在发送请求
2 send方法执行完成 已经收到全部响应内容
3 正在解析响应内容
4 响应内容解析完成 可以在客户端调用

status
2XX 请求成功
3XX 需要重定向 浏览器直接跳转
4xx 客户端请求错误 404 找不到
5XX 服务端错误

# 浏览器理解
浏览器是直接在用户端使用的 实现和用户交互 所以浏览器在前端扮演很重要的角色

#浏览器内核
Trident(IE内核 )
Gecko(Firefo内核) // 会自动更新 一般不会出现兼容性问题
Blink(Google和Opera SOftware内核)
Webkit(Safari内核 Chrome内核原型， 开源)
Presto(Opera前内核 现在已经废弃了)

# css样式权重
!important > 内嵌样式 > ID > class >tagname | 伪类 | 属性选择器 > 伪对象 > 继承 > 通配符
css代码存放位置查看优先级
内嵌样式 > 内部样式表 > 外联样式表

# html5的新特性
a 拖拽释放 (Drag and drop) API
b 语义化更好的内容标签 header nav footer aside article section
c 音频 视频 audio video
d 画布canvas API
e 地理位置 Geolocation API
f 本地离线存储 localStorage
g sessionStorage 关闭浏览器后会自动删除
h 表单控件 calendar data time email url search
i 新的技术 webworker websocket Geolocation

# h5移除了哪些元素
1 纯表现的元素 basefont big center font s strike tt u
2 对可用性产生负面影响的元素 frame frameset noframes

# 如何处理html5的兼容性问题（旧浏览器不兼容）
方法1 针对ie 6 7 8 创建新的标签
document.creatElement('')  // 然后添加并设置样式
方法2 使用成熟的框架 使用最多的是html5shim
<!--[if lt IE 9]>
<script src='......./html5shim.js'></script>
<![endif]-->

#css 如何优化提高性能
压缩
利用继承 合写css
抽离拆分css 不加载所有css 按需加载
图标尽量采用框架的图标
css放在head中 减少请求
不用css表达式
避免使用通配符或隐式通配符
用最简单的方式布局

# css2实现一个元素水平居中和垂直居中对齐
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
      .wrapper {
        width: 500px;
        height: 500px;
        background: gray;
        text-align: center;
      }
      .main {
        width: 200px;
        height: 200px;
        background: red;
        display: inline-block;
        vertical-align: middle;
      }
      /* 
      方法2添加伪类
      .wrapper:after {
        content: '';
        height: 100%;
        display: inline-block;
        vertical-align: middle;
      }

       */
      .vm {
        height: 100%;
        background: blue;
        display: inline-block;
        vertical-align: middle;
        width: 2px;/*test*/
      }
    </style>
  </head>
  <body>
    <div class="wrapper">
      <div class="main"></div><div class="vm"></div><!--方法2 通过css2添加伪类实现 则把.vm元素换成wrapper的after伪类-->
    </div>

  </body>
  </html>

  通过css3实现(可以用flex布局)
  <!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <style>
    .wrapper {
      width: 500px;
      height: 500px;
      background: gray;
      display:-moz-box;
      display:-webkit-box;
      display:box;
      -webkit-box-pack: center;
      -webkit-box-align: center;
    }
    .main {
      width: 200px;
      height: 200px;
      background: red;
      display: inline-block;
    }
  </style>
</head>
<body>
  <div class="wrapper">
    <div class="main"></div>
  </div>

</body>
</html>

#
