# CSS

## 样式清单

### font

1. 最小简写形式：`font: size/height family;`其它属性直接放在size前面，顺序不影响
2. `font-family: 'Microsoft YaHei', 'SimHei', 'SimSun', 'NSimSun', 'FangSong', 'KaiTi';`指定字体
3. `font-size: 14px;`指定字体大小
4. `font-weight: bold;`字体粗细lighter->normal->bold->bolder
5. `font-style: italic;`英文斜体

指定字体大小的几种表示方法
1. 直接指定：`font-size: 14px;`
2. 相对于父级缩放：`font-size: 150%;`或`font-size: 1.5em;`
3. 关键字预设：`font-size: small;`从小到大是xx-small->x-small->small->medium->large->x-large->xx-large

### text

1. `text-decoration: underline;`文本风格
    1. none
    2. underline
    3. overline
    4. line-through
2. `text-align: center;`只能对块元素设置，对所有子级内联元素居中（不只是文本居中）

### background

1. `background: white url(images/background.gif) repeat-x;`
2. `background-color: #ffffff;`指定背景颜色
3. `background-image: url(images/background.gif);`指定背景图片
4. `background-repeat: no-repeat;`背景图片不重复
    1. no-repeat
    2. repeat-x
    3. repeat-y
    4. inherit
5. `background-position: top left;`背景图片位置
    1. top
    2. left
    3. right
    4. bottom
    5. center

### 盒模型

#### margin

1. `margin`
2. `margin-top: 30px;`
3. `margin-left: 20%;`
4. `margin-left: auto;`
5. `margin-right: 20%;`

#### border

1. `border: 2px dotted black;`
2. `border: none;`
3. `border-bottom: 1px solid black;`或`border-bottom: thin dotted #888888;`
4. `border-style: groove;`边框样式
    1. solid
    2. double
    3. groove
    4. outset
    5. dotted
    6. dashed
    7. inset
    8. ridge
5. `border-width: 5px;`或`border-width: thin;`边框宽度
    1. thin
    2. medium
    3. thick
6. `border-color: #ff0000;`边框颜色
7. `border-radius: 15px;`或`border-radius: 3em;`圆角半径（em相对于该元素font-size）

单独指定某一边（角）的某一属性
1. top
2. bottom
3. left
4. right

例：

 `border-top-color`

 `border-top-style`

 `border-bottom-color`

 `border-left-width`

 `border-top-left-radius: 3em;`

 `border-bottom-right-radius: 3em;`

#### padding

1. `padding: 10px 10px 10px 10px;`或`padding: 10px;`或`padding: 0px 10px;`（上下0px，左右10px）
2. `padding-left: 80px;`

### 布局和定位

#### 浮动布局

1. `float: right;`从流中删除，浮动到最右/左边
2. `clear: right;`不允许右/左边有浮动的元素

#### 绝对/相对定位

1. `position: absolute;`从流中删除，使用绝对定位
    1. static流
    2. absolute绝对定位
    3. fixed相对于浏览器窗口定位
    4. relative相对于流定位
2. `top: 100px;`绝对定位点，也可用百分比表示，如`top: 10%;`取浏览器窗口宽度的10%
3. `left: 100px;`
4. `right: 200px;`
5. `z-index: 1;`指定覆盖的层级（就像PS的图层一样）

#### 表格布局

1. `display: block;`指定元素类型
    1. block块元素
    2. inline内联元素
2. `display: table;`表格显示
3. `border-spacing: 10px;`表格中单元格的边框间距（作用到table）
4. `display: table-row;`表格行
5. `display: table-cell;`表格单元格
6. `vertical-align: top;`内容相对于单元格上对齐（作用到table-cell）

### 列表样式

1. `list-style`
2. `list-style-type: none;`取消列表的特殊显示，在`<nav>`中经常用到

### CSS变换

1. `transform: rotate(45deg);`变换：旋转45度
2. `transition: 1s;`指定变换持续时间

### 杂项

