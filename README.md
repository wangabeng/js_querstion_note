# 需要反复
javascript高级程序设计
javascript算法与结构

```
计算机核心：
1 数学课（线性代数，概率统计，离散数学，高等数学/数学分析）（大一）

2 数据结构与算法 《算法导论》 （大二）

3 计算机体系结构：看 David Patterson 和 John Hennessy 合写的书 《计算机组成与设计》 （大二）

4 操作系统：看 Tanenbaum 的书 《现代操作系统》 （大三）

5 计算机网络 （大三）

```

# js中, apply方法简单理解：
```
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
```
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply

# 关于this的指向问题
记住2句话
1 this的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定this到底指向谁，实际上this的最终指向的是那个调用它的对象
2 
```
var o = {
    a:10,
    b:{
        // a:12,
        fn:function(){
            console.log(this.a); 
        }
    }
}
o.b.fn(); //undefined 尽管对象b中没有属性a，这个this指向的也是对象b，因为this只会指向它的上一级对象，不管这个对象中有没有this要的东西。
```
https://www.cnblogs.com/pssp/p/5216085.html 这篇关于this的讲解 是最为浅显易懂的

# 数组的forEach方法遍历 获取index索引值的方法
```
var arr = ['a', 'b', 'c'];
arr.forEach((item) => {
	console.log(item, arr.indexOf(item));
}) // a 0
      b 1
      c 2
```

# jQuery方法扩展
1 注册 
```
$.fn.extend({
	tab: function () {
		console.log('data');
		// 加上this把自身返回 即把调用此方法的对象返回 this指 $('.test') 返回后即可实现链式调用 
		// return this;  
	}
});
```

2 调用 比如一个div.test调用（加上return后 可以实现链式调用）
```
$('.test').tab().css('background', 'red');
等价于
$('.test').tab();
$('.test').css('background', 'red');
```

# encodeURL及encodeURLComponent的区别：
```
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
```

# 解决html文件更改 不能刷新的问题
参考： http://coding.imooc.com/learn/questiondetail/6266.html
```
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
  ```

# 解决bash命令行ctrl+c不能杀死node进程的问题（2个进程 ctrl+c只能杀死一个进程，导致再次启动dev-server的时候无法开启服务）
解决方法 用系统自带的命令行工具 ctrl+c即可停止node进程

# (?<=exp)也叫零宽度正回顾后发断言 在js中不支持
(?<=exp)也叫零宽度正回顾后发断言，它断言自身出现的位置的前面能匹配表达式exp。比如(?<=\bre)\w+\b会匹配以re开头的单词的后半部分(除了re以外的部分)，例如在查找reading a book时，它匹配ading。

# 一个函数理清call和apply的区别
用法
```
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
```

# 关于设备像素比
```
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
```

# 正则中^的含义
http://blog.csdn.net/sufubo/article/details/50990684
```
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
```

# 秒懂原型链
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
```
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
```  

# js中new的作用
1 创建了一个实例对象
2 这个实例对象的_proto_属性指向这个实例的原型
3 改变原型方法里的this指向 this指向这个实例

# js中给对象添加属性的2种方式
1 直接添加到该对象上
2 给该对象的prototype模式给对象添加属性和方法

# 手写ajax实现
```
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
```

# 浏览器理解
浏览器是直接在用户端使用的 实现和用户交互 所以浏览器在前端扮演很重要的角色

#浏览器内核
```
Trident(IE内核 )
Gecko(Firefo内核) // 会自动更新 一般不会出现兼容性问题
Blink(Google和Opera SOftware内核)
Webkit(Safari内核 Chrome内核原型， 开源)
Presto(Opera前内核 现在已经废弃了)
```

# css样式权重
!important > 内嵌样式 > ID > class >tagname | 伪类 | 属性选择器 > 伪对象 > 继承 > 通配符
css代码存放位置查看优先级
内嵌样式 > 内部样式表 > 外联样式表

# html5的新特性
```
a 拖拽释放 (Drag and drop) API
b 语义化更好的内容标签 header nav footer aside article section
c 音频 视频 audio video
d 画布canvas API
e 地理位置 Geolocation API
f 本地离线存储 localStorage
g sessionStorage 关闭浏览器后会自动删除
h 表单控件 calendar data time email url search
i 新的技术 webworker websocket Geolocation
```

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
```
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
```

  通过css3实现(可以用flex布局)
  ```
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
```

# 一面 上限
ajax请求原理
跨域请求解决方法

