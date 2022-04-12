# html&&css

+ 注意

  ```java
  /*
   1. 多个name相同的单选框，在同一时间只有一个单选框能被选中 ；没有指定name属性的单选框，在任何时候都能被选中(底层给了一个独一无二的name属性，导致其始终只有一个，在任何时候都可以是选中状态)
   2. 标签元素的文档流特点只会对其最近一层父级标签有效，如 <h1><div>1</div></h1> 此 div会铺满整个h1盒子
  */
  ```

+ emt语法

  ```java
  /*
   
  HTML简写，一种是在模板上改，一种是使用简写规则。
  
  简写方法，主要是Emmet（常用项目）和Haml（ruby项目），本文主要介绍使用Emmet简写规则。
  
  Emmet是一个插件，基本用法是简写形式+转化键。
  
  转化键，不同IDE不同，MAC OS的是“tab键”，Wins是"<Ctr+y>,"（ctrl键+y键+逗号键）
  简写形式举例(原理就是类似于css定位形式)：
  标签名（使用p代替举例）
  1.p
  例：输入p 按Tab键，===>   <p></p>
  
  2.p#id
  例：p#addr 按Tab键，===>   <p id="attr"></p>
  
  3.p.class
  例：p.addr 按Tab键，===>   <p class="attr"></p>
  
  4.p[属性]
  例：p[a=attr] 按Tab键，===>   <p a="attr"></p>
  
  5.p{包含的内容}
  例：p{显示的内容} 按Tab键，===>   <p>显示的内容</p>
  
  6.标签>子标签
  例：div>p (代表p是div 的子元素)按Tab键，===>
  <div>
     <p></p>
  </div>
  7.标签+标签
  例：div+p (代表p和div 是同级元素)按Tab键，===>
  <div></div>
  <p></p>
  
  8.重复N个标签（块）*数字
  例：div>p*3   按Tab键，===>
  <div>
     <p></p>
     <p></p>
     <p></p>
  </div>
  
  9.标签属性自动编号，使用符号$
  例：div#id$.class$$*3  按Tab键，===>
  <div id="id1" class="class01"></div>
  <div id="id2" class="class02"></div>
  <div id="id3" class="class03"></div>
  10.以上进行排列组合
  
  例：html:5
  ul.menu>li*6>a[href=#]{显示内容$}
  */
  ```




+ 边框重叠处理

  ```java
  /*
   1.table去重叠边框
  table{
    border-collapse:collapse;
  }
  
  2.非table去重叠边框
  只需要设置div的margin值为边框值的相反数就行
  
   border:1px solid #333; margin-right:-1px; margin-bottom:-1px;
  */
  ```

  


+ label和input的关系

  ```java
  /*
   input标签与label标签的“合作关系”
  一直忽略了input和label的关系。一次在做自定义单选框的时候又重新捡起来这对“兄弟”。
  
  label的for属性和input的id值一致的话，input和label就会组成一个组。例如：
  
  <label for="test">
      <input type="checkbox" id="test" abc="1111" />
  </label>
  点击label的区域同样会触发到input的选中效果。利用这一特性，然后结合伪元素可以自定义单选框和单选按钮。
  */
  ```

  

## 细节

+ form表单的用法

  ```html	
  <!-- form标签下有 action method 两个主要属性 action为跳转的页面,method为 发送请求的类型 -->
  <form action="http://localhost:9000" method="post">
      姓名: <input type="text" name="name" id="name">
      年龄: <input type="text" name="age" id="age">
      <input type="submit">
  </form>
  ```

  

  

把所有的盒子都封装在一个大盒子里

开启blc块方法，overflow：hidden && position: absolute可以开启，此时这个盒子可以设置宽高等块级运算拥有的属性

## 1.0 html

+ html由`<html></html> <head></head><body></body>`组成 ` <br/>`为换行符

### 1.1 文本标签&盒子标签

+ `<h1></h1>....<h6></h6> 为`标题标签 
+ `<p></p> `为段落标签
+ `<strong></strong>或<b></b>为加粗标签`
+ `<em></em>或<i></i>`为倾斜标签
+ `<del></del>或<s></s>`为删除线标签
+ `<ims></ims>或<u></u>`为下划线标签
+ `<div></div>`独占一行 `<span></span>`一行显示多个

### 1.2 图像标签

+ 图片格式有:jpg,gif,png,webp等,webp聚集了前三类的优点，小且清晰

+ `<img src="图片地址 ...">` 图像后可附加:

  ```python
  #alt='' 给无法显示的图片添加备注
  #title='' 鼠标放到图片上时显示标题
  #width=  设置图像宽度
  #height= 设置图像高度
  #border= 设置边框
  ```

### 1.3 超链接

+ `<a></a>`语法格式:`<a href="超链接" target="跳转方式"></a>`

  ```python
  #跳转方式有 _self 默认窗口 _blank 新窗口
  ```

+ 下载链接:`<a href='压缩包'></a>`
+ 锚点链接: `<a href="id号"></a> <h1 id = 1><h1/>`,如果href填入h1里的id号则会自动跳转到h1标签位置



### 1.4 注释标签

+ <!---文字--->或者 快捷键ctrl+/ 
+ 特殊字符：空格符 `&nbsp;`  大于号 `&gt;` 小于号`&lt;` 
+ `<br>` 换行，`<hr>`添加分割线,&copy添加版权符号，
+ &#(10进制编码)，可以条用Unicode编码里的标识符

### 1.5 表格标签

+ 用于显示数据，可自动形成表格

  ```html
  <table> <!--定义表格-->
      <tr> <!--定义行-->
          <td>...<td> <!--定义格-->
      </tr>
  </table>
  ```


<img src="C:\Users\zbcz\Desktop\web\images\table.png" style="zoom:100%;" />

+ `<th></th>`表头标签可以让此行标签粗体显示

+ `<thead></thead>表格头部 <tbody></tbody>表格主题`

+ 表格属性 aligh center 居中，border加边框，cellpadding改变文字和边框距离，cellspacing 单元格和单元格距离 width 宽度    height 高度

+ 合并单元格

  跨行合并:rowspan 跨列合并colspan



### 1.6 列表标签

+ 无序列表

  `<ul><li></li></ul> `

  + 

+ 有序列表

  `<ol><li></li></ol>`

   1.

+ 自定义列表

  ```html
  <dl>
      <dt></dt>
      <dd></dd>
  </dl>
  ```



### 1.7 表单标签

+ label标签，可以使自己成为表单的一部分，当点击label里的内容时，自动跳转到对应input框 

  ```html
  <label for="demo">账号：</label><!--当点电子账号时，光标会快速调转到demo框-->
  <input type="text" id="demo">
  ```

+ radio单选框

  ```html
  <!--想要实现当选，需要把他们的name值都设为相同项，这样同一种状态下，指挥选择一个标签-->
  男<input type="radio" name="gender">
  女<input type="radio" name="gender">
  ```

  

+ 表单由表单域，表单控件，提示信息3部分构成

  `<form></form>`表单域标签

  ```html
  <form action = "url地址" method="提交方式(post,get)" name="表单名">
      
  </form>
  ```

