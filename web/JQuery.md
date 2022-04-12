# JQuery

基本特性，HTML选取，HTML元素操作，CSS操作，HTML事件处理，JS动画效果，链式调用，读写合一，浏览器兼容，易扩展插件，ajax封装

jquery对象和dom对象的一些属性和方法不互通，所以需要转换，但底层封装的window对象下的东西(语法，属性，方法)是互通的(基于这个开发)

jquery转dom的方法，单个获取jquery对象下的元素即可(如单个下标，遍历等)，dom对象转jquery,使用$('dom对象')即可包装为jquery对象

jquery本质是把dom转换为一个对象，当作判断运算符时要想它们相等，需要把jquery对象转为dom对象，因为对象和对象开辟的地址不同所以不相等

## 1.0 基础概念

jquery再获取document时，不用加引号

+ 立即调用函数

  ```js
  /*
   $(function(){
   })
  */
  ```

+ 核心函数

  ```js
  /*
   jQuery本质上使用非常多的函数封装，使用 $||jQeury来调用，jQuery函数返回值一般为一个对象
  */
  ```

+ $(parse)

  ```js
  /*
   $(parse)
   1.当parse为函数时，表示当DOM加载完成后，执行此回调函数，和window.onload()类似
   2.参数为选择器字符串，会找所有匹配的标签，把他们封装成jQuery对象
   3.参数为DOM对象时，将dom对象封装成jquery对象
   4.参数为html标签字符串，创建标签对象并封装成jQuery对象
   
   5. $作为标识符调用他设置好的方法
   $.each(arr,(index,item)=>console.log(index,item))
  */
  ```

+ jquery获取dom对象

  ```js
  /*
   如果是用jquery方法获得的对象再标识符最前面需要加$标识,用jquery获得的元素已经被转换为jquery对象，因此必须使用jquery对象封装的属性和方法
   let $btns = $('button')
  */
  ```

+ jquery对象基本方法

  ```js
  /*
   $('str').size() ||  $('str').length  可以获得jquery对象长度   
   $.each(arr,()=>{}) 遍历数组 ，相似 arr.foreach()
   $('str').index() 可以显示自己再当前兄弟元素中排行第几(从0开始)
  */
  ```

+ jquery对象和dom对象的转换

  ```js
  /*
   通过$('str')封装的dom对象会变为jquery对象
   通过获取jquery对象的下标可以转换为dom对象进行操作
          var $div = $('div'); //jQuery对象
          var div = $div[0]; // 对jquery对象单个引用自动转化成dom对象
          var div = $div.get(0); // 转化成dom对象
          div.style.color = 'red'; //操作dom对象的属性
  */
  ```

## 1.1 基本操作

+ 选择器

```js
/*
一.元素选择器
 1. 选择id
 $('#divs')
 2. 选择元素
 $('div')
 3. 选择类
 $('.pi')
 4. 并集选择
 $('div,span')
 5.后代选择器
 $('div span')
 6.子集选择器
 $('div>span')
 7.选择第一个元素
 $('div:first')
 8.选择最后一个
 $('div:last')
 9. 选择除了.box以外的标签
 $('div:not(.box)')
 10. gt() && lt()选择器
 $('div:gt(0):lt(2)')  //选择所有下标大于0小于等2的元素
 11. 选择内容为xx的元素
 $('li:contains(xx)')
 12. 选择隐藏的元素
 $('li:hidden')
 
 二.表单选择器
 1.表单元素选择器
 $(':input')
 2.选择不可用的文本输入框
 $(':input:text:disabled')
 3.input选择器
 $('input')
*/
```





+ 工具方法

```js
/*
 $.each(arr,function()) 遍历任意数组或对象中的数据
 $.trim() 去除字符串两边的空格
 $.type(obj) 得到数据的类型
 $.isArray(obj) 判断是否是数组
 $.isFunction(obj) 判断是否是函数
 $.parseJSON(json) 解析json字符串转换为js对象/数组'
 $('div').each(function())遍历div对象
*/
```

