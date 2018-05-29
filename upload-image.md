
图片上传 浏览器 存储base 64位的图片
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