+ 表单控件

  `<input type=""属性值/>` 

  type为text为文本，radio为单选按钮

  checkbox为复选框   配合checked使用

  password 为密码框 

  maxlength 限制输入字符

  submit 提交按钮

  reset 定义重置按钮

  button 按钮

  file上传

  -------------

  控件
  
  + ​    value 输入框内容
  + checked 复选框选择
  + name和value给后台看
  + placeholder 首次加载表单，显示提示字符
  + disabled 禁止点击



### 1.8 lable标签

+ 可以和input嵌套使用，用于绑定表单元素当点击lable标签所注释得文本时，浏览器会自动将焦点转到元素

+ `<select></select>`表单元素 下拉标签配合option使用

  ```html
  <select>
      <option>12</option>
  </select>
  ```



### 1.9 文本域

+ `<textarea></textarea>`

  ```html
  <textarea nows="3字" cols="20行"></textarea>
  ```

  

### 1.10 iframe标签

+ iframe标签可以添加小分页框

### 2.11 mate标签

+ 用来设置网页中元素数据，内容不被用户查看，一般只被浏览器解析网页，告诉引擎页面内容

+ 可选属性: name.要设置属性的名字  ，content,要设置属性的内容

  ```html
  <!--description类名用于描述页面内容，在搜索引擎中会以小字进行显示-->
  <meta name="description" content="这是我的网站">
  <!--keywords类名，当在浏览器搜用框输入内容时，如输入的内容和你设置的关键字一样，则此页面会会推送给用户，(以访问数排名)-->
  <meta name="keywords" content="C,python,html,css,javascript">
  ```

+ http-equiv="refresh" ,content="3;url=https://www.daibu.com"，可以对页面进行从定向，多用于404页面，可以选择跳转时间，这里设置的是3秒

  ```html
  <!--重定向网页-->
  <meta http-equiv="refresh" content="3;url=https://www.daibu.com">
  ```



### 2.12 语义化标签

H5新增元素，有兼容性问题，一般多用div来代替

和div作用相同,主要帮助爬虫抓取数据，有header,main,footer等都是块元素，

+ header,主要用于存放logo,搜索框等网页头部标签

+ main,表示主题内容

+ footer,标签表示网页的底部

+ nav,页面导航栏

+ article,表示页面的文章

+ hgroup,用于分类段落标签

+ aside,用来解释一篇文章

+ section,表示一块独立的区域

  ```html
  <header>123</header>
  <main>3</main>
  <footer>2</footer>
  <nav>3</nav>
  <article></article>
  <hgroup></hgroup>
  <section></section>
  ```



### 2.13 音频(视频)标签

+ audio(source,embed(用于兼容老浏览器(ie8)))，src引入路径，引入后没有播放组件，需要自己设置，

+ video,组件和audio一致，可以通过复制网页的

+ 常见的播放属性：

  control，设置是否允许用户控制音频，如果允许则设置，写成control,control=control都行

  autoplay,自动播放,已经被大多浏览器禁止播放，老版本浏览器没有禁用，有的也不支持，

```html
<!--写法-->
<audio controls autoplay src="路径"></audio>

<!--解决兼容问题-->
<audio controls>
    “你的浏览器不支持播放音频”
    <source scr=".mp3"></source>
	<source scr=".ogg"></source>
</audio>
<video></video>
```



## 1.1 CSS

css类思想：给单个标签设置单独类，给单个类设置单独功能样式，其他标签引用这个类获得此样式(多用)

### 1.1.1 选择器

分为，标签选择器，类选择器，id选择器，通配符选择器

+ 标签选择器

  ```css
  p {
  	color:purple;
  }
  ```

+ 类选择器

  ```css
  .lab{
      height: 500px;
  }
  ```

+ id选择器

  ```css
  #id {
      width:300px;
  }
  ```

+ 通配符选择器,能选取页面中所有的标签

  ```css
  * {
      margin: 50px;
  }
  /*
  *{
   margin: 0;
  padding: 0;
  } 可以快速初始化页面，可是这样会造成空间浪费，所以在练习中可以使用*{}方式初始化，在工作中，需要一个手写一个用到什么写什么，也可以去找一些初始化代码
  */
  ```

+ 属性选择器,选择相同属性的标签

  ```css
  /*选择div盒子中有title属性的标签*/
  div[title]{
      color: pink;
  }
  /*选择全局title为hello的标签*/
  *[title="hello"]{
      color:pink;
  }
  /*选择以h开头的标签,可以为正则表达式$^*/
  div[title^="h"]{
      color:pink;
  }
  ```

  

### 1.1.2 字体属性

选择输入的字体属性

+ 选择字体

  ```css
  .a {
      font-family:"microsoft YaHe";
      /*serif 更多字体可百度*/
  }
  ```

+ 字体大小

  ```css
  .a{
      font-size:18px;
  }
  ```

+ 字体粗细,右normal(400),和bold(700)选项

  ```css
  .a{
      font-weight: bold; 
  }
  ```

+ 文字样式, normal 默认值，italic 斜体

  ```css
  .a{
      font-style: normal;
  }
  ```

+ 复合写法

  ```css
  /*font: font所有属性的复合写法，后两个必须是 字体大小和 字体类型,其余顺序随意，font:[加粗 斜体 变形] 大小/行高 字体族；
  如： font: bold 40px 'Microsoft Yahei'*/
  .a {
      font: (font-style) (font-weight) (font-size) (font-family);
  }
  ```
  
  ```python
  # font-variant：small-caps; 字体变形
  ```
  
  

### 1.1.3 文本属性

可以定义文本外观

+ 定义文本颜色

  ```css
  .a {
      color:pink;
  }
  ```

+ 对齐文本,center句中对齐，left左对齐，right右对齐，justify 两端对齐

  ```css
  .a {
      text-aligh: center;
  }
  ```

+ 装饰文本，可以给文本添加，删除下划线,none没有装饰，underline 下划线，overline上划线mline-through 删除线，

  ```css
  /*还可复合设置： text-docoration: underline dotted red;*/
  .a {
      text-decoration: none;
  }
  ```

+ 更改超链接样式

  ```css
  .a {
      text-decoration:none;
      color: #333;
  }
  ```

### 1.1.4 文本缩进

+ 文本缩进，给标签缩进,2em,em为单位大小

  ```css
  /*再图片文字或官网文字中，一般都是以图片的形式显示，或想隐藏文字可以用到 font-sezi: 0px;或 text-indent:-9999px;做一个伪隐藏*/
  .a {
      text-indent: 10px;
  }
  ```

+ 行间距设定，让文字垂直句中

  ```css
  /*1.行高可以把内容撑开，如父盒子没有指定高度，给子元素指定行高的话，父盒子会撑大，子元素会居中
  2.文字并不是贴着行底部，而是沿着基线(basic line)*/
  .a{
      line-height:26px;
  }
  ```

+ ```python
  #1. text-overflow: 如何处理往一处的文本，ellipsis 省略显示
  # text-overflow: ellipasis;
  
  #2. letter-spacing 设置字之间的间距 letter-specing: 1px;等
  
  #3. word-specing 设置单词之间的距离
  ```

  