# 关于面试的启发
http://v.youku.com/v_show/id_XMjk3MzgyMTU4MA==.html?spm=a2h0k.8191407.0.0&from=s1.8-1-1.2
1 心态 淡定 可能不是我low 可能太
2 当作一次查漏补缺

# json深拷贝
方法1
```
function cloneJson (json) {
  return JSON.parse(JSON.stringify(json));
}
```
方法2
```
function clone (json) {
  var isArray = json instanceof Array;
  var _json = isArray? []: {};
  for (var k in json) {
    var _json[k] = json[k] isinstanceof Object? clone(json[k]): json[k];
  }
  return _json;
}
```

# css盒子模型
```
盒子模型包括border padding width height
比如：
<div>
    sdfsfdsfffffffffffffffffffffffff
</div>
设置width:150px height: 150
在低版本的IE（5 6）中 高度会被文字撑开 实际高度会大于150px(兼容性问题)
```

# 导致产生跨域问题的三个条件
1 浏览器限制
2 跨域
3 xhr请求

# 剖析Vue原理&实现双向绑定MVVM
https://segmentfault.com/a/1190000006599500

# 正则子项匹配
需求： 把字符串转成驼峰命名 例如 ben-wang-haha 转成benWangHaha
用正则子项
```
var str = 'ben-wang-haha';
function change (str) {
  var reg = /-(\w)/g;
  return str.replace(reg, function ($0, $1) { // $1表示(\w)
    console.log($0 + 'and' + $1); // -w and w -h and h匹配一次 执行一次
    return $1.toUpperCase();
  })
}
console.log(change(str)); // benWangHaha
```

# 如何遍历出26个字母
```
  <script>
  for(var i=0;i<26;i++){
      document.write(String.fromCharCode(65+i));//输出A-Z  26个大写字母
  }
  for(var i=0;i<26;i++){
      document.write(String.fromCharCode(97+i));//输出a-z  26个小写字母
  }
  </script>
```

# 需求123456789 变成 123,456,789
```
var str = '123456789';
function change (str) {
  return str.replace(/(?!(?=\b))(?=(\d{3})+$)/g, ',');
}

console.log(change(str));
```

# 正则里如何使用变量
```
var str = 'ssgggaaaassssbsssss';
function change (str) {
  var regN = 's';
  var reg = new RegExp(regN, 'g');
  return str.match(reg).length;
}
console.log(change(str)); // benWangHaha
```

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
```
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
```

# 滴滴难题
  https://www.cnblogs.com/foolgry/p/5309192.html
  ```
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
```

# 父元素高度设置为min-height后 子元素的height 100%无效 why?
  父及元素 div 设置 min-height:400px; 
  子元素 div设置 height:100%； 
  奇怪的是子元素的100%并没有把400px继承过来，100%的高度设置失效
```
  <div style="min-height:400px; background:#000;" class="a1">
    <div style="height:100%; background:#fff;" class="b1"></div>
  </div>
```
  原因：min-height 是在 height 计算之后再套用的。在计算容器 height 时，默认值为 auto，故由其内容决定。这种情况下子元素百分比的 height 都会当作 auto 处理。例子中算出子元素高度 0，于是容器得到 height 为 0，比 min-height 小，所以最后容器应用 min-height。

  总结：height 100%失效原因 
  1 此元素的父级的父级的父级 没有设置特定的高度 或是即便设置了min-height类似的高度
  2 父级没有加定位 fixed absolute等

# 子元素margin-top属性传递给父元素的问题  
```
  html结构：
  <div class="box1"><div class="box1_1"></div></div>

  css样式：
  .box1{height:400px;background:#fad;}
  .box1_1{height:100px;margin-top:50px;background:#ade;}
```
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
```  
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
```

# jquery插件开发的集中方式
https://www.cnblogs.com/ajianbeyourself/p/5815689.html

# html5的data属性
 ```
 <div id = 'read-haha' data-role='page' data-options='{"name":"John"}'></div>

  如何读取data的属性
  $('read-haha').data('role') // page
  $('read-haha').data('options') // {"name":"John"}
```

# chorme强制刷新页面快捷键
普通刷新 ctrl + r
强制刷新 ctrl + F5

# 用权重值决定哪种样式胜出
  一个内联样式 1000分
  一个ID选择器样式 100分
  一个类选择器 10分
  一个标签选择器 1分

