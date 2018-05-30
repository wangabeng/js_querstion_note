sublime 自动添加兼容前缀插件autoprefixer

```
安装插件autoprefixer步骤：

1、确保Node.js已经安装，未安装请 点击 这里>>

2、下载autoprefixer插件 https://github.com/sindresorhus/sublime-autoprefixer

3、打开sublime ，选择菜单 Preferences >  Browse Packages 将下载的压缩包解压到这

4、设置快捷键，选择菜单Preferences > Key Bindings – User 

[
    { "keys": ["ctrl+alt+shift+p"], "command": "autoprefixer" }
]
 

 

可选------------------------

5、默认的是没有兼容IE/opera的-ms/-o，选择菜单：Preferences > Package Settings > Autoprefixer > Settings - User

例子1：为浏览最新版本添加前缀，市场份额大于%，美国份额>5%，ie8和ie7

{
    "browsers": ["last 1 version", "> 10%", "> 5% in US", "ie 8", "ie 7"]
}
例子2：

{
    "browsers": ["last 2 versions","Firefox >= 20"]
}
.a{
    display:flex;
}
输出

.a{
    display:-webkit-flex;
    display:-moz-box;
    display:-ms-flexbox;
    display:flex;
}
参考写法：

last 2 versions	每一个主要浏览器的最后2个版本
last 2 Chrome versions	谷歌浏览器的最后两个版本
> 5%	市场占有量大于5%
> 5% in US	美国市场占有量大于5%
ie 6-8	ie浏览器6-8
Firefox > 20	火狐版本>20
Firefox >= 20	火狐版本>=20
Firefox < 20	火狐<20
Firefox <= 20	火狐<=20
iOS 7	指定IOS 7浏览器
更多请参考：https://github.com/ai/browserslist#queries
```