### 1.1.5 CSS引入方式

有三种:行内样式表，内部样式，外部样式

```html
<!--外部样式-->
<link rel="style sheet" href"css路径">
<!--style-->
<style></style>
<!--行内样式-->
<div style="height: 300px"></div> 
```



### 1.1.6 Emmet语法

+ 快速生成代码，写出标签后按**TAB**
+ 想快速生成多个标签，div*10，按TAB生成10个div标签
+ 如有父子级关系标签可用**ul>li**
+ 如有兄弟标签可用**div+p**
+ 如要生成类名或id名，可用.demo(类)，#tar(id)，这种方法定义的标签默认为div标签，如要给其他标签声明类或id可**标签.(#)name**即可
+ 如需定义顺序，可在变量后加$,如**p.demo$*5**,创建5个demo类标p标签，类名带编号



### 1.1.7 复合选择器

包括，后代选择器，子选择器，并集选择器，伪类选择器

+ 后代选择器,ol li 之间必须有空格

  ```css
  
  ol li{
      color:pink;
  }
  ```

+ 子选择器,选择标签里的下一级子元素

  ```css
  div>p{
      color:pink;
  }
  ```

+ 并集选择器,可以同时选择多个标签统一更改样式

  ```css
  div,p,h1 {
      height: 10px;
  }
  ```

+  兄弟选择器

  ```css
  /*选中的为inner标签，和div为兄弟关系*/
  div + inner {
      display: none;
      
  }
  ```

  

+ 伪类选择器,用来给标签添加效果

  ```python
  # lab:link 选择所有没有被访问的链接
  # lab:visited 选择被访问的链接，只能修改颜色
  # lab:hover 鼠标移动到改元素上会产生的效果
  # lab:active 点击时触发
  # lab:focus 用于选取获得焦点的表单元素
  # lab:first-child 选择标签中第一个元素，如果第一个标签与你声明的标签不符，则不会发生变化
  # lab:first-of-type 选择同类型元素中排列，选择第一个元素
  # lab:last-child 选择标签中最后一个元素，如果最后标签与你声明的标签不符，则不会发生变化
  # lab:last-of-type 选择同类型的最后一个
  # lab:nth-child(表达式) 选择元素，可以可以添加表达式(odd(单),even(双))，表达式从1开始
  # lab:only-child 选择唯一的一个子元素
  # lab:only-of-type 同类型中唯一的一个子元素
  # lab:empty 选择一个内容为空的标签
  # lab:not 否定伪类，表示除了 p:not(.p1){...}除了.p1所有p都执行此样式
  
  #给.a标签添加hover，当移动到a上时， .inner执行下面的代码
  #.a:hover .inner{
  #            display: block;
  #        }
  ```

### 1.1.8 元素显示模块

文档流，指在页面中的元素，高度被内容撑开。浏览器在加载时会有一套默认样式，会放大margin,padding等，这时就要清除默认内外边距。在开发时一定要清除默认样式，可在网上查找写好的默认样式去除方法

常见的块元素有:

```python
# <h1>~<h6> <p>,<div>,<ul>,<ol>,<li>
#块元素特点:独占一行，高度，距离，宽度可控，可以存放行内和块元素
```

常见的行内元素有:

```python
# <a>,<strong>,<b>,<em>,<i>,<del>,<s>,<ins>,<u>,<span>,<span>是最经典的行内元素
# 行内元素的特点：一行可以显示多个，无法设置宽高，只能容纳文本或其他行内元素
```

行内块元素:

```python
# <img>,<input>,<td>他们同时有块元素和行内元素特点
#  行内块元素特点: 一行上可以有多个行内块元素，高度宽度可变
```



元素显示转换：

```python
 # 转换为块元素代码&&显示元素 display: block;
 # 转换为行内元素代码 display: inline;
 # 转换为行内块元素代码 display: inline-block;
 # 隐藏元素 display: none;
 # 表格显示 display: table;
 # 弹性容器显示 display: flex;
 # 行内党性容器 display: inline-flex;
```

单行字垂直句中，设置文字行高和盒子高度相等即可



### 1.1.9 background

用来填充背景颜色，和添加背景图片

+ 添加背景颜色

  ```css
  .a{
      background-color:
  }
  ```

+ 添加背景图片

  ```css
  /*设置背景图片时，如果图片小于元素，则图片默认会从元素的左上角开始平铺，如果图片大于元素，则多余部分不会显示*/
  /*浏览器再首次加载时，一些隐藏(未触发)的资源(图片等)不会立刻响应请求，而是再你触发了这个行为时才会把隐藏的资源相应给你，所以再你首词加载时可能出现，盒子的空挡，那是响应机制问题 , 解决方法，把所有图片放到一个图里，所以用到了雪碧图*/
  .a {
      background-image: url(图片位置);
  }
  ```

+ 背景平铺，有repeat(平铺)，no-repeat(不平铺)，repeat-x(x轴平铺),repeat-y(y)

  ```css
  .a{
      background-repeat:repeat;
  }
  ```

+ 背景图片位置,如果参数是方位名词两个值前后顺序不影响，如果第一个值指方位，另一个值可以忽略。如果参数是精确坐标第一个一定是x，第二个是y

  ```css
  /*1.用来设置背景图片的位置，设置方法
  通过位置关键字来设置， 有 top left bottom right center,可以写两个值，如果写一个第二个值默认为center
  2.使用偏移量来设置背景图片位置  ，第一个值，水平方向，第二个值 垂直方向*/
  .a{
      background-position: x y;
  }
  ```

+ 背景图片固定(背景附着),scroll(滚动)，fixed(固定)

  ```css
  .a{
      background-attachment:scroll;
  }
  ```

+ 背景复合写法

  ```css
  .a{
      background:背景颜色，背景图片位置，背景平铺，背景图片滚动，背景位置
  }
  ```

+ 背景半透明

  ```css
  .a{
      background:rgba(0,0,0,.3);
  }
  ```

+ ```python
  # background-clip 可以设置背景显示区域，
  # 可选值： border-box 背景会延伸到边框  。padding-box 背景会设置到内边距，content-box 背景会设置到内容区
  # background-clip : border-box;
  ```

+ ```python
  # background-repeat 可以设置背景排列方式
  # 可选属性 repeat 默认值 双向重复
  # repeat-x 水平方向平铺
  # repeat-y 垂直方向平铺
  # no-repeat 不平铺
  ```

+ ```python
  # background-size 可以设置背景图片的尺寸
  # 需要两个值作为参数，宽度，高度
  # background-size: auto 100%;  cover (等比例缩放)
  # 可选值 contain 完整显示背景图片，可能会有位置没有图片
  # cover 使图片将元素填满，可能有部分图片从元素中溢出
  ```

+ 精灵(雪碧)图操作

  ```python
  # 再html中 x轴右为正，左为负，y轴下为正，上为负
  # 雪碧图使用方法
  # 1.创建一个元素 然后根据图里需要的小图片 设置 background-position: -x -y;(之所以要负数是因为要把这个图片往上和左移动，正数的话是往右和下移动，不符合要求)，
  ```

  

### 1.1.10 CSS三大特性

层叠性，继承性，优先级