1. `color: maroon;`设置颜色
2. `width: 200px;`指定内容区宽度
3. line-height三种写法的继承方式不同
    1. `line-height: 1.6em;`设置行距为字体大小的1.6倍，继承时直接继承父元素的行距大小（父元素字体大小x倍数）
    2. `line-height: 1;`继承时取子孙元素自身字体大小x倍数
    3. `line-height: normal;`让浏览器指定
4. `letter-spacing`

## 其它

### 元素样式的特性

* 流模型：块元素从上到下，元素之间间隔一个换行；内联元素在块元素内部从左往右，填满一行换行
* 盒模型：内容区->padding->border->margin

块元素外边距会重叠，内联元素外边距不重叠，浮动元素外边距不重叠  
两个嵌套元素只要外面的元素没有边框，内外元素的外边距也会重叠

### 选择器特定性计算方法

设：id个数A，类/伪类个数B，元素个数C

如有：
* 选择器1：A1 B1 C1
* 选择器2：A2 B2 C2

则：
1. 如果A1>A2则1更特定
2. 如果A1=A2看B，方法同A，依次往下
3. 如果A1=A2且B1=B2且C1=C2则选择器在css文件中越靠后越特定

选择器用逗号分隔选择多个元素，视为单独选择各个元素，如：h1, h2{}视为h1{} h2{}

### 选择器的一些特殊用法

1. 选择多个元素：`h1, h2`
2. 选择某父元素下的某子元素：`div>p`
3. 选择某父元素下的某子孙元素：`div p`
4. 选择某兄弟元素后面紧跟着的某兄弟元素：`h1+p`
5. 选择某元素的某类：`p.intro`
6. 选择任何元素的某类：`.intro`
7. 选择某元素的某id：`div#footer`
8. 选择任何元素的某id：`#footer`
上述都可以混用，如： `div#footer p, p.intro img`

### 属性选择器

1. `img[width]`选择有width属性的`<img>`元素
2. `img[height="300"]`选择height属性值为300的`<img>`元素
3. `img[alt~="flowers"]`选择alt属性值中包含flowers单词的元素

### 伪类

* `a:link`
* `a:visited`
* `a:hover`
* `a:focus`
* `a:active`
* `p:hover`
* `p:first-child`
* `blockquote:last-child`

### 伪元素

* `p:first-letter{}`
* `p:first-line{}`

### 一些布局方式

#### 1. 流体（浮动）

作为边栏的元素：

```css
float: right;
width: 200px;
```

作为页脚的元素：

```css
clear: right;
```

此时正文部分如果比边栏长，正文底端的内容会延伸到边栏底端  
如果不想要这种显示方式，可以给正文部分加上 `margin-right`

#### 2. 冻结（凝胶）

冻结布局很简单，只要把整个网页用 `<div>` 包起来，给到如下样式：

```css
width: 800px;
margin-left: auto;
margin-right: auto;
```

#### 3. 绝对布局

绝对布局是使用绝对定位的布局方式

```css
position: absolute;
top: 100px;
left: 100px;
width: 100px;
z-index: 1;
```

#### 4. 表格

首先HTML里有两个 `<div>` 作为表格的整体和表格的一行

```html
<div id="tableContainer">
    <div id="tableRow">
```

加上表格中各个元素作为单元格，有三类元素需要设置样式

为表格整体添加如下样式：

```css
display: table;
border-spacing: 10px;
```

为表格行添加如下样式：

```css
display: table-row;
```

为所有作为表格单元格的元素加上：

```css
display: table-cell;
vertical-align: top;
```

### 指定什么设备类型应用什么样式

* `@media screen and (min-device-width:481px){}`
* `@media screen and (max-device-width:480px){}`
* `@media print{}`

注：也可在html中为不同设备类型link不同样式表来达到同样效果

### 修改原网页的css

在自己为了修改这个网页而写的css中，末尾加!important，如：
 `p{font-size: 14px !important;}`

### 杂项

注释 `/*xxxx*/`

css像素：96px/英寸

`@font-face{}` 用于指定Web字体