# 几个重要的CSS3应用
1 transfrom 放大缩小等变换 这个变换是瞬间完成的 比如 当鼠标移入的时候 从一种状态变换到另外一种状态 没有过渡的效果 瞬间完成 需要触发 比如鼠标悬停的时候触发
```
  transform: rotate(10deg) // 旋转10度  
  transform: scale(2) 
  transform: scale(2) 
  transform: translate(1px,2px) // 水平方向和垂直方向移动一定位置
  transform: translate3d(1px,2px,0) // 水平方向和垂直方向移动一定位置
  transform: skew(0, 45deg) // 水平或竖直方向倾斜一定位置

  transform-origin: 0 0  或者 right bottom 或者 0% 0%  // 设置中心点
```

2 transition 设置在一定时间内 一组CSS属性变换到另一组属性的动画展示过程
例如 设置一个横幅在2秒内旋转360度。和tranform区别就是有动画过渡效果。  
这个动画需要触发 比如鼠标悬停 等
示例：
```
.test {
  color: red;
  background-color: green;

  transition-property: background-color; // 可以设置多个动画属性 用逗号隔开
  transition-duration: 1s;
  transition-timing-function: ease-in-out; // 深入用法 transition-timing-function: cubic-bezier(.20, .96, .74, .07) //使用函数控制动画过渡缓急
  transition-delay: 0.5s // 设置动画延迟
}
.test:hover {
  background-color: red;
}

transition快捷设置方法:
.test {
  color: red;
  background-color: green;
  transition: color 1s, background-color 2s;
}
.test:hover {
  color: blue;
  background-color: yellow;
}
```

3 animation动画 不需要触发
创建动画的2个步骤：
第一步： 定义动画
第二步： 定义关键帧
示例：
让一个元素从不显示到淡入视图
第一步： 定义动画
```
@keyframes fadeIn {
  from {
    opacity: 0;
  }
  50% {
    tranform: scale(1.2);
    opacity: 0.6;
  }

  to {
    opacity: 1;
  }
}
```
第二步： 应用动画
.test {
  animation-name: fadeIn;
  animation-duration: 1s;
}
也可以同时给一个元素定义多个动画并应用 例如
.test {
  animation-name: fadeIn, blink;
  animation-duration: 1s, 3s; // 定时
  animation-timing-function: ease-out;
  animation-delay: 1s; // 设置动画延时
  animation-iteration-count: 10/infinite; // 设置动画播放的次数 10次或无限制重复播放
}

animation的快捷方式应用
.test {
  animation-name: fadeIn 2s ease-in-out 2 alternate 0.5s;
}
如何暂停动画：
只有一个方法能应用它 用一个伪类触发 例如鼠标悬停来暂停动画。
.test:hover {
  animation-play-state: paused;
}

# css3 防止浮动下落：(IE8及以上均支持) box-sizing兼容写法
  box-sizing: content-box; // 盒子宽度包含了border padding css width
  box-sizing: padding-box; // 盒子宽度包含了padding css width
  box-sizing: border-box; // 盒子宽度包含了border css width

  一般在全局样式里设置border-box属性
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;

# 解决移动端点击事件延迟 用css属性 
  touch-action: manipulation;
  CSS属性 touch-action 用于指定某个给定的区域是否允许用户操作，以及如何响应用户操作（比如浏览器自带的划动、缩放等）。
  我们平常说的移动端300ms延迟，就可以使用 touch-action: manipulation; 来解决。

# 移动端手机滚动插件iscroll.js (官网)

# jQuery上下滑动加载刷新插件 iscroll.js
http://www.jq22.com/jquery-info6625

# css shadow兼容性写法
http://blog.csdn.net/xiaoya_syt/article/details/52370715
```
.box_shadow{ 
  background-color: #eee; 
  filter: progid:DXImageTransform.Microsoft.Shadow(color='#969696', Direction=135, Strength=5);/*for ie6,7,8*/ 
  -moz-box-shadow:2px 2px 5px #969696;/*firefox*/ 
  -webkit-box-shadow:2px 2px 5px #969696;/*webkit*/ 
  box-shadow:2px 2px 5px #969696;/*opera或ie9*/ 
}
```

# 文字多行显示 溢出用省略号代替
```
单行
html
<p style="width: 300px;overflow: hidden;white-space: nowrap;text-overflow: ellipsis;">

效果如图：
文本的溢出显示省略号同学们......

多行文本:
  font-size: .2rem;
  line-height: .3rem;
  height: .9rem;
  overflow: hidden;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 3; /*自动显示3行 多出的部分用...代替*/
```