+ 层叠性

  相同选择器基于相同样式，后面会覆盖前面的样式(程序从上往下执行)

+ 继承性

  子标签会继承父标签的样式，背景相关，width,border等样式不会继承

+ 优先级

  选择器相同则层叠执行，不同则根据选择器权重执行(权重可以叠加)

  !important>行内样式>ID选择器>伪类选择器>元素选择器>继承

  ```css
  .a{
      color: pink;!important
  }
  ```

  

### 1.1.11 盒子模型

盒子模型组成，border边框，content内容,padding 内边距,margin 外边距

+ 边框(可设置宽度，样式，颜色),边框会影响盒子的实际大小

  ```python
  # 边框 border-style 有 solid(实线) dashed(虚线) dotted(点线) double 双实线
  # border: border-width| border-style|border-color;
  
  #表格的细线边框
  # border-collapse
  # border-right-color 等可以设置一边的属性
  ```

+ 内边距

  padding设置内边距，即边框和内容的距离，内边距会撑大盒子

  ```python
  # padding设置内边距，即边框和内容的距离
  # 有 padding-left 左边距 padding-right 右边距 padding-top 上边距 padding-bottom 下边距
  
  #内边距的简写 如4边均不同 顺序为 上右下左 padding: 1px 2px 3px 4px;
  # 如果是 如2边据不同 顺序为 上下 左右 padding: 1px 3px;
  # 如4边都相同 为 padding: 1px;
  ```

+ 外边距

  ```python
  # margin设置外边距 
  # 有 margin-left 左边距 margin-right 右边距 margin-top 上边距 margin-bottom 下边距
  
  #外边距的简写 如4边均不同 顺序为 上右下左 padding: 1px 2px 3px 4px;
  # 如果是 如2边据不同 顺序为 上下 左右 padding: 1px 3px;
  # 如4边都相同 为 padding: 1px;
  
  # 外边距可以让盒子水平居中
  # 必须满足两个条件，指定宽度，盒子左右外边距设置为auto，以父标签为参照点水平居中，父元素指他的外面一层元素
  
  # 内盒子外边距与父盒子合并
  # 嵌套塌陷解决办法，为父级元素定义上边框，为父级元素定义内边距，为父级元素添加 overflow:hidden ， 最完美的解决方法是给父元素添加::before{display:table;} 这样可以解决问题
  
  #margin: 0 auto;只有当元素设置宽高后才能设置居中
  ```

+ 清除全局内外边距

  ```css
  *{
      padding:0;
      margin:0;
  }
  
  ```

  行内元素设置左右内外边距，不设置上下内外边距，转换为行内元素后需要才需要设置

+ 圆角边框

  ```python
  # border-top-left-radius:10px;
  # border-top-right-radius:10px;
  # border-bottom-left-radius:10px;
  # border-bottom-right-radius:10px;
  # border-radius: 50px;
  # 想得到一个圆角时，width和height需相等,border-radius是它们的一半(50%)
  # 想得到圆角矩形时，width要大于height，且border-radius genius
  # border-radius是一个简写属性也跟四个值，顺序为上右下左，和内外边距写法一致
  ```

+ 盒子阴影

  ```python
  # box-shadow: h-shadow v-shadow;
  # h-shadow 水平阴影位置 ； v-shadow 垂直阴影位置
  # box-shadow: 2px(x轴阴影) 3px(y阴影) 4px(阴影清晰度) rgba(0,0,0,.3)(透明度)
  # blur 模糊距离；spreed 阴影尺寸;color阴影颜色;inset 内部阴影
  
  #第一个参数设置内部阴影
  #box-shadow:inset 0px 0px 20px rgba(0,0,0,.3);
  
  #阴影可以设置多重阴影
  #.nav_little:hover{
  #    box-shadow: 2px 2px 30px rgba(0,0,0,.3),
  #     -2px -2px -30px rgba(0,0,0,.3);
  #   }
  ```

+ 文字阴影

  ```python
  # text-shadow: h-shadow,v-shadow blur color;
  ```



### 1.1.12 浮动

+ 使用float 来设置元素浮动，可以向左和向右浮动，设置浮动以后，元素会脱离文档流，所以下边的文档流元素会自动上移。当同时脱离文档流时，元素会并行排列。如给行类元素设置浮动，则此元素可以设置宽高
+ 浮动元素不会超过其上没有浮动的块元素，且如果宽度允许，浮动元素只会往上移动一个盒子元素，不会置顶显示
+ 文字不会被浮动元素所覆盖，而是围绕在图片四周
+ 块元素高度默认情况是被子元素撑开，当子元素浮动时，高度会出现一个塌陷问题，导致其他元素错位，

```python
# float: left;  向右移
# float: right; 向左移
```

+ 高度塌陷:给一个子标签添加浮动后，如父元素(一般不设置高度)没有设置高度就会出现塌陷

  解决办法：设置BFC块，设置后子标签外边距不影响父标签，且不会被浮动标签覆盖，还可以包含浮动的子标签。

  设置BFC块的方法使用overflow:hidden来开启元素BFC块，谁会塌陷就给谁加

  设置clear:left(左边)&right(右边)&both(清除影响大的);来清除浮动元素造成的影响，使其他没有浮动的盒子不会占据浮动盒子的位置。(clear原理:添加clear属性的元素，浏览器会自动给它添加一个外边距)

  ```css
  /*写法直接复制即可，写需要清除给谁的父盒子*/
  .clearfix::before,
  .clearfix::after{
      content:"";
      display:table;
      clear: both;
  }
  ```

  



### 1.1.13 伪类选择器

标签前面和后面各存在一个元素，可以通过::before,::after来控制

```css
/*before更改前面的样式，和内容*/
lab::before{
    content: "hello";
    color:pink;
}
/*after更改前面的样式，和内容*/
lab::after{
    content: "hello";
    color:pink;
}
/*选择提一个字符*/
lab::first-letter{
    content: "hello";
    color:pink;
}
/*选择第一行*/
lab::first-line{
    background: pink;
}
/*选择鼠标选中的字符*/
lab::selection{
    background: pink;
}
```

### 1.1.14 长度概念

+ px(像素)，分辨率:1396(px)x768(px),。有css像素，和物理像素，在不同分辨率下px像素显示不同，但系统会默认缩放实现最佳效果，只有在手机端才会受影响所以有百分比布局等等

+ 百分比，相对与其父级(包含块)长度进行缩放，元素的父级(包含块)是body，

  ```css
  .box {
      width: 100px;
      height: 200px;
  }
  .box .w{
      width:30%;
      height:50%;
  }
  ```

  

### 1.1.15 overflow

overflow表示溢出的元素的显示方式，可以有scroll,hidden 等

```python
# overflow: scroll 溢出部分添加滚动条
# overflow: hidden 溢出部分隐藏
# overflow: visible 默认值，溢出部分可见
# overflow: auto 自动适应，常用
```

### 1.1.16 visibility

可以设置元素的显示状态，隐藏依然占据位置

```python
# visibility 可以设置元素的显示状态
# visibility: hidden  隐藏 依然占据位置
# display: none 隐藏 不占据位置
# visibility : visible 可见
```



