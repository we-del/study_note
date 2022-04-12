# less

变量之前可以做运算

## 1.0 变量

写css代码把一类代码写在一行，便于阅读，(定位相关，宽度内外边距，字体相关，背景图片相关)

+ 使用@定义变量，可直接引用到css样式中

```js
/*
1. 存储参数
 @width: 10px;
@height: @width + 10px;

#header {
  width: @width;
  height: @height;
}
2. 当作键值
@wrap:wrap;
.@{wrap}{	
	width: 100px;
	height: 100px;
}
*/
```

+ @变量，值得引用

  ```js
  /*
   1.@变量得值等于当前作用域最后修改得值为准，如果没有该值就回去外层作用域找，不会改变(影响)外层作用域得值
   @pos: 0;
  
  .bigbox{
    position: absolute;
    @pos:1;
    right: @pos;
    top: @pos;
    bottom: @pos;
    @pos:2;
    .innerbox{
      width: @pos;
      .innersmallbox{
        height: @pos;
        @pos:4;
         .microbox{
          height: @pos;
          @pos:5;
        }
      }
    }
  }
  
  --------------css代码
  .bigbox {
    position: absolute;
    right: 2;
    top: 2;
    bottom: 2;
  }
  .bigbox .innerbox {
    width: 2;
  }
  .bigbox .innerbox .innersmallbox {
    height: 4;
  }
  .bigbox .innerbox .innersmallbox .microbox {
    height: 5;
  }
  
  */
  ```

  

## 1.1 混合

+ 可以把相同的样式封装称一个混合(类似函数)，调用这个混合传入参数即可返回它设置好的样式

+ 混合得形参值可以设置初始值，再没有接收实参时，计算时会使用初始值来进行计算，避免报错

  ```js
  /*
  .bordered(@borT,@borB) {
    border-top: dotted 1px @borT;
    border-bottom: solid 2px @borB;
  }
   #menu a {
    color: #111;
    .bordered(@borT:pink,@borB:balck);
  }
  */
  ```

  

## 1.2 嵌套

+ less代码可以使用嵌套的方式，less会按嵌套顺序书写css代码

  ```js
  /*
  -------less
   #header {
    color: black;
    .navigation {
      font-size: 12px;
    }
    .logo {
      width: 300px;
    }
  }
  ---------------css
  #header {
    color: black;
  }
  #header .navigation {
    font-size: 12px;
  }
  #header .logo {
    width: 300px;
  }
  */
  ```

+ & 表示当前选择器得上一层父级

  ```js
  .clearfix {
    display: block;
    zoom: 1;
  
    &:after {
      content: " ";
      display: block;
      font-size: 0;
      height: 0;
      clear: both;
      visibility: hidden;
    }
  }
  ```

+ @媒体查询嵌套冒泡

  ```js
  /*
   .component {
    width: 300px;
    @media (min-width: 768px) {
      width: 600px;
      @media  (min-resolution: 192dpi) {
        background-image: url(/img/retina2x.png);
      }
    }
    @media (min-width: 1280px) {
      width: 800px;
    }
  }
  ---------- css
  .component {
    width: 300px;
  }
  @media (min-width: 768px) {
    .component {
      width: 600px;
    }
  }
  @media (min-width: 768px) and (min-resolution: 192dpi) {
    .component {
      background-image: url(/img/retina2x.png);
    }
  }
  @media (min-width: 1280px) {
    .component {
      width: 800px;
    }
  }
  */
  ```

## 1.3 映射

+ less可以引用其他元素得属性得值

  ```js
  /*
  1. 使用 元素名[属性名]得方式或得该属性得值
   #colors() {
    primary: blue;
    secondary: green;
  }
  
  .button {
    color: #colors[primary];
    border: 1px solid #colors[secondary];
  }
  ----------- css
  .button {
    color: blue;
    border: 1px solid green;
  }
  */
  ```

## 1.4 扩展