# 移动端小结--正常网站都会用到的东西
```
H5页面窗口自动调整到设备宽度，并禁止用户缩放页面
<meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no" />

这meta的作用就是删除默认的苹果工具栏和菜单栏。
content有两个值”yes”和”no”,当我们需要显示工具栏和菜单栏时，这个行meta就不用加了，默认就是显示。
<meta content="yes" name="apple-mobile-web-app-capable">

忽略将页面中的数字识别为电话号码
<meta name="format-detection" content="telephone=no" />

忽略Android平台中对邮箱地址的识别
<meta name="format-detection" content="email=no" />

当网站添加到主屏幕快速启动方式，可隐藏地址栏，仅针对ios的safari
<meta name="apple-mobile-web-app-capable" content="yes" />
<!-- ios7.0版本以后，safari上已看不到效果 -->

将网站添加到主屏幕快速启动方式，仅针对ios的safari顶端状态条的样式
<meta name="apple-mobile-web-app-status-bar-style" content="black" />
<!-- 可选default、black、black-translucent -->
```

# 控制文本段落第一个字母的样式或缩进
```
p:first-letter {margin-left: 2em; color: red} // css3是2个冒号p::first-letter IE8不支持2个冒号 CSS3继续支持单冒号 所以 可以继续使用单冒号
```

# 三角形边线
https://blog.csdn.net/qq_34645412/article/details/78062304

# 自定义滚动条样式
```
/* 滚动条样式 */
.course-content-outer::-webkit-scrollbar {/*滚动条整体样式*/
    width: 8px;     /*高宽分别对应横竖滚动条的尺寸*/
    height: 4px;
}
.course-content-outer::-webkit-scrollbar-thumb {/*滚动条里面小方块*/
    border-radius: 5px;
    -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
    background: rgba(0,0,0,0.2);
}
.course-content-outer::-webkit-scrollbar-track {/*滚动条里面轨道*/
    -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
    border-radius: 0;
    background: rgba(0,0,0,0.1);
}
/* 滚动条样式结束 */
```

# body设置高度100%的时候出的问题
如果body的子元素加了margin-top 则margin-top传递给了body 导致出现滚动条 

# 单行 多行文本溢出省略号
```
单行 html width: 70%; white-space: nowrap; overflow: hidden; text-overflow: ellipsis;

效果如图： 文本的溢出显示省略号同学们......

多行文本: font-size: .2rem; line-height: .3rem; height: .9rem; overflow: hidden; display: -webkit-box; -webkit-box-orient: vertical; -webkit-line-clamp: 3; /自动显示3行 多出的部分用...代替/
```

# jquery获取rem的值会丢失小数点后面的px大小
参考： https://segmentfault.com/a/1190000009694995
计算方法，精度损失来自于浏览器将rem转成px的过程。
element.currentStyle ? element.currentStyle : window.getComputedStyle(element, null)
可以获取px小数点后的数值
例：
```
<pre>
<code>
function getElementStyle(element) {
	return element.currentStyle ? element.currentStyle : window.getComputedStyle(element, null);
}
getElementStyle($('.abc')[0]) // 获取到样式集合
</code>
</pre>
```

# html+jquery实现图片上传预览效果 完整代码：
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>HTML5上传单个图片预览</title>
</head>
<body>
	<h3>请选择图片文件：JPG/GIF/PNG</h3>
	<form name="form0" id="form0" >
	<input type="file" name="file0" id="file0"/><br>
	<img src="" id="img0" width="200px" height="auto">
	</form>

	<script src="http://libs.baidu.com/jquery/1.10.2/jquery.min.js"></script>
	<script> 
		$("#file0").change(function(){
			var objUrl = getObjectURL(this.files[0]) ;
			if (objUrl) {
				$("#img0").attr("src", objUrl) ;
			}
		}) ;
		//建立一个可存取到file的url
		function getObjectURL(file) {
			var url = null; 
			if (window.createObjectURL!=undefined) { // basic
				url = window.createObjectURL(file);
			} else if (window.URL!=undefined) { // mozilla(firefox)
				url = window.URL.createObjectURL(file);
			} else if (window.webkitURL!=undefined) { // webkit or chrome
				url = window.webkitURL.createObjectURL(file);
			}
			return url;
		}
	</script>