### 1.1.17 outline

轮廓，设置标签的轮廓(类似于边框)，但不会元素的改变位置 

```python
# outline: 1px red solid;
```



### 1.1.18 定位

+ 通过定位可以将元素放到页面中的任意位置

  ```python
  # position 属性值设置元素的定位
  # 可选值 static(静态) relative(相对) absolute(绝对) fixed(固定)
  ##开启定位后，可以通过 top bottom left right 设置定位方向
  # 当给定位元素设置 0 0 使它会相对于他的包含块居中
  
  #相对定位 relative
  #开启相对定位，元素不会发生任何变化，不会脱离文档流
  #相对定位的元素，是相对于其再文档流中的位置进行定位，即以原来的位置为参照点，相对定位的元素会保留原来的位置
  #相对元素会使元素提高一个层级
  # 定位元素层级还可以通过 z-index来设置 ，z-index:3;
  # 父元素层级再高也会盖住子元素
  # opacity 用来设置元素的不透明度 ，范围再0-1之间，opacity: 0.4; ,图片也可以设置透明
  # 给一个相对定位的盒子开启内外边距，会影响以该盒子为参照点的，绝对定位和子，绝对定位和子也会继承这个内外边距，所以使用时要减去内外边距
  
  #绝对定位 absolute
  #1.绝对定位会让元素完全脱离文档流
  #2.绝对定位会改变元素的性质，行内变行内块
  #3.开启绝对定位后如果不设置top等值位置不变
  #4.绝对定位会相对于离它最近的开启了定位的祖先元素进行定位，如果所有祖先元素没有开启定位，则相对于HTML标签开启定位 。 所以一般情况，为一个元素开启了绝对定位，会同时为它的父元素开启相对定位，绝对定位元素会相对于它的包含块进行定位 。HTML就是包含块
  #5. 却对定位会使元素提升一个层级
  
  #固定定位 fixed
  #1.大部分特点都和绝对定位类似
  #2.固定定位总是相对于浏览器的视口(窗口)进行定位
  
  #黏滞定位 sticky
  #1.当移动到添加了粘滞定位的元素时，这个元素会按照你给的粘滞设置固定到页面上
  
  #可以使元素动态垂直居中
  #{   position: absolute;
  #        left:0px;
  #        top: 0px;
  #        bottom: 0px;
  #        right:0px;
  #        margin:auto;
  #           }
  
  # 颜色#fff(红绿蓝)，每一位代表此内的颜色占比
  ```
  
+ 居中方法

  ```css
  /*
   1.
    {
  	position: absolute;
  	top:50%;
  	left:50%;
  	transform:translate(-50%,-50%);
  }
   2.
  	{
  	position:absolute;
  	top:0;
  	left:0;
  	right:0;
  	bottom:0;
  	margin: auto;
  
   3.
  	position: absolute;
  	left:50%;
  	top:50%;
  	margin-left: -50%;
  	margin-top: 50%;
  }
  */
  ```

  

  

### 1.1.19 图标字体

```python
#使用步骤
#1.将css和font文件夹放到项目目录下，引入css 使用阿里巴巴矢量图，把iconfont引入到html页面
#2. 定义样式 @font-face {
#    font-family: "iconfont logo";
#    src: url('https://at.alicdn.com/t/font_985780_km7mi63cihi.eot?t=1545807318834');
#    src: url('https://at.alicdn.com/t/font_985780_km7mi63cihi.eot?t=1545807318834#iefix') format('embedded-opentype'),
#    url('https://at.alicdn.com/t/font_985780_km7mi63cihi.woff?t=1545807318834') format('woff'),
#    url('https://at.alicdn.com/t/font_985780_km7mi63cihi.ttf?t=1545807318834') format('truetype'),
#    url('https://at.alicdn.com/t/font_985780_km7mi63cihi.svg?t=1545807318834#iconfont') format('svg');
 #   } 和 .iconfont{
 #       font-family:"iconfont" !important;
 #       font-size:16px;font-style:normal;
 #       -webkit-font-smoothing: antialiased;
#        -webkit-text-stroke-width: 0.2px;
#        -moz-osx-font-smoothing: grayscale;
#    }
# 3. 把iconfont引入到需要数用的标签类，然后调用相应代码即可
# 4. 引入代表中时直接复制即可 如 &#xe60a;  ，但在样式中引入时写 \e60a 


# 字体大小的概念
# 1. em相对于字体  rem相对于html长度  px相对于屏幕像素
```

### 1.1.20 vertical

```python
# vertical-align垂直对齐方式  baseline 基线对齐  top 顶部对齐 bottom 底部对齐 super 上标 sub 下标

#当插入图片时，有时图片下方不对齐(因为字体默认基线对齐，会产生缝隙)，可以设置 font-size: 0 或 vertical-align:bottom 让图片底部对齐
```

### 1.1.21 white-space

```python
# 1. 如何处理空白内容  ，normal 默认值，自动换行 ，nowrap 不换行，pre 保留文本格式，
```

### 1.1.22 表格(table)

+ 表格,(thead总会再最前面显示，tbody总会再中间显示，tfoot总会再末尾显示)

```python
# 表格用来表示一些格式化的数据，再网页中，使用table来创建
# 用<table><tr><td></td></tr></table> 表示，tr表示行，td表示列
# <th>表示表头的单元格
# 合并单元格
# colspan=2 横向合并单元格，表示2列 ，默认向右
# rowspan=2 纵向合并单元格，表示2行，默认向下
# thead 表示表格头
# tbody 表示表格体
# tfoot 表示表格底部
# border-spacing: 0px 和 border-collapse:collapse 都可以合并单元格，而border-spacing是把两个边框进行重叠，border-collapse是把边框变为1px

#创建table表格时，如果不写tbody,tfoot浏览器会自动进行添加
```

+ ```js
  /*
   标准表格格式
   <table>
      <thead>
      <tr>
          <th>序号</th>
          <th>内容一</th>
          <th>内容二</th>
      </tr>
      </thead>
      <tbody>
      <tr>
          <td><input type="checkbox"></td>
          <td>123</td>
          <td>456</td>
      </tr>
      <tr>
          <td><input type="checkbox"></td>
          <td>123</td>
          <td>456</td>
      </tr>
      <tr>
          <td><input type="checkbox"></td>
          <td>123</td>
          <td>456</td>
      </tr>
      </tbody>
  </table>
  */
  ```
  
+ 表单

  ```python
  # form 用来将信息提交给服务器
  # 使用form标签来创建表单,
  #如果希望服务器可以正常接收表单数据，必须要给表单项设置一个name属性
  
  #提交按钮 input 
  ```

