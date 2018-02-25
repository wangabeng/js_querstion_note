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

# 一面 上限
ajax请求原理
跨域请求解决方法

# 关于面试的启发
http://v.youku.com/v_show/id_XMjk3MzgyMTU4MA==.html?spm=a2h0k.8191407.0.0&from=s1.8-1-1.2
1 心态 淡定 可能不是我low 可能太
2 当作一次查漏补缺

# json深拷贝
方法1
function cloneJson (json) {
  return JSON.parse(JSON.stringify(json));
}
方法2
function clone (json) {
  var isArray = json instanceof Array;
  var _json = isArray? []: {};
  for (var k in json) {
    var _json[k] = json[k] isinstanceof Object? clone(json[k]): json[k];
  }
  return _json;
}

# css盒子模型
盒子模型包括border padding width height
比如：
<div>
    sdfsfdsfffffffffffffffffffffffff
</div>
设置width:150px height: 150
在低版本的IE（5 6）中 高度会被文字撑开 实际高度会大于150px(兼容性问题)

# 导致产生跨域问题的三个条件
1 浏览器限制
2 跨域
3 xhr请求

# 剖析Vue原理&实现双向绑定MVVM
https://segmentfault.com/a/1190000006599500

# 正则子项匹配
需求： 把字符串转成驼峰命名 例如 ben-wang-haha 转成benWangHaha
用正则子项
var str = 'ben-wang-haha';
function change (str) {
  var reg = /-(\w)/g;
  return str.replace(reg, function ($0, $1) { // $1表示(\w)
    console.log($0 + 'and' + $1); // -w and w -h and h匹配一次 执行一次
    return $1.toUpperCase();
  })
}
console.log(change(str)); // benWangHaha

# 如何遍历出26个字母
  <script>
  for(var i=0;i<26;i++){
      document.write(String.fromCharCode(65+i));//输出A-Z  26个大写字母
  }
  for(var i=0;i<26;i++){
      document.write(String.fromCharCode(97+i));//输出a-z  26个小写字母
  }
  </script>

# 需求123456789 变成 123,456,789
var str = '123456789';
function change (str) {
  return str.replace(/(?!(?=\b))(?=(\d{3})+$)/g, ',');
}

console.log(change(str));


# 正则里如何使用变量
var str = 'ssgggaaaassssbsssss';
function change (str) {
  var regN = 's';
  var reg = new RegExp(regN, 'g');
  return str.match(reg).length;
}
console.log(change(str)); // benWangHaha

# 正则表达式简明教程 
  https://deerchao.net/tutorials/regex/regex.htm
  http://www.360doc.com/content/16/0422/13/478627_552831815.shtml

  https://www.zybang.com/question/abbd3e6de6eabdf2f33f1d59b38b1ab2.html 正则在线测试 http://tool.chinaz.com/regex/

# 性能优化的几个关键点:
1 多使用内存 缓存
2 减少cup计算 减少网络请求
3 静态资源合并压缩 静态资源缓存
4 使用CDN让资源加载更快
5 使用SSR后端渲染
6 渲染优化 css放前面 JS放后面
7 懒加载（图片）
8 尽量减少DOM操作 对DOM查询做缓存
9 事件节流.

# 原型链问题
  var F = function(){}
  Object.prototype.a = function(){
    console.log('a()')
  }
  Function.prototype.b = function(){
    console.log('b()')
  }
  var f = new F()

  F.a() // ?
  F.b() // ?
  f.a() // ?
  f.b() // ?

https://segmentfault.com/q/1010000002736664/a-1020000002737037

假如我们有以下例子:画的图
见我
var foo = {},
    F = function(){};

Object.prototype.a = 'value a';
Function.prototype.b = 'value b';

console.log(foo.a)    // value a
console.log(foo.b)    // undefined
console.log(F.a)      // value a
console.log(F.b)      // value b

那么

foo.a的查找路径: foo自身: 没有 ---> foo.__proto__(Object.prototype): 找到value a
foo.b的查找路径: foo自身: 没有 ---> foo.__proto__(Object.prototype): 没有 ---> foo.__proto__.__proto__ (Object.prototype.__proto__): 没有
F.a的查找路径: F自身: 没有 ---> F.__proto__(Function.prototype): 没有 ---> F.__proto__.__proto__(Object.prototype): 找到value a
F.b的查找路径: F自身: 没有 ---> F.__proto__(Function.prototype): 找到value b

# 预解析过程中 变量和函数重名了 就只留下函数 注意以下2种情况的区别
alert(getAge) // function getAge() {
                    console.log(50)
                  }
 var getAge = function () {
    console.log(40)
  }

  function getAge() {
    console.log(50)
  }

////
var getAge = function () {
    console.log(40)
  }

  function getAge() {
    console.log(50)
  }
console.log(getAge)  //ƒ () {
                        console.log(40)
                      }  

// 阿里的
  function Person() {
    getAge = function () {
      console.log(10)
    }
    return this
  }

  Person.getAge = function () {
    console.log(20)
  }

  Person.prototype.getAge = function () {
    console.log(30)
  }

  var getAge = function () {
    console.log(40)
  }

  function getAge() {
    console.log(50)
  }

  Person.getAge() // ?20
  getAge() // ?50 X40 v
  Person().getAge() // ?20  x 10 v
  getAge() // ?50 X     10 v
  new Person.getAge() // ?20
  new Person().getAge() // ?30  