</body>
</html>
```

```
<!DOCTYPE html>  
<html>  
<head>  
<title>HTML5上传图片预览</title>  
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">  
<script src="http://www.codefans.net/ajaxjs/jquery-1.6.2.min.js"></script>  
</head>  
<body>  
<h3>请选择图片文件：JPG/GIF</h3>  
<form name="form0" id="form0" >  
<!-- 这里特别说一下这个 multiple="multiple" 添加上这个之后可以一次选择多个文件进行上传，是 html5 的新属性-->  
<input type="file" name="file0" id="file0" multiple="multiple" /><br><img src="" id="img0" >  
</form>  
<script>    
$("#file0").change(function(){  
  // getObjectURL是自定义的函数，见下面  
  // this.files[0]代表的是选择的文件资源的第一个，因为上面写了 multiple="multiple" 就表示上传文件可能不止一个  
  // ，但是这里只读取第一个   
  var objUrl = getObjectURL(this.files[0]) ;  
  // 这句代码没什么作用，删掉也可以  
  // console.log("objUrl = "+objUrl) ;  
  if (objUrl) {  
    // 在这里修改图片的地址属性  
    $("#img0").attr("src", objUrl) ;  
  }  
}) ;  
//建立一個可存取到該file的url  
function getObjectURL(file) {  
  var url = null ;   
  // 下面函数执行的效果是一样的，只是需要针对不同的浏览器执行不同的 js 函数而已  
  if (window.createObjectURL!=undefined) { // basic  
    url = window.createObjectURL(file) ;  
  } else if (window.URL!=undefined) { // mozilla(firefox)  
    url = window.URL.createObjectURL(file) ;  
  } else if (window.webkitURL!=undefined) { // webkit or chrome  
    url = window.webkitURL.createObjectURL(file) ;  
  }  
  return url ;  
}  
</script>  
</body>  
</html>  

```

# 图片懒加载
https://zhuanlan.zhihu.com/p/24057749

# 应用CDN的问题
This request has been blocked; the content must be served over HTTPS.
https://segmentfault.com/q/1010000005872734/a-1020000005874533
解决办法：
对于同时支持HTTPS和HTTP的资源，引用的时候要把引用资源的URL里的协议头去掉，浏览器会自动根据当前是HTTPS还是HTTP来给资源URL补上协议头的，可以达到无缝切换。

# 根据手机客户端浏览器跳转不同的页面
```
function locationDiffUrl () {
    var browser = {
        versions: function () {
            var u = navigator.userAgent, app = navigator.appVersion;
            return {//移动终端浏览器版本信息
                trident: u.indexOf('Trident') > -1, //IE内核
                presto: u.indexOf('Presto') > -1, //opera内核
                webKit: u.indexOf('AppleWebKit') > -1, //苹果、谷歌内核
                gecko: u.indexOf('Gecko') > -1 && u.indexOf('KHTML') == -1, //火狐内核
                mobile: !!u.match(/AppleWebKit.*Mobile.*/), //是否为移动终端
                ios: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/), //ios终端
                android: u.indexOf('Android') > -1 || u.indexOf('Linux') > -1, //android终端或者uc浏览器
                iPhone: u.indexOf('iPhone') > -1 || u.indexOf('Mac') > -1, //是否为iPhone或者QQHD浏览器
                iPad: u.indexOf('iPad') > -1, //是否iPad
                webApp: u.indexOf('Safari') == -1 //是否web应该程序，没有头部与底部
            };
        }(),
        language: (navigator.browserLanguage || navigator.language).toLowerCase()
    }
    if (!browser.versions.mobile && !browser.versions.ios && !browser.versions.android) {
        window.location.href = "pchome.html";
    } else {
        window.location.href = "mbhome.html";
    }
}
```

# Chrome浏览器表单自动填充默认样式 显示黄色 非常难看
样式分析
之所以出现这样的样式, 是因为Chrome会自动为input增加如下样式.
```
input:-webkit-autofill, textarea:-webkit-autofill, select:-webkit-autofill {
    background-color: rgb(250, 255, 189);
    background-image: none;
    color: rgb(0, 0, 0);
}
```
这个样式的优先级也比较高. 
无法通过important覆盖(这就比较恶心了).
解决方法：
1. 关闭浏览器自带填充表单功能

如果你的网站安全级别高一些, 可以直接关闭. 也不需要再调样式了.
<!-- 对整个表单的设置 -->
<form autocomplete="off">
<!-- 单独对某个组件设置 -->
<input type="text" autocomplete="off">
<!-- 对整个表单的设置 -->
<form autocomplete="off">
<!-- 单独对某个组件设置 -->
<input type="text" autocomplete="off">
PS: 毕竟是一个很好的功能, 关了多不方便.

2. 通过纯色的阴影覆盖底(huang)色
input:-webkit-autofill {
 -webkit-box-shadow: 0 0 0px 1000px white inset;
 -webkit-text-fill-color: #333;
}
注: 这种只适用于纯色背景的输入框.

3. 通过设置input样式动画
推荐使用这种的. 因为基本上没有人会等那么久…
```
<!-- 99999s 基本上就是一个无限长的时间 
    通过延长增加自动填充背景色的方式, 是用户感受不到样式的变化
