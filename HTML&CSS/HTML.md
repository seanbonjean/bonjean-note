# HTML

## 元素清单

### 开头几个标签

`<!DOCTYPE html>` 标识HTML5

`<html>` lang="zh-cn"

### `<head>` 内元素

01. `<meta charset="utf-8">`必须放在head的第一个子元素
02. `<title>`网页标题
03. `<style>`type="text/css"
04. `<script>`放js脚本
05. `<link rel="stylesheet" href="overall.css">`链接到样式表
    01. `type="text/css"`这个属性HTML5一般不用
    02. `media="screen and (max-device-width: 480px)"`指定应用这个样式表的设备类型，也可以在css里指定，但要换成css的写法  
        可用参数有：

        01. screen/print
        02. (max/min-device-width: 480px)
        03. (orientation: landscape/portrait)

### `<body>` 内元素

01. `<h1>` `<h2>` `<h3>`几级标题
02. `<p>`id=属性使标签能够被`<a>`中的#符定位到
03. `<em>`强调文本
04. `<strong>`和em差不多，也是强调
05. `<img>`图片
    01. src=
    02. alt=如果图像无法显示就显示这个内容
    03. width=
    04. height=
06. `<a>`超链接
    01. href=
    02. title=鼠标停留时会显示这个内容
    03. target="_blank"
07. `<q>`行引用
08. `<blockquote>`块引用
09. `<br>`换行
10. `<ol>`或`<ul>`然后里面放`<li>`
11. `<code>`代码
12. `<pre>`按照输入的方式原样显示文本
13. `<del>`要删除的内容
14. `<ins>`要插入的内容
15. `<div>`id=
16. `<span>`

### HTML5新增

01. `<article>`
02. `<nav>`
03. `<header>`
04. `<footer>`
05. `<time>`时间，可以不是具体时间，此时用datetime=属性标明具体时间，如`<time datetime="2022-07-11 11:25">今天现在</time>`
06. `<aside>`
07. `<section>`
08. `<video>`
    01. controls提供控件（布尔属性，没有="xxx"）
    02. autoplay自动播放（同上）
    03. loop循环播放（同上）
    04. width=
    05. height=
    06. src="xxx.mp4"
    07. poster="xxx.png"未开始播放或无法播放时显示的图片
    08. preload=视频播放前提前加载的模式none不加载/metadata加载元数据/auto浏览器决定
09. `<source>`：在`<video>`内指定不同视频源，例：

```html
<video width="1920" height="1080">
    <source src="xxx.mp4" type='video/mp4; codecs="avc1.42E01E, mp4a.40.2"'>
    <source src="xxx.webm" ; codecs="vp8, vorbis"'><p>您的浏览器不支持播放视频</p></video>
```

10. `<mark>`记号笔形式突出显示文本
11. `<audio>`
    01. src=或用`<source src="xxx.mp3" type="audio/mpeg">`
    02. controls
    03. autoplay
    04. loop
12. `<progress>`进度条
13. `<meter>`度量计
14. `<canvas>`显示用javascript绘制的图像和动画
15. `<figure>`照片、图表、代码清单等独立内容

## 其它

### 三种元素

* 块元素：会单独成段
* 内联元素
* 空元素：不用`</xxx>`

### 一些转义字符

* `&lt;`转义<
* `&gt;`转义>
* `&amp;`转义&

### 一些通用元素

* title=
* class=
    > class=属性为元素指定类，方便css用  
    > 用class属性加入多个类：class="intro headline big"

* id=
    > id=属性方便 `<a>` 标签链接，也方便css用  
    > `<a>` 标签链接到id方法：href="index.html#intro"会连接到id为intro的地方

如果只有一个就用id=，如果有多个就用class=

### 杂项

注释 `<!--xxxxx-->`

图像最好使用thumbnail，建一个thumbnails文件夹存缩略图

w3认证https://validator.w3.org/
