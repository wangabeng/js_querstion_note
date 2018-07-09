
图片上传 浏览器 存储base 64位的图片(20180709更新 每次只能上传一张)
```
html:
    <div class="rechargeBox">
        <span>凭　　证：</span>
        <a href="javascript:;" class="a-upload">
            <input type="file" id="img" class="pzupload" value="" accept="image/*" />上传
        </a>
        <div class="showFileName">请选择文件</div>
    </div>
    <div id="photo" class="rechargeImg">图片预览</div>
    <div class="rechargeBtn">
        <a href="javascript:;">提交</a>
        <a href="javascript:;">取消</a>
    </div>
js:
    $(function () {
        $('#img').change(function () {
            var file = this.files[0];
            var r = new FileReader();
            r.readAsDataURL(file);
            $(r).load(function () {
                $('#photo').html('<img src="' + this.result + '" alt="" />');
            })
        })
    })
```


20180709
优化版 每次可以上传一张或多张（更合理）
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,minimum-scale=1.0,user-scalable=no"/>
    <meta content="yes" name="apple-mobile-web-app-capable">
    <meta content="black" name="apple-mobile-web-app-status-bar-style">
    <meta content="telephone=no" name="format-detection">
    <meta content="email=no" name="format-detection">

    <title>XXX-xxx</title>
    <link href="./css/awesome.css" rel="stylesheet">
    <link href="./css/base.css" rel="stylesheet">
    <link href="./css/common.css" rel="stylesheet">
    <link href="./css/iSlider.css" rel="stylesheet">
    <link href="./css/index.css" rel="stylesheet">
    
    <script type="text/javascript" src="./js/jquery1.10.2.js"></script>
    <script type="text/javascript" src="./js/fontSize.js"></script>
    <script type="text/javascript" src="./js/common.js"></script>
    <script type="text/javascript" src="./js/iSlider.js"></script>
    <script type="text/javascript" src="./js/iSlider.animate.js"></script>
    <script type="text/javascript" src="./js/iSlider.dot.js"></script>
    <script type="text/javascript" src="./js/iSlider.plugin.button.js"></script>
    <script type="text/javascript" src='./layer/layer.js'></script>
    <style>
        .img-div{  
            border: 1px solid #ddd;  
            border-radius: 100%;  
            float: left;  
            line-height: 1;  
            margin-left: 5px;  
            overflow: hidden;  
        }
    </style>
</head>
<body>
<form id="form"  enctype="multipart/form-data">  
        <input type="file" id="xdaTanFileImg"  multiple="multiple"  name="fileAttach" onchange="xmTanUploadImg(this)"/>  
    <div class="img-box" id="imgboxid">  
  
    </div>  
  
            <div id="xmTanDiv"></div><br/>  
                <div id="errordiv"   style="margin-top:15px;width:100%;text-align:center;">  
                <input id="bt" type="button" onclick="test(this)" value="提交" />   
            </div>  
</form> 
</body>
<script>
    //选择图片，马上预览  
    function xmTanUploadImg(obj) {  
  
        var fl=obj.files.length;  
        for(var i=0;i<fl;i++){  
            var file=obj.files[i];  
            var reader = new FileReader();  
  
            //读取文件过程方法  
  
            reader.onloadstart = function (e) {  
                console.log("开始读取....");  
            }  
            reader.onprogress = function (e) {  
                console.log("正在读取中....");  
            }  
            reader.onabort = function (e) {  
                console.log("中断读取....");  
            }  
            reader.onerror = function (e) {  
                console.log("读取异常....");  
            }  
            reader.onload = function (e) {  
                console.log("成功读取....");  
  
                var imgstr='<img style="width:100px;height:100px;" src="'+e.target.result+'"/>';  
                var oimgbox=document.getElementById("imgboxid");  
                var ndiv=document.createElement("div");  
  
                ndiv.innerHTML=imgstr;  
                ndiv.className="img-div";  
                oimgbox.appendChild(ndiv);  
                 
            }  
  
            reader.readAsDataURL(file);  
//alert(1);  
        }  
  
    } 
</script>
</html>
```