-->
input:-webkit-autofill,
    input:-webkit-autofill:hover,
    input:-webkit-autofill:focus,
    input:-webkit-autofill:active {
        -webkit-transition-delay: 99999s;
        -webkit-transition: color 99999s ease-out, background-color 99999s ease-out;
}
```

## 资源预加载 摘自张鑫旭（2018 此功能已经被谷歌启用 以后不要用这个功能了 谷歌浏览器自带预加载可能的链接）
<link rel="prefetch" href="(url)">
其实HTML5中有原生的预加载属性，名为prefetch和prerender

## jquery对象绑定多个事件 jquery高版本支持
```
$("p").on({

    mouseover:function(){$("body").css("background-color","lightgray");}, 

    mouseout:function(){$("body").css("background-color","lightblue");}, 

    click:function(){$("body").css("background-color","yellow");} 

  });
  ```
## 关于video手机端的几个问题
1 手机端播放自动全屏的问题解决办法
为video标签添加属性：
```
x5-playsinline="" playsinline="" webkit-playsinline=""
```
2 手机端设置了autoplay 无效的解决棒法
本身就是为了尊重用户 在IOS或安卓关闭了video自动播放的问题
如果在JS代码中 在开机后强行开启video
如：
```
    setTimeout(function () {
      $('video')[0].play();
    }, 1000);
```
则会报错
Uncaught (in promise) DOMException: play() failed because the user didn't interact with the document first.
原因分析：只有用户点击播放按钮等行为才能触发video的播放方法。
按照别人的说法：移动端必须要有用户交互才能播放的。
你可以监听touchstart事件，触摸屏幕后应该就可以播放了。

## 解决动态添加元素绑定事件在ios上面失效
动态append到页面中的新元素 绑定事件需要这样绑定
// 'body'需要冒泡的元素 element 事件绑定的元素
$('body').on('click',element,fn);
但是在IOS系统上会无效 解决办法： 
在绑定的元素上添加一段css属性即可：
cursor:pointer

## 解决jquery和zepto同时使用的冲突问题
在jQuery引用完了后，调用下$.noConflict()，比如：
```
<script type="text/javascript" src="js/jquery.min.js"></script>
<script>$.noConflict();</script>
<script type="text/javascript" src="js/zepto.min.js"></script>
```
之后在使用jQuery的$符号时，换成jQuery就行了，比如：
```
jQuery(document).ready(function(xxx) {
    // 这里是使用 jQuery $ 的代码
});
```

## CSS3中trasition和animation重复执行的问题

每次触发动画的时候 
首先取消动画 然后添加动画 此时动画只能执行一次（拿抽奖动画来讲 就是这样）
问题根源：
这是浏览器渲染机制导致的。
页面等待js线程执行完毕，随后执行GUI线程渲染，而removeClass随后addClass这样的代码，对于GUI线程来说页面上没有任何改变，自然不会做任何动作。

而设置了setTimeout之后，执行顺序为 js线程执行完毕 -> GUI线程渲染（此时页面的class已去除）-> 异步线程执行完毕 -> GUI线程渲染（此时页面的class已添加）

在这样的机制下，如果你把removeClass和addClass同时放在setTimeout中，设置时间间隔大于一定时间（w3c标准是25ms,但实际能分辨的最小时间间隔会更小一点，因浏览器而异），也有同样的效果。

解决方法1  增加一个GUI线程渲染的命令
        $('.test1').removeClass('active');
        $('.test1').width(); //  GUI线程渲染
        $('.test1').addClass('active');

解决方法2
	用定时器
        $('.test1').removeClass('active');
        setTimeout(function () { // 3000毫秒后 让浏览器重新渲染完整个元素属性
          $('.test1').addClass('active');
        }, 3000);
	
## css3 animation-timing-function: cubic-bezier(.77,.19,.6,.95);
设置CSS3的动画缓急函数 http://cubic-bezier.com

# 为元素动态绑定事件 即为未来元素绑定事件
```
//这里的ParentEle是 thisEle的父辈元素或者祖先元素，ParentEle可以是document，也可以是body等。<br><br><br>//注意：如果此时调用的函数是外部定义好的函数，那在调用的时候不要加（），不然会跳过点击事件直接触发函数
$("body").on("click", ".newBtn", function() {
	alert('这里是动态元素添加的事件');
}); //.newBtn是要绑定的元素
```
事件代理遇到的问题：
在IOS系统中点击不触发点击事件。解决方法：
例如：
```
<div class="name">点我</div>
$(document).on("click", ".name", function() {
    alert("name");
});
```
解决办法：
1 给这个捕获事件的元素（即最终需要添加到点击事件本身的元素）添加一个CSS属性：
.name{ 
cursor:pointer; 
} 
2 仍然用click来触发点击事件。（亲测有效）

# 解决苹果系统在移动端button按钮 或input type=button 按钮 颜色自动渐变的问题
```
/* 苹果手机上input的button颜色变蓝变绿 */
input[type=button], input[type=submit], input[type=file], button {
    cursor: pointer;
    outline: 0px;
    -webkit-appearance:none;
}
```

# 对象拷贝 深拷贝
1 原生js深拷贝。通过递归，来拷贝深层的对象
```
var obj = {a:{b:10}};
function deepCopy(obj){
    if(typeof obj != 'object'){
        return obj;
    }
    var newobj = {};
    for ( var attr in obj) {
        newobj[attr] = deepCopy(obj[attr]);
    }
    return newobj;
}
var obj2 = deepCopy(obj);
obj2.a.b = 20;
alert(obj.a.b); //10 
```
2 jQuery深拷贝$.extend(true,object1, object2);
```
// jQuery中$.extend(true,object1, object2);可以深拷贝对象
// 实际使用中默认参数
var object1 = {
a: 0,
b: {
    gg: 11,
    mm: 22
}
};
// 实际使用中传递的参数 用于覆盖默认参数
var object2 = {
b: {
    mm: 333
},
c: 100
};
console.log('extend', $.extend(true,object1, object2));
// $.extend(true,object1, object2); // object2保持不变 把object2中的属性拷贝或覆盖到object1中 最后使用object1的数据 相当于$.extend(true,object1, object2)返回的数据
console.log('obj1', object1);
console.log('obj2', object2);

