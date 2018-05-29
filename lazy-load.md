# 关于image对象
```
创建一个Image对象：var a=new Image();    定义Image对象的src: a.src=”xxx.gif”;    这样做就相当于给浏览器缓存了一张图片。
Image 对象的属性:
    src 设置或返回图像的 URL
    width   设置或返回图像的宽度。

DEMO:

    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="UTF-8">
    <!-- <script src='./jquery1.10.2.js'></script> -->
    <title>test</title>
    </head>
    <body>
    </video>
    <div id="output">
      <img data-src="http://blog.visme.co/wp-content/uploads/2017/07/50-Beautiful-and-Minimalist-Presentation-Backgrounds-02.jpg" src='test.png' alt="">
    </div>
    <script type="text/javascript">
    (function(){
      var A = new Image();
      var a = document.getElementsByTagName('img')[0];
      A.src = a.getAttribute('data-src');
      A.onload = function () {
        console.log('data-src:', a.getAttribute('data-src'), '加载完成');
        a.setAttribute('src', this.src);
      };
      
    })();
    </script>
    </body>
    </html>


懒加载总结：
情况1 在可视区范围内的 用以上的方法 先用显示一张相同的图片 然后待真实的图片加载完成后 替换掉这张相同的图片；
情况2 没在可视区范围的图片 此时 可视区高度（即视窗高度）+scrollTop(滚动距离) < offsetTop（元素距离文档顶部的高度）
      在可视区范围的时候  此时 可视区高度（即视窗高度）+scrollTop(滚动距离) >= offsetTop（元素距离文档顶部的高度）此时加载该图片

关于图片预加载 这里介绍得最详细了
https://segmentfault.com/a/1190000010744417
```    
```
javascript图片懒加载与预加载的分析

懒加载与预加载的基本概念。

  懒加载也叫延迟加载：前一篇文章有介绍：JS图片延迟加载 延迟加载图片或符合某些条件时才加载某些图片。

   预加载：提前加载图片，当用户需要查看时可直接从本地缓存中渲染。

 两种技术的本质：两者的行为是相反的，一个是提前加载，一个是迟缓甚至不加载。懒加载对服务器前端有一定的缓解压力作用，预加载则会增加服务器前端压力。

 懒加载的意义及实现方式有：

   意义： 懒加载的主要目的是作为服务器前端的优化，减少请求数或延迟请求数。

   实现方式： 

      1.第一种是纯粹的延迟加载，使用setTimeOut或setInterval进行加载延迟.

    2.第二种是条件加载，符合某些条件，或触发了某些事件才开始异步下载。

    3.第三种是可视区加载，即仅加载用户可以看到的区域，这个主要由监控滚动条来实现，一般会在距用户看到某图片前一定距离遍开始加载，这样能保证用户拉下时正好能看到图片。

  预加载的意义及实现方式有：

    预加载可以说是牺牲服务器前端性能，换取更好的用户体验，这样可以使用户的操作得到最快的反映。实现预载的方法非常多，可以用CSS(background)、JS(Image)、HTML(<img />)都可以。常用的是new Image();，设置其src来实现预载，再使用onload方法回调预载完成事件。只要浏览器把图片下载到本地，同样的src就会使用缓存，这是最基本也是最实用的预载方法。当Image下载完图片头后，会得到宽和高，因此可以在预载前得到图片的大小(方法是用记时器轮循宽高变化)。

怎么样才能实现预加载？

 我们可以通过google一搜索：可以看到很多人用这种方式进行预加载：代码如下：

function loadImage(url,callback) {
    var img = new Image();
    
    img.src = url;
    img.onload = function(){
        img.onload = null;
        callback.call(img);
    }
}
在google或者火狐下测试 都是正常的 不管我怎么刷新都是正常的，但是在IE6下不是这样的 我点击一下 是正常 再次点击或者重新刷新都不正常。下面的jsfiddle地址：有兴趣的同学可以试试 点击按钮后 弹出正常结果 再次点击在IE6下就不执行onload里面的方法了，接着重新刷新也不行。

 想要看效果，点击我！

为什么其他浏览器正常的：其实原因很简单，就是浏览器缓存了，除了IE6以外(即说opera也会，但是我特意用opera试了下，没有，可能版本的问题吧，或许现在已经修复了。)，其他浏览器重新点击会再次执行onload方法，但是IE6是直接从浏览器取的。

那现在怎么办？最好的情况是Image可以有一个状态值表明它是否已经载入成功了。从缓存加载的时候，因为不需要等待，这个状态值就直接是表明已经下载了，而从http请求加载时，因为需要等待下载，这个值显示为未完成。这样的话，就可以搞定了。经过google搜索下即介绍：发现有一个为各个浏览器所兼容的Image的属性——complete。所以，在图片onload事件之前先对这个值做一下判断即可。最后，代码变成如下的样子：

function loadImage(url,callback) {
    var img = new Image();
    
    img.src = url;

    if(img.complete) {  // 如果图片已经存在于浏览器缓存，直接调用回调函数
        
        callback.call(img);
        return; // 直接返回，不用再处理onload事件
    }

    img.onload = function(){
        img.onload = null;
        callback.call(img);
    }
}
也就是说如果图片已经在浏览器缓存里面 那么支持直接从浏览器缓存取得直接执行img.complete里面的函数 接着返回.

但是我们可以看到上面的代码：必须等图片加载完成后，可以执行回调函数，也可以说等图片加载后，我们可以获取图片的宽度和高度。那么如果我们想提前获取图片的尺寸那怎么办？上网经验告诉我：浏览器在加载图片的时候你会看到图片会先占用一块地然后才慢慢加载完毕，并且不需要预设width与height属性，因为浏览器能够获取图片的头部数据。基于此，只需要使用javascript定时侦测图片的尺寸状态便可得知图片尺寸就绪的状态。代码如下：（但是有个前提是 这个方式不是我想的，也不是我写的代码，是网上朋友总结的代码 我只是知道有这么一个原理）

var imgReady = (function(){
    var list = [],
        intervalId = null;

    // 用来执行队列
    var queue = function(){

        for(var i = 0; i < list.length; i++){
            list[i].end ? list.splice(i--,1) : list[i]();
        }
        !list.length && stop();
    };
    
    // 停止所有定时器队列
    var stop = function(){
        clearInterval(intervalId);
        intervalId = null;
    }
    return function(url, ready, error) {
        var onready = {}, 
            width, 
            height, 
            newWidth, 
            newHeight,
            img = new Image();
        img.src = url;

        // 如果图片被缓存，则直接返回缓存数据
        if(img.complete) {
            ready.call(img);
            return;
        }
        width = img.width;
        height = img.height;

        // 加载错误后的事件 
        img.onerror = function () {
            error && error.call(img);
            onready.end = true;
            img = img.onload = img.onerror = null;
        };

        // 图片尺寸就绪
        var onready = function() {
            newWidth = img.width;
            newHeight = img.height;
            if (newWidth !== width || newHeight !== height ||
                // 如果图片已经在其他地方加载可使用面积检测
                newWidth * newHeight > 1024
            ) {
                ready.call(img);
                onready.end = true;
            };
        };
        onready();
        // 完全加载完毕的事件
        img.onload = function () {
            // onload在定时器时间差范围内可能比onready快
            // 这里进行检查并保证onready优先执行
            !onready.end && onready();
            // IE gif动画会循环执行onload，置空onload即可
            img = img.onload = img.onerror = null;
        };
        
        
        // 加入队列中定期执行
        if (!onready.end) {
            list.push(onready);
            // 无论何时只允许出现一个定时器，减少浏览器性能损耗
            if (intervalId === null) {
                intervalId = setInterval(queue, 40);
            };
        };
    }
})();
调用方式如下：

imgReady('http://img01.taobaocdn.com/imgextra/i1/397746073/T2BDE8Xb0bXXXXXXXX-397746073.jpg',function(){
　　alert('width:' + this.width + 'height:' + this.height);
});
```
