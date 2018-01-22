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
