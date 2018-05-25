checkbox选中与取消选择

```
<form>
    你爱好的运动是？<br/>
    <input type="checkbox" name="items" value="足球" />足球
    <input type="checkbox" name="items" value="篮球" />篮球
    <input type="checkbox" name="items" value="羽毛球" />羽毛球
    <input type="checkbox" name="items" value="乒乓球" />乒乓球 <br/>
    <input type="button" id="CheckAll" value="全选" />
    <input type="button" id="CheckNo" value="全不选" />
    <input type="button" id="CheckRev" value="反选" />
</form>
```
    想要实现的是全选，全不选和反选三种效果，其中需要特别注意的是全选按钮这里
```
<script>
    $(function(){
        $("#CheckAll").click(function(){
            $("input:checkbox").prop("checked","checked");
        });
        $("#CheckNo").click(function(){
            $("input:checkbox").removeAttr("checked");
        });
        $("#CheckRev").click(function(){
            $("input:checkbox").each(function(){
                this.checked=!this.checked;
            });
        });
    });
</script>
```
请注意，现在使用的是prop(),如果使用attr(),那么就会出现下面这种情况：

选择“全选”按钮后，正常；点击“全不选”，正常；当这个时候再去点击“全选”按钮时，发现代码那里的“checked”=checked,但是页面上没有显示出来；

使用prop()方法后，可以解决此问题；

。。。。没有测浏览器的兼容。。。。