+ input

  ```css
  /*
  input为表单
  `<input type=""属性值/>` 
  
  type为text为文本
  
  radio为单选按钮  单选按钮通过name属性进行分组，同组内只有一个单选按钮被选中，需要有value值作为一个参数的传递
  
  checkbox为复选框 需要name属性进行分组，同时也需要value值用于传递信息
  
  password 为密码框 
  
  name和value给后台看
  
  
  value 可修改内容
  
  submit 提交按钮
  
  reset 定义重置按钮  可以重置所有选项到初始状态
  
  button 按钮  
  
  
  
  file上传
  
  ------------
  有兼容问题
  color 选择颜色
  
  email 邮箱输入框 再pc端会提示输入不合法
  
  date 日期输入框  
  
  参数可选值： required 必填项  autofoces 自动获得焦点 checked 使元素首次加载被选中  可以使单选按钮默认选中
  
  maxlength 限制输入字符  readonly 设置为只读属性  disabled 使表单不可用
   
  placeholder 阴影显示，鼠标点击时显示消失 
   autocomplete="off"自动填写关闭 "on" 开启
  
  
  */
  ```

  

+ ```css
  /*
  	1. outline 设置轮廓
  
  	2. cursor default(光标) pointer(小手) wait(加载)等 设置鼠标样式
  	
  */
  ```

+ 下拉列表

  ```css
  /*
  	使用select 来创建一个下拉列表，再select中使用option来设置选项
  	同时，也需要name分组 和value值来做一个返回值 name加给select ,value给option里
  	可以加 multiple 属性 多选
  	再option中可添加selected属性将当前选项设置为默认选项
  	
  
  
  	button标签
  	成对出现  有 type ,submit,reset,button 属性 
  	<button type="submit"></button>
      <button type="reset"></button>
      <button type="button"></button>
  */
  ```

  

### 1.1.23 设置网页小图标

```css
/*
设置网页图标
1.制作ico图标(在线制作)
2.将标签放到项目的根目录下，并命名为favicon.ico
3.再网页中link标签进行引入
<link rel="icon" href="./favicon.icon">
*/
```

### 1.1.24 压缩css

```css
/*
 百度可查
*/
```



### 1.1.25 过渡

可以使一个属性的值经过一段时间后再变成某种状态

```css
/*
1.transition-property 要进行过渡的属性，可以让多个属性进行过渡，用逗号隔开 ，可直接写all表示所有属性过渡
 transition-property:width,height||transition-property:all

2.transition-duration 要进行过渡的时间，s秒 ms 1s=1000ms。可以与
transition-property属性一一对应，设置时间、

3.transition-delay 延时开始动作时间

4.transition-timing-function 设置过渡时间函数 
参数有： ease 起步慢，然后加速 linear 匀速运动 ease-in 加速运动       ease-out 减速运动   stemp(6,end) end比start快 stemp(6m,start)

5.transition 上面属性的复合写法，无先顺序 谁需要过渡给加

6. calc()函数帮助计算 calc(-623px/4*3)

7. 过渡设置的transition-timing-funcation 最终都会转换为bezer曲线   使用cubic-bezier(.17,.46,.59,.67) 前两个值改变起始点的速度，后两个值改变终点速度
*/
```

### 1.1.26 动画(animation)

```css
/*
通过动画可以实现更复杂的交互效果，要实现css动画，必须设置关键帧 ，可以自己触发，不需要执行操作

基本控件：@keyframes 名字{from{设置开始位置} to{结束位置}} .要再需要执行动画的元素上，设置动画细节
1.animation-name: test;(动画名)
2.anation-duration:2s;(动画持续时间)
3.animation-timing-function: steps(3,end)(动画时间(状态))ease 起步慢，然后加速 linear 匀速运动 ease-in 加速运动       ease-out 减速运动   stemp(6,end) end比start快 stemp(6m,start)
4.animation-iteration-count: infinite;(动画执行次数) infinite(无线) 
5.animation-delay: 3s;(动画延时开始时间)
6.animation-play-state:;(动画播放状态) paused(暂停) running(开始（默认）)
7.animation-direction:;(动画运行方向)                              可选值 normal(默认),0%-100%,reverse(反转,100-0)，alternate(0-100,100-0),alternate-reverse(0-100,100-0)
8.animation(复合写法):test 400ms steps(6) normal;

@keyframes test {
from{
	background-position: 0 0;
}
to{
	background-position:calc(-528px/4*3) 0;
}
}
*/
```

### 1.1.27 变形，旋转

变形和旋转复合写时，前后顺序有讲究，前一个动作绝对后一个动作

```css
/*
变形
transform 变形，通过变形可以对元素进行平移，放大，缩小，拉伸等.不会影响页面布局，和相对定位类似
translateX()沿x轴方向平移 translateY()沿着y轴方向平移 translateZ（）沿Z轴方向平移
1.transform:translateX()可以配合position进行无限制长宽的盒子居中box {position:absolute;left:50%;top:50%;transform:translateX(-50%)  translateY(-50%)}水平垂直居中,
可以简写transform:translate(-50%,-50%),translateZ()要单独写
2.transform:translateZ()相对于正面平移，需要配合视距使用，视距:用于设置人眼和网页的距离 设置方式:html{perspective: 800px;}常设置800-1000

旋转
1.transform: rotateX(45deg) translateZ(100px); 沿着x轴旋转，正数向前转，负数向后转
2.transform: rotateY(45deg);沿着y轴旋转，正数向右转，负数向左转
3.transform: rotateZ(94deg);沿着中心转
4.如果需要设置3D效果，需要设置 transform-style:preserve-3d;谁需要开3d给谁开
5.transform-origin: top right;可以设置旋转方向，搭配rotateZ使用

缩放
1.transform: scaleX(2) ,沿X轴放大
2.transform: scaleY(1.5),沿Y轴放大 
3.transform: scale(2,2) x和y的复合放大写法 第一个为x，第二个为y
4.transform: scaleZ(1.2) 沿z轴放大，仅对三维图像有效
*/
```

### 1.1.28 opacity

```css
/*
	opacity 用来设置元素的不透明度 ，范围再0-1之间，opacity: 0.4; ,图片也可以设置透明
*/
```

### 1.1.29 backface-visibility

```css
/*
用来设置背面的图片是否可见 有 backface-visibility:hidden (visit);
*/
```

### 1.1.30 项目注意点

```css
/*
1.主页目录一般用index.html,放在根目录下，其他页面可以放到source文件夹里

2.z-index一般是设置正数高层级

3.img一般常用为相当路径引入。也可以开启一个CDN服务器，把所有资源放到这个服务器中，再用绝对路径引入，绝对路径中协议名称可以省略，系统会根据你打开的方式隐式添加合适协议。如果手动添加协议则只有此协议认可的方法才能打开

4.出现了一个大宽度的图片时，多半是为了试应多个屏幕的显示器而存在，通过让符合宽度等于 100%,然后子盒子图片设置相对定位对其left:0top:0居中图片，然后父盒子设置overflow:hidden来剪切多余部分

5.左侧固定，右侧自试应布局，给左侧盒子外边距让它移动到自试应的左边，然后自试应盒子给左外边距让内容显示


*/
```



### 1.1.31 渐变

