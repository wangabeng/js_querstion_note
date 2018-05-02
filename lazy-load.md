https://zhuanlan.zhihu.com/p/24057749 摘自知乎
# 前端如何实现图片懒加载(lazyload) 提高用户体验
来源：http://i.jakeyu.top/2016/11/26/实现图片懒加载/
作者：Jake

懒加载的意义（为什么要使用懒加载）
原理
代码

对页面加载速度影响最大的就是图片，一张普通的图片可以达到几M的大小，而代码也许就只有几十KB。当页面图片很多时，页面的加载速度缓慢，几S钟内页面没有加载完成，也许会失去很多的用户。

所以，对于图片过多的页面，为了加速页面加载速度，所以很多时候我们需要将页面内未出现在可视区域内的图片先不做加载， 等到滚动到可视区域后再去加载。这样子对于页面加载性能上会有很大的提升，也提高了用户体验。

原理

将页面中的img标签src指向一张小图片或者src为空，然后定义data-src（这个属性可以自定义命名，我才用data-src）属性指向真实的图片。src指向一张默认的图片，否则当src为空时也会向服务器发送一次请求。可以指向loading的地址。


<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
        img {
            display: block;
            margin-bottom: 50px;
            width: 400px;
            height: 400px;
        }
    </style>
</head>
<body>
    <img src="default.jpg" data-src="http://ww4.sinaimg.cn/large/006y8mN6gw1fa5obmqrmvj305k05k3yh.jpg" alt="">
    <img src="default.jpg" data-src="http://ww4.sinaimg.cn/large/006y8mN6gw1fa5obmqrmvj305k05k3yh.jpg" alt="">
    <img src="default.jpg" data-src="http://ww1.sinaimg.cn/large/006y8mN6gw1fa7kaed2hpj30sg0l9q54.jpg" alt="">
    <img src="default.jpg" data-src="http://ww1.sinaimg.cn/large/006y8mN6gw1fa7kaed2hpj30sg0l9q54.jpg" alt="">
    <img src="default.jpg" data-src="http://ww4.sinaimg.cn/large/006y8mN6gw1fa5obmqrmvj305k05k3yh.jpg" alt="">
    <img src="default.jpg" data-src="http://ww4.sinaimg.cn/large/006y8mN6gw1fa5obmqrmvj305k05k3yh.jpg" alt="">
    <img src="default.jpg" data-src="http://ww4.sinaimg.cn/large/006y8mN6gw1fa5obmqrmvj305k05k3yh.jpg" alt="">
    <img src="default.jpg" data-src="http://ww4.sinaimg.cn/large/006y8mN6gw1fa5obmqrmvj305k05k3yh.jpg" alt="">
    <img src="default.jpg" data-src="http://ww1.sinaimg.cn/large/006y8mN6gw1fa7kaed2hpj30sg0l9q54.jpg" alt="">
    <img src="default.jpg" data-src="http://ww4.sinaimg.cn/large/006y8mN6gw1fa5obmqrmvj305k05k3yh.jpg" alt="">
    <img src="default.jpg" data-src="http://ww4.sinaimg.cn/large/006y8mN6gw1fa5obmqrmvj305k05k3yh.jpg" alt="">
</body>

JavaScript

<script>
    var num = document.getElementsByTagName('img').length;
    var img = document.getElementsByTagName("img");
    var n = 0; //存储图片加载到的位置，避免每次都从第一张图片开始遍历
    lazyload(); //页面载入完毕加载可是区域内的图片
    window.onscroll = lazyload;
    function lazyload() { //监听页面滚动事件
        var seeHeight = document.documentElement.clientHeight; //可见区域高度
        var scrollTop = document.documentElement.scrollTop || document.body.scrollTop; //滚动条距离顶部高度
        for (var i = n; i < num; i++) {
            if (img[i].offsetTop < seeHeight + scrollTop) {
                if (img[i].getAttribute("src") == "default.jpg") {
                    img[i].src = img[i].getAttribute("data-src");
                }
                n = i + 1;
            }
        }
    }
</script>

jQuery

<script>
    var n = 0,
        imgNum = $("img").length,
        img = $('img');
    lazyload();
    $(window).scroll(lazyload);
    function lazyload(event) {
        for (var i = n; i < imgNum; i++) {
            if (img.eq(i).offset().top < parseInt($(window).height()) + parseInt($(window).scrollTop())) {
                if (img.eq(i).attr("src") == "default.jpg") {
                    var src = img.eq(i).attr("data-src");
                    img.eq(i).attr("src", src);
                    n = i + 1;
                }
            }
        }
    }
</script>