# 滴滴难题
  https://www.cnblogs.com/foolgry/p/5309192.html
  function fun(n, o) {
    console.log(o)
    return {
      fun: function (m) {
        return fun(m, n)
      }
    }
  }

  // 测试一: undefined ? ? ?
  var a = fun(0) // undefined
  a.fun(1) // 0
  a.fun(2) // 0
  a.fun(3) // 0

  // 测试二: undefined ? ? ?
  var b = fun(0).fun(1).fun(2).fun(3)

  // 测试三: undefined ? ? ?
  var c = fun(0).fun(1)
  c.fun(2)
  c.fun(3)

# 对象的访问器属性操作 Object.defineProperty()
  var book = {
    _year: 2004,
    edition: 1
  };
  Object.defineProperty(book, 'year', {
    get: function () {
      return this._year;
    },
    set: function (newValue) {
      this.edition = 3;
      this._year = newValue;
    }
  });
  // book.year = 2008;
  console.log(book.year); // 2004 get函数是在读取'year'的时候调用

  //////
  var book = {
    _year: 2004,
    edition: 1
  };
  Object.defineProperty(book, 'year', {
    get: function () {
      return this._year;
    },
    set: function (newValue) {
      this.edition = 3;
      this._year = newValue;
    }
  });
  book.year = 2008; // set函数是在写入'year'的时候调用
  console.log(book._year); // 2008  

# 父元素高度设置为min-height后 子元素的height 100%无效 why?
  父及元素 div 设置 min-height:400px; 
  子元素 div设置 height:100%； 
  奇怪的是子元素的100%并没有把400px继承过来，100%的高度设置失效

  <div style="min-height:400px; background:#000;" class="a1">
    <div style="height:100%; background:#fff;" class="b1"></div>
  </div>

  原因：min-height 是在 height 计算之后再套用的。在计算容器 height 时，默认值为 auto，故由其内容决定。这种情况下子元素百分比的 height 都会当作 auto 处理。例子中算出子元素高度 0，于是容器得到 height 为 0，比 min-height 小，所以最后容器应用 min-height。

  总结：height 100%失效原因 
  1 此元素的父级的父级的父级 没有设置特定的高度 或是即便设置了min-height类似的高度
  2 父级没有加定位 fixed absolute等

# 子元素margin-top属性传递给父元素的问题  
  html结构：
  <div class="box1"><div class="box1_1"></div></div>

  css样式：
  .box1{height:400px;background:#fad;}
  .box1_1{height:100px;margin-top:50px;background:#ade;}
  解决办法：
  1.修改父元素的高度，增添padding-top样式模拟（常用）；// 这样总高度会增加 不可取
  2.为父元素添加overflow:hidden;样式即可（完美）；// 总高度不会增加 可取 很完美 或则用clearfix清除伪类
  5.为父元素或者子元素声明浮动（可用）；
  3.为父元素添加border（可用）;
  4.添加额外的元素放在子元素最前面，设置高度为1px，overflow:hidden(若为行内元素，需要声明为块元素)（啰嗦）;
  6.为父元素或者子元素声明绝对定位（……）。
   
  原理
  一个盒子如果没有上补白(padding-top)和上边框(border-top)，那么这个盒子的上边距会和其内部文档流中的第一个子元素的上边距重叠。
   
  这就是原因了。“嵌套”的盒元素也算“毗邻”，也会 Collapsing Margins。这个合并Margin其实很常见，就是文章段落元素<p/>，并列很多个的时候，每一个都有上下1em的margin，但相邻的<p/>之间只会显示1em的间隔而不是相加的2em。这个很好理解，我就是奇怪为什么W3C要让嵌套的元素也共享Margin，想不出来在什么情况下需要这样的表现。 　　这个问题的避免方法很多，只要破坏它出现的条件就行。给Outer Div 加上 padding/border，或者给 Outer Div / Inner Div 设置为 float/position:absolute(CSS2.1规定浮动元素和绝对定位元素不参与Margin折叠)。
   
  在 IE/Win 中如果这个盒子有 layout 那么这种现象就不会发生了：似乎拥有 layout 会阻止其孩子的边距伸出包含容器之外。此外当 hasLayout = true 时，不论包含容器还是孩子元素，都会有边距计算错误的问题出现。

# stickerfoot布局 出自慕课的饿了吗App项目：
  代码如下
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <style>
/*  http://blog.csdn.net/FE_dev/article/details/68954481
*/
    .detail {  /* 和手机屏幕一样大 */
      width: 100%;
      height:100%;
      position: fixed;
      left: 0;
      top: 0;
      z-index: 0;
    }
    .detail-wrapper {
      width: 100%;
      min-height:100%;
      /*overflow: hidden; 用了overflow hidden清楚浮动 就不需要加clearfix清除浮动了*/
    }
    .detail-main {
      width: 100%;
      min-height:100%;
      background: gray;
      margin-top: 64px;
      padding-bottom: 64px;  
    } 
    .detail-close {
      /*position: relative;*/
      width: 32px;
      height:32px;
      background: blue;
      margin: -64px auto 0 auto;
      clear: both;
    }
    .clearfix { /*如果用clearfix必须设置为inline-block 否则总高度还是会增加*/
      display: inline-block;
    }
    .clearfix:after {
       content:""; 
       display: block; 
       clear:both; 
    }
  </style>
</head>
<body>
  <div class="detail">
    <div class="detail-wrapper clearfix">
      <div class="detail-main">
          主内容区<br>主内容区<br>
    </div>

    <div class="detail-close"></div>
  </div>
</body>
</html>  