```css
/*
1.渐变背景不是颜色，而是图片，我们需要通过background或background-iamge来设置渐变 background-image: linear-gradient(角度，颜色 位置，颜色 位置),渐变方向默认自上而下,颜色占比默认平均分配，可手动调整。             background-image:linear-gradient(to right,red 30px,yellow 50px)   background-image:linear-gradient(red 20%,yellow 40%)             background-image:repeating-linear-gradient(red 30px,yellow 50px)可以使渐变重复分区,根据给定位置的差值来进行分区
值有 linear-gradient()线性渐变 radial-gradient()径向渐变
渐变总的来说也是图片，也可以用background的一些操作来对其进行更改

2. background:radial-gradient(30px 30px,red 30px,yellow 50px)效果为以中心为原点向四处扩散。默认铺满，可以再最前面以px形式设置径向渐变大小

可以加at 30px 30px指定圆心位置
background:radial-gradient(at 30px 30px,red,yellow)
可以加 closest-side ，与最近边相切。farthest-side与最远边相切
background:radial-gradient(closest-side at 30px 30px,red,yellow)
background:radial-gradient(farthest-side at 30px 30px,red,yellow)
可以加closest-corner 显示到离圆心最近的角。farthest-corner显示到离圆心最远的角
background:radial-gradient(closest-corner at 30px 30px,red,yellow)
background:radial-gradient(farthest-corner at 30px 30px,red,yellow)
*/
```



### 1.1.32 less

一门css预处理语言，扩展了css语言，增加了变量，函数等，使css具有可维护性

```css
/*
命令行：命令提示符，DOS窗口，CMD窗口，终端，Terminal,Shell

常用命令：
dir表示当前位置文件目录
cd 文件目录 进入到当前目录
cd .. 回到上级目录
cd . 当前目录
mkdir hello 常见文件到当前目录
rmdir hello 移除当前文件
cd \ 进入根目录

文件搜索流程：
当我们在命令中访问文件时，程序先到当前目录寻找，如果没有找到回去PATH环境路径变量中去寻找，PATH环境分系统环境和用户环境，再哪添加都行，但最好再用户环境中添加
*/
```

```css
/*
less  (使用/法运算时，要加())
less @元素查找，找当前{}最后一个，自己没有就去上层找最外面的一个
写法类：
1.{{}}，括号里的括号为父元素的子类，可以采用嵌套写法，会自动转换为css

2.可以支持+-/* width: 300+100px;

3.$样式名,可以直接引入其他属性的值(再同一个作用于类的)height:$width;

4.使用@来定义变量，语法：@a:10px; 可以将变量的值作为选择器或路径使用,用@{}方式引用，  如：@className:box2; .@{className}{wdith: 10px} ,使用变量时依据就近原则，自己作用域没有的参数会去父级作用域找，还没有则去，全局作用域下找。
@变量还可以声明一个字符，然后再引用时使用@@变量来调用该变量如，@a:3;@b:4;引用时@a:b,@@a(指得就是@b)调用得就是的值,
@设置的变量可用于保留文章，页面主题色

5.Mixins扩展，再less中如果两个属性相同一个属性写好后，可以使用 .属性名();把另一个属性的样式全部引入到此属性中，如.box1{wdith:330px} .box{.box1();}，必须使用类选择器来引用，如果有关系嵌套一并要写上，如 .box1 .box2();

6.extend()扩展(继承)，可以将其他的选择器上的样式，扩展到当前选择器上 ,   语法：.box2:extend(.bo1){font-size: 16px;} 

7.Mixins和extend区别是，Mixins是全部复制，extend是扩展。
在定义mixins时，如果在类名后添加了()不会被引入到css中，将会被设为一个函数 .类名() 方式获取。可以引接收用者传递的参数.box1(@a){width:@a} .box2{.box1(500px);},可以设置多个形参，但实参也需要对应。传递实参时可以顺序传递，也可以指定参数传递
8.&表示外层选择器，可以选择最外层的父级，可以对此父级在内部进行扩展

9.@import 用来导入，可以将外部的less代码导入到当前less中

样式类：
draken()函数用于加深颜色 语法：darken(颜色，百分比)   draken(red,20%)
*/
```



### 1.1.33 弹性盒子

```css
/*
box-sizing可以自动适应边框即内外边距的大小，让其等于你设置的宽高，通过压缩你的内容   语法： box-sizing:border-box;

flex的以下属性都是给弹性盒子添加

弹性盒子css3新增的方式，通过弹性盒可以让一个盒子自动适应其容器大小

弹性容器，就是弹性容器，必须先将元素设置弹性容器,设置弹性容器有两种方式
display: flex(用的多) 块级弹性容器 ；display:inline-flex 行内弹性容器

弹性元素(弹性项，弹性子元素)，弹性容器中的子元素是弹性元素
弹性容器的属性：
1.flex-direction,用来设置弹性容器的主轴方向,主轴纵向辅轴横向，可选值，row自左向右，    row-reverse 自右向左 ,column 主轴纵向(自上向下),column-reverse 自下向上     如：flex-direction: column-reverse；

2.flex-wrap,弹性元素在主轴上是否自动换行，可选值，nowrap 不换行(默认)，wrap 自动换行

3.flex-flow 一个简写属性，可以同时设置 flex-direction和flex-wrap    如 flex-flow: column wrap;

4.justify-content设置在主轴上的对齐方式，可选值，flex-start 起点,flex-end 终点 center 中间  space-between 将空白区域设置到元素右边，相当于给盒子一个右外边距，space-around 将空白取余设置到元素前后    space-evenly 将空白空间设置到元素的一侧 stretch 拉伸让盒子占满盒子

5.align-items 辅轴又叫：垂轴，辅轴，侧轴，在侧轴上设置对齐方式
align-items ，侧轴操作，可选值，flex-start侧轴起边，flex-end 侧轴终边，center 侧轴中间      如：aligh-items: center;

6.align-content 用来设置辅轴上的空白的分布方式
可选值，flex-start 沿着辅轴起边对齐，flex-end 沿着辅轴中变对齐  space-between 沿着元素之前对齐 space-around 围着元素的四周        space-evenly 沿着元素一侧

7.flex-grow 用来指定元素的增长系数 使用剩余空间来按比例分配 ，          如 flex-grow: 1;

8. 当子元素的总宽度超过父元素时，需要使用缩减系数来进行限制,           使用flex-shrink，来进行操作，flex-shrink: 1;
每一个元素缩减多少，由元素的flex-basis 和 flex-shrink共同决定

9.flex-basis,用指定弹性元素的指定大小，这个元素设置了数字则width失效

10.flex,flex-grow,flex-shrink,flex-basis的复合属性，可以指定三个值 ，语法：flex: flex-grow flex-shrink flex-basis;                    可选值，initial :0 1 auto,auto: 1 1 auto,none: 0 0 auto;

11. aligh-self 设置弹性元素自身再辅轴上的对齐方式，用来覆盖元素上的aligh-items属性

12.order 用来指定元素的排列顺序 ，语法order: 数字；
*/
```



### 1.1.34 视口