```

# jquery给一个元素直接设置data值 如 $('.aaa').data('deg', 30)与设置$('.aaa').attr('data-deg', 30)属性的区别
前者在html中不会体现出data-deg的属性及值 后者则会提现在html中  data-deg='30' 注意 这个值是字符串

# 抽奖案例中关于call的改变this指向的使用 配合ajax请求异步使用数据
1 定义构造函数，定义属性及方法:
```
// js部分
(function (window, document, $) {
	var defaultOpts = {
		callback: function () {}
	};
	function constructFn (opts) {
		this.opts = $.extend(true, defaults, opts); // 覆盖参数
		this.init();
	}
	// 初始化 在构造函数中执行
	constructFn.prototype.init = function () {
		// 使用opts对象中的方法 并且改变在使用callback这个函数环境中this的指向
		this.opts.callback.call(this); // 执行这个callback方法
	}；
	// 在回调函数中执行 没有在构造函数中执行
	constructFn.prototype.dosomething = function (msg) {
		alert(msg);
	};
)(widonw, document, $);
```
使用：
1 引用此js文件
2 使用方法
```
new constructFn({
	// 定义callback
	callback: function () {
		var _this = this;
		// 发送ajax请求 然后在回调中使用此实例对象的方法 dosomething
		$.ajax({
		    url:'/comm/test1.php',
		    type:'POST', //GET
		    async:true,    //或false,是否异步
		    data:{
			name:'yang',age:25
		    },
		    success:function(data,textStatus,jqXHR){
			console.log(data);
			console.log(textStatus);
			console.log(jqXHR);
			// 在回调中使用此实例对象的方法 dosomething(按理说是不可以直接用_this.dosomething使用的 ，但是因为在这里_this的指向已经改变，_this指向的已经不是这个opts对象，而是这个constructFn实例化对象，这个实例化对象有dosomething方法)
			_this.dosomething(data); // 把ajax请求获取的数据传递过去
		    },
		    error:function(xhr,textStatus){
			console.log('错误')
			console.log(xhr)
			console.log(textStatus)
		    },
		})
	}
});
```
# gulp项目中运用：
## 主要用途：项目中文件的压缩
## 使用步骤
### 1 安装NodeJS
### 2 安装Gulp环境
```
$ npm install --global gulp
```
### 3 作为项目的开发依赖（devDependencies）安装
```
$ npm install --save-dev gulp

```
### 4 安装工具依赖
```
 npm install gulp-minify-css gulp-uglify gulp-concat gulp-rename gulp-jshint del --save-dev

 /////////////////

 1.css压缩 　　gulp-minify-css
 2.js压缩　　　gulp-uglify
 3.css或js合并　　　gulp-concat　
 4.重命名　　   gulp-rename
 5.js代码检测　 gulp-jslint　(或tgulp-jshint)
 6.文件删除    del
```
### 5 在根目录中创建gulpfile.js
```
 var gulp = require('gulp'),
 minifycss = require('gulp-minify-css'),
     concat = require('gulp-concat'),
     uglify = require('gulp-uglify'),
     rename = require('gulp-rename'),
     del = require('del');


 //压缩css
 gulp.task('minify_css', function () {
     var cssSrc = ['./css/*.css'];

     return gulp.src(cssSrc)      //压缩的文件
         .pipe(concat('all.css'))    //合并所有css到all.css
         .pipe(gulp.dest('./main/css'))   //输出文件夹
         // .pipe(rename({suffix: '.min'}))
         .pipe(minifycss())
         .pipe(gulp.dest('./main/css')); //执行压缩
 });


 //压缩js
 gulp.task('minify_js', function() {
     var jsSrc = ['./lib/*.js','!./lib/*.src.js'];

     return gulp.src(jsSrc)
         .pipe(concat('all.js'))    //合并所有js到all.js
         .pipe(gulp.dest('./lib'))    //输出all.js到文件夹
         .pipe(rename({suffix: '.min'}))   //rename压缩后的文件名
         .pipe(uglify())    //压缩
         .pipe(gulp.dest('./lib'));  //输出
 });

  // 默认任务
 gulp.task('default',['minify_css','minify_js']);
```
### 6 在命令行工具中执行
```
gulp
```
### 7 检查压缩文件，是否正常

备注：
1 关于gulp非常全面的介绍
https://www.cnblogs.com/2050/p/4198792.html
2 gulp和webpack是互补的 而不是互斥的 关于他们之间的区别 见
https://www.cnblogs.com/iovec/p/7921177.html

# layer不能获取弹出层input框的value值的解决方案
```
    $('.add-one').on('click', function () {
	//页面层
	curLayer = layer.open({
	    title: '添加物流信息',
	    type: 1,
	    // skin: 'layui-layer-rim', //加上边框
	    area: ['5rem', '50%'], //宽高
	    content: $('.logistics-detail-layer') // 这里不要写成$('.logistics-detail-layer').html() 
	});
    });
```
content: $('.logistics-detail-layer') // 这里不要写成$('.logistics-detail-layer').html() ，然后即可在关闭的时候获取input框的value值。

# 自定义layer弹出层
1 遮罩层
```
position: fixed; z-index: 1000;width:100%;height: 100%;opcacity: .3;background: #000
```
2 内容层  
```
position: fixed;width: 4rem;height: 40$;left: 50%;top: 50%;margin-left: -50%宽度;margin-top: -50%高度;
```

# bind方法（高程）
ECMAScript 5还定义了一个方法：bind()。这个方法会创建一个函数的实例，其this值会被绑定到传给bind()函数的值。
```
window.color = 'red';
var o = { color: 'blue' };
function sayColor(){
alert(this.color);
}
var objectSayColor = sayColor.bind(o);
objectSayColor(); //blue
```
在这里，sayColor()调用bind()并传入对象o，创建了objectSayColor()函数。objectSayColor()函数的this值等于o，因此即使是在全局作用域中调用这个函数，也会看到“blue”。
支持bind()方法的浏览器有IE9+、Firefox 4+、Safari 5.1+、Opera 12+和Chrome。

# 一个简单的css3动画 类似手风情菜单箭头向右 向下切换效果
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <script  src="https://code.jquery.com/jquery-2.2.4.js"  integrity="sha256-iT6Q9iMJYuQiMWNd9lDyBUStIq/8PuOW33aOqmvFpqI="  crossorigin="anonymous"></script>
  <style>
    .test {width: 30px;height: 300px;background-color: red;margin: 0 auto;
      -webkit-transition: all 0.4s ease;
      -o-transition: all 0.4s ease;
      /* 元素本身添加动画 没有触发的时候 动画不显示 */
      transition: transform 0.4s ease; 
    }
    .open {
      background-color: blue;
      -webkit-transform: rotate(180deg);
      -ms-transform: rotate(180deg);
      -o-transform: rotate(180deg);
      transform: rotate(180deg);
    }
  </style>
</head>
<body>
<div class="test">123233</div>
<input type="button" class='btn' value='chufa'>
<script>
$('.btn').on('click', function () {
  $('.test').toggleClass('open');
});
</script>
</body>
</html>
```