:extend ,引用:extend(类名) 后，引用得这一类选择器会以并集选择器得方式设置样式，可以再原有样式得基础上做扩展，多用于设置定位等固定元素位置

+ ```js
  /*
   .my-inline-block {
    display: inline-block;
    font-size: 0;
  }
  .thing1 {
    &:extend(.my-inline-block);
    width: 100px;
    height: 100px;
  }
  .thing2 {
    &:extend(.my-inline-block);
    width: 300px; height: 300px;
  }
  --------css
  .my-inline-block,
  .thing1,
  .thing2 {
    display: inline-block;
    font-size: 0;
  }
  .thing1 {
    width: 100px;
    height: 100px;
  }
  .thing2 {
    width: 300px;
    height: 300px;
  }
  
  */
  ```

  

# bootstrap

bootstrap是一个样式封装代码，里面封装了一系列解决兼容问题，响应式布局等容器

向上兼容：如果设置的分辨词缀小于当前分辨率依然有效，大于则无效

## 1.0 布局容器

布局容器和padding不能同时使用

+ .container&&.container-fluid

  ```js
  /*
   .container 类为固定宽度并支持响应式布局的容器
   .container-fluid 类用于 100%宽度，占全部视口
  */
  ```

  

## 1.1 栅格系统

可以随着屏幕或视口的伸缩分成对应的栅格，最多为12列，由自己分配

根据不同屏幕分成多种类

栅格系统占位根据上一级.row父类来划分

```js
/*
 每个屏幕都有 12栅格
 1.超小屏幕(<768px) 类前缀 .col-xs  如 .col-xs-3  
 2.小屏幕(>=768px) 类前缀 .col-sm    如 .col-sm-3  
 3.中等屏幕(>=992px) 类前缀 .col-md  如 .col-md-3  
 4.大屏幕(>=1200px) 类前缀 .col-lg  如 .col-lg-3  
 可同时给一个元素设置上述类，使它到合适屏幕时，产生合适效果
*/
```



## 1.2 列偏移

列偏移相当于左边多出一个占用.col-*-offset-x 的一个隐形的盒子(visibility)

```js
/*
 <div class="row">
 	<div class="col-lg-4"></div>
 	<div class="col-lg-4 col-lg-offset-4"><div>
 </div>
*/
```

## 1.3 列嵌套

根据已划分的栅格再进行划分(以划分后的栅格为基础)

```css
<div class="container">
    <div class="row">
      <div class="col-lg-4" style="background-color: #e1c522">
        2
        <div class="row" style="background-color:pink">
          <div class="col-lg-4" style="background-color: #a8c529">1</div>
          <div class="col-lg-4 " style="background-color: skyblue;">2</div>
        </div>
      </div>
      <div class="col-lg-8 " style="background-color: skyblue;">3</div>
    </div>
  </div>
```

## 1.4 列排序

通过使用 `.col-md-push-*` 和 `.col-md-pull-*` 类就可以很容易的改变列（column）的顺序。

push往右移动，pull往左移动

```css
<div class="row">
  <div class="col-md-9 col-md-push-3">.col-md-9 .col-md-push-3</div>
  <div class="col-md-3 col-md-pull-9">.col-md-3 .col-md-pull-9</div>
</div>
```

## 1.5 特效标签

`<small>12</small> ` 字体稍小

`<mark>12</mark>`  有背景标记

## 1.6 显示与隐藏

隐藏：使用 hidden-xs hidden-sm 等可以设置再不同的屏幕下隐藏的效果，根据后缀-xx设置在不同屏幕下的显示和隐藏

显示：使用visible-xx 可以设置下不同的屏幕下显示的效果   如 visible-md  visible-xs



## 1.7 修改网页标题

```js
/*
  可使用 document.title获取
  document.title = '快点 抓紧' // 修改网页标题
  可利用定时器做闪烁效果
*/
```



##  等多类设置查看bootstrap文档