+ 属性

  ```js
  /*
   jquery特性：只有一个参数的方法，不传递参数为读，传递参数为写 html() val()
   又两个参数的方法 传递一个参数为读 两个参数都传递为写 css() attr()
   1. attr()可以读取和设置自定义属性
   $('div:first').attr('title') 读取有title属性的元素
   $('div').attr('name','i') 给一个元素设置name属性
   
   2. $('str').removeAttr('title') 删除一个元素的属性
   
   3.$('str').addClass('abc') 再原有类上增加一个类
   4.$('str').removeClass('abc') 再原有类上减少一个类
   5.$('str').html('h1') 给一个标签添加内容，可识别html标签
   6.$(':text').val('str') 获取表单的输入值，也可以写入值
   7.$('input').prop('cheched',true) 用来操作元素的固有属性，布尔值，多用表单
  */
  ```

+ css方法

  ```js
  /*
   $('p').css('样式'，'参数')，如果只有一个值则是获取当前设置的样式值，两个参数则是设置样式 
   
   1.获取样式
   let $divs = $('div:first').css('width')
      console.log($divs.slice(0, -2)) 获取div的width值(为字符串，剪切后与number类型数据相减变为number数据)
      
   2.设置样式(jquery对象设置)
   let $divs = $('div:first')
      $divs.css('backgroundColor', '#ace')
      //当设置多个css样式的时候，可以采用对象写法(jquery允许)
      let $divs = $('div:first')
      $divs.css(
        {
          'backgroundColor': '#ace',
          'border': '1px solid #ccc'
        })
  */
  ```

+ offset()&&position()

  ```js
  /*
   jquery对象方法 offset()，可以获得一个元素相对于第一视口的left,top值，返回的是一个对象
   1.
   $('div').offset() // left top 值,是一个对象
   $('div').offset().left 拿到left值
   $('div').offset().top 拿到top值
   同时可以使用offset({left:30,top:30})可以更改left,top值
   
   2.
   $('div').position() // 相对于他的父元素的left和top值
   $('div').position().left 拿到left值
    $('div').position().top 拿到top值
  */
  ```

+ scroll()

  ```js
  /*
   $(document).scrollTop()||  $(document).scrollTop()不加参数可以获取页面原位置到当前位置的值，加参数表示设置页面值
  */
  ```

+ width()|| height()

  ```js
  /*
   单纯拿到 width和height值
   $('div').width() 拿到元素的width值(为数字 100)
   $('div').height() 拿到元素的height值(为数字 100)
   -------------------------
   拿到wdith+height+padding值
   $('div').innerWidth()
   $('div').innerHeight()
   -----------------------
    拿到wdith+height+padding+border值
    可以传入参数true,这样会计算外边距
   $('div').outerWidth([true])
   $('div').outerHeight()
  */
  ```

+ 筛选_过滤

  ```js
  /*
   1.过滤选择器 
   是在查找dom的过程中，使用选择器选择的方式，进行筛选，把匹配的Dom元素封装进jquery对象里
   2.筛选过滤的方法 
   是已经再jquery对象的前提下，对当前的这个对象进行过滤，这是只能通过方法解决，不能通过选择器
   
   let $list = $('li')
   $list.first()// 选择第一个元素
   $list.last() // 选择最后一个元素
   $list.eq(num) // 选择指定元素
   $list.filter('[class=box]') // 选择class为box的元素
   $list.filter('[class!=box]') // 选择class不为box的元素
   $list.has('span').css() // 选择li中有span元素的那个li元素
   $list.children() // 选择list的最近一级子集
   $list.children('span:eq(1)') 选择list的最近一级第二个span
   $list.find([元素]) // 找到它子集里的元素
   $list.parent([元素]) //选择它的一个父级(可以连写)
   $list.prev([元素]) // 选择它前面的一个同级元素
   $list.next([元素]) // 选择它后面的一个同级元素
   $list.prevAll([元素]) // 选择它前面的所有同级元素
   $list.nextAll([元素]) // 选择它后面的所有同级元素
   $list.siblings([元素]) // 选择所有兄弟元素 
   $list.append('content') // 再末尾添加一个内容，可解析语义化标签
   $('content').appendTo($lis) // 再末尾添加一个内容，可解析语义化标签
   $list.prepend('content')// 再开头添加一个内容，可解析语义化标签
   $('content').prependTo($lis) // 再开头添加一个内容，可解析语义化标签
   $list.before('content') // 再元素之前添加一个元素，可解析语义化标签
    $list.after('content') // 再元素之后添加一个元素，可解析语义化标签
    $list.replaceWith('content') // 将所有$list元素替换为content
    $list.remove(); // 删除节点
    $list.empty(); // 清空节点内容
  */
  ```