```css
/*
1.css像素 物理像素
手机屏幕的像素相比显示器的物理像素要小
如果再移动端，依然保持1：1会使手机看的内容非常小，所以再移动端中，都是使用1：2，1：3的方式来进行布局
meta name="viewport" content="width-375px" 以这种方式可以设置视口值，所以content="wdith=device-width,initial-scale=1.0"这就是一个完美视口

2.像素单位
vw表示视口，1vw=1%视口宽度
em表示相对于字体的大小
rem表示一个html的字体大小

3.适配方案
vw适配
用100vw/设计图大小*40,值给html设置字体大小(等于把每一个设计图的1像素分配成了vw像素) html{font-size: (100vw/750)*400}(之所以*40因为font-size要么是0，要么是12~∞)，然后之后测量的物理像素值用rem单位/40可以得到完美视口，其他元素在设置宽高时使用rem来适配 
总结：把设计图的像素转换成了vw完美像素，然后把算出来的像素给html，其他元素使用rem转换成完美像素，又因为font-size只能在0或12~∞，所以html*40达到有效范围，然后其他值在引用rem时/40达到完美像素效果 。 在使用时只需要直到设计图大小然后使用100vw/设计图大小*40，然后每个元素使用rem/40就可以达到完美视口

*/
```

​	

### 1.1.35 网页共享

```css
/*
在同一个局域网内的用户，可以共享网址，localhost为本机访问
当文件以IPv4地址的方式打开时，在同一个网段内的用户可以共享此页面
*/
```



### 1.1.36 媒体查询

```css
/*
响应式布局
随着浏览器窗口随之改变

媒体查询(不写查询条件默认screen)
1.基础语法
可以根据设备窗口大小来实时改变样式
语法 @media 查询条件{
	样式
}

2.查询条件
当查询条件为"all"时，表示把此样式用在所有元素上，当为"print"样式会应用在打印样式上，当为"screen"时样式会用到屏幕上，当为"speech"样式会应用到阅读器上
。查询条件可以写多个值，用逗号(and)连接 如 print,screen 

一般媒体查询都会以only screen开头，only用来解决兼容性问题

3.and , not 
@media screen and (min-width: 800px)来限制查询条件，即在800以上都为真，都执行

and等价于与 | ，(逗号)等价于或 | not 表示取反等价于!

4.orientation
@media screen and (orientation:landscape)可以设置手机横屏或竖屏时的样式，orientation有两个参数 portrait纵向，landscape横向

5.断点
指某个宽度的临界点，跨过这个点布局就会发生显著的变化
超小屏幕768px以下， 小屏幕 大于等于768px，中等屏幕 大于等于992px,大屏幕 大于等于1200 
相应开发，移动端优先，渐进增强
*/
```

![image-20210530192635451](C:\Users\zbcz\AppData\Roaming\Typora\typora-user-images\image-20210530192635451.png)

## 

## 块级元素和行内元素的分类

html中的块级元素：

| 标签         | 描述                                       |
| :----------- | :----------------------------------------- |
| <address>    | 定义地址。                                 |
| <article>    | 定义文章。                                 |
| <aside>      | 定义页面内容之外的内容。                   |
| <audio>      | 定义声音内容。                             |
| <blockquote> | 定义长的引用。                             |
| <canvas>     | 定义图形。                                 |
| <caption>    | 定义表格标题。                             |
| <dd>         | 定义定义列表中项目的描述。                 |
| <div>        | 定义文档中的节。                           |
| <dl>         | 定义定义列表。                             |
| <dt>         | 定义定义列表中的项目。                     |
| <details>    | 定义元素的细节。                           |
| <fieldset>   | 定义围绕表单中元素的边框。                 |
| <figcaption> | 定义 figure 元素的标题。                   |
| <figure>     | 定义媒介内容的分组，以及它们的标题。       |
| <footer>     | 定义 section 或 page 的页脚。              |
| <form>       | 定义供用户输入的 HTML 表单。               |
| <h1> to <h6> | 定义 HTML 标题。                           |
| <header>     | 定义 section 或 page 的页眉。              |
| <hr>         | 定义水平线。                               |
| <legend>     | 定义 fieldset 元素的标题。                 |
| <li>         | 定义列表的项目。                           |
| <menu>       | 定义命令的列表或菜单。                     |
| <meter>      | 定义预定义范围内的度量。                   |
| <nav>        | 定义导航链接。                             |
| <noframes>   | 定义针对不支持框架的用户的替代内容。       |
| <noscript>   | 定义针对不支持客户端脚本的用户的替代内容。 |
| <ol>         | 定义有序列表。                             |
| <output>     | 定义输出的一些类型。                       |
| <p>          | 定义段落。                                 |
| <pre>        | 定义预格式文本。                           |
| <section>    | 定义 section。                             |
| <table>      | 定义表格。                                 |
| <tbody>      | 定义表格中的主体内容。                     |
| <td>         | 定义表格中的单元。                         |
| <tfoot>      | 定义表格中的表注内容（脚注）。             |
| <th>         | 定义表格中的表头单元格。                   |
| <thead>      | 定义表格中的表头内容。                     |
| <time>       | 定义日期/时间。                            |
| <tr>         | 定义表格中的行。                           |
| <ul>         | 定义无序列表。                             |

html中的行内元素：

| 标签       | 描述                       |
| :--------- | :------------------------- |
| <a>        | 定义锚。                   |
| <abbr>     | 定义缩写。                 |
| <acronym>  | 定义只取首字母的缩写。     |
| <b>        | 定义粗体字                 |
| <bdo>      | 定义文字方向。             |
| <big>      | 定义大号文本。             |
| <br>       | 定义简单的折行。           |
| <button>   | 定义按钮 (push button)。   |
| <cite>     | 定义引用(citation)。       |
| <code>     | 定义计算机代码文本。       |
| <command>  | 定义命令按钮。             |
| <dfn>      | 定义定义项目。             |
| <del>      | 定义被删除文本。           |
| <em>       | 定义强调文本。             |
| <embed>    | 定义外部交互内容或插件。   |
| <i>        | 定义斜体字。               |
| <img>      | 定义图像。                 |
| <input>    | 定义输入控件。             |
| <kbd>      | 定义键盘文本。             |
| <label>    | 定义 input 元素的标注。    |
| <map>      | 定义图像映射。             |
| <mark>     | 定义有记号的文本。         |
| <objec>    | 定义内嵌对象。             |
| <progress> | 定义任何类型的任务的进度。 |
| <q>        | 定义短的引用。             |
| <samp>     | 定义计算机代码样本。       |
| <select>   | 定义选择列表（下拉列表）。 |
| <small>    | 定义小号文本。             |
| <span>     | 定义文档中的节。           |
| <strong>   | 定义强调文本。             |
| <sub>      | 定义下标文本。             |
| <sup>      | 定义上标文本。             |
| <textarea> | 定义多行的文本输入控件。   |
| <time>     | 定义日期/时间。            |
| <tt>       | 定义打字机文本。           |
| <var>      | 定义文本的变量部分。       |
| <video>    | 定义视频。                 |
| <wbr>      | 定义可能的换行符。         |



## 常见初始化

```css
* {
      margin: 0;
      padding: 0;
    }

    a {
      text-decoration: none;
    }

    ul,
    li {
      list-style: none;
    }

    input {
      outline: none;
    }

    img {
      display: block;
    }

    html,
    body {
      overflow: hidden;
      height: 100%;
    }
```



# 文资溢出变为..

```css
/*
	{
	overflow: hidden;
	text-overflow:ellipsis;
	white-space:nowrap;
}
*/
```

   

