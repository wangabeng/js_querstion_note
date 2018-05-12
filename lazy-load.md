# 关于image对象
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