+ 事件绑定和解绑

  ```js
  /*
   1. 事件绑定
    $('div').click(()=>{})
    // jquery没封装的事件,可以通过,on()传参的方式调用底层代码和addEventlistener语法类似。还可以同时给一个属性绑定多个方法,on最终返回原绑定的对象，所以可以链式写法
    $('div').on('click',()=>{}).on('mouseleave',()=>{})
    
    // jqeury封装事件，$('div').hover 。hover 可接收两个回调函数 第一个回调函数执行移入逻辑，第二个回调函数执行移出逻辑，如果只传递一个回调函数则移入移出都触发此函数
    $('.box').hover(() => {
        console.log('我进来了')
      }, () => console.log('我出来了'))
   2. 事件解绑
    $('div').off();  // off可写参数，写参数表示解绑写下的事件，可同时解绑多个事件用空格隔开，不写参数表示全部解绑事件
    
   3. change事件 当内容发生改变时触发
  */
  ```

+ 事件委派

  ```js
  /*
  1. jquery封装委派函数 $('div').delegate('委派对象'，'事件',回调函数)
    $('ul').delegate('li', 'click', function () {
        $(this).css({
          'background': 'pink',
          'width': '100px',
          'height': '100px'
        })
      })
      
    2. dom对象和jquery对象混用  
      $('ul').click((event) => {
        event = event || widnow.event
        $(event.target).css({
          'background': 'pink',
          'width': '100px', 
          'height': '100px'
        })
      })
    3. on做委托 $('div').delegate('事件','委派对象'，回调函数)
    $('ul').on('click','li',function()){
    	$(this).css('background','pink')
    }
  */
  ```

+ 淡入，淡出,滑动，显示，隐藏，动画

  ```js
  /*
   1.$('div').fadeOut(time) 淡入
   2.$('div').fadeIn(time) 淡出
   3.$('div').fadeToggle(time) 淡入淡出切换
   4.$('div').slideUp(time)向上收起
   5.$('div').slideDown(time) 向下拉出
   6.$('div').slideToggle(time) slideUp&&slideDown切换
   7.$('div').show(time) 不写动画时间单纯显示，写动画时间，按时间要求显示
   8.$('div').hide(time)  不写动画时间单纯隐藏，写动画时间，按时间要求隐藏
   9.$('div').toggle(time)显示与隐藏切换
   10.$('div').animate(css样式,time).animate(...) 按css样式要求和动画持续时间开始进行动画，可以链式写法，旨在先完成一个动画，在完成另一个动画，可以无限连链 
   jquery封装的样式允许再字符串内进行运算操作 ,如 $('div').animate({left:'+=200'},1000)&&$('div').css('width', '+=200')
   11. $('div').stop(boolean,boolean)
   stop第一个参数代表是否清空队列，第二各参数代表当前动画是否执行完
   $('div').stop(true,true) 立即停止动画，元素回到最初位置，清空队列
    $('div').stop(false,true) 立即停止当前动画
    $('div').stop(true,false) 立即结束当前动画，元素停在当前位置(常用)，一般写在事件开头，用来初始化队列
    $('div').mouseenter(() => {
        $('div').stop(true, false)
        $('div').animate({ width: 300, height: 300 }, 1000)
  
      })
  */
  ```

  

+ jquery扩展

  ```js
  /*
   想要对jquery对象进行扩展，我们用到$.extend({})，把需要扩展得功能写在{}对象里
   $.extend({
   	max(a,b){  // 返回最大值
   		return a>b? a:b;
   	},
   	min(a,b){  // 返回最小值
   		return a>b? b:a;
   	},
   	leftTrim(str){   // 删除左边得空格
   		return str.replace(/^\s+/,'')
   	},
   	rightTrim(str){ // 删除右边得空格
   		reutrn str.replace(/\s+$/,'')
   	}
   })
  */
  ```

+ 多库共存(关键字一致时，能互相区分)

  ```js
  /*
   jquery可以释放$得使用权，让另一个库可以正常使用，此时jquery只能使用jQuery关键对象
    使用jQuery.noConflict(); 进行释放，之后使用jquery代码只能使用jQuery('str')
    $.noConflict();
      console.log(jQuery('input').width())
  */
  ```

+ onload&&ready

  ```js
  /*
   $(document).ready(()=>{}) 再dom加载完成后自动调用
   window.onload() 再dom,js,html,css加载完后才会调用
  */
  ```

  

