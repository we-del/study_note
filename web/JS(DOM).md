# JS(DOM&&BOM)

+ JS调用别人封装好的方法时，如果有返回值则会返回到该行(哪里调用的返回到哪里)

+ 将一个[object Text]转换为字符串

  ```java
  /*
  console.log(JSON.stringify((a.lastChild).data))
  */
  ```

  

## 1.0 DOM

html ,js都是对象型语言，html里的所有键值，属性值，都对应了js里的一个对象属性

js操作的永远是行内样式，在style标签中设置的样式，在js中获取不到

因为同步和异步的关系，同步先读取数据，异步在读取数据，所以同步数据只会执行一次，异步数据触发就会执行

+ 概念，及作用

  ```js
  /*
  1. DOM是一个文档对象模型，以可时文本和脚本有能力动态更新文档结构的1语言和接口
  2. window是浏览器窗口对象,document属于window的属性，而document又裂变了html,head,body等
  3. 文档对象 document ，window下的一个属性
  4. 元素 标签
  5. 根元素  html标签
  6. 文档树 以HUML为根节点，形成一颗倒立的树状结构，称为DOM树，这个树上的所有东西称为节点，也叫DOM对象
  */
  ```

+ window.onload

  ```js
  /*
   window.onload 为页面加载事件，只有页面加载完成才执行dom操作，如果没有加载则可能获取不到元素
  */
  ```

+ 获取元素方式

  ```js
  /*
   document.getElementById('n') 通过id指向一个元素
   document.getElementByTagName() 通过元素标签指向一个元素
   document.getElementsByClassName('pp'); 通过类名拿到元素
   document.querySelector('.pp') 通过选择方式拿到值 . # p
   document.querySelectorAll('.pp') 通过选择方式拿到所有值
   
   也可以通过父盒子>子盒子   父盒子 .子盒子 
  */
  ```

+ 事件基础

  ```js
  /*
   1.事件三要素 事件源 ，事件，事件处理回调函数
   2.事件处理三步 获取事件源DOM对象 
   3.html中各控件值，在js中可以看成对象属性进行调用修改
   4.a标签的href值也可以修改，通过a.href = 来更改目标值
   表单控件 checked = checked   在dom中获取为 check.checked =    true&&false,true为锁定,false为不锁定
   5. a.className = 可以修改元素的类名
   6. a.style.width = 500px  style是a对象的属性，属性里有width等属性
  */
  ```

+ setAttribute&&getAttribute

  ```js
  /*
   1.在操作元素属性时，语法只能操作元素天生具有的属性，如果时自定义属性，通过setAttribute&&getAttribute操作
   2.setAttribute&&getAttribute 无论是天生的还是自定义的都可以操作
   3. 设置元素控件值，node.setAttribute('名'，'值')
   4. 得到元素值，node.getAttribute('名')
  */
  ```

+ 改变html文本属性

  ```js
  /*
   获得元素属性后可以时用innerHTML && innerText && textContent,以下属性都会覆盖原来的值
   1. innerHTML 输入输出内容，碰到标签原样输出或显示，且会带空格和换行
   2. innerText 输入或输出文本内容，不包含标签，如果标签被隐藏，则也获取不到内容
   3. textContent 输入或出入文本内容，不包含标签，如果标签被隐藏，仍然可以获取
  */
  ```

+ 排他思想

  ```js
   var pLIst3 = document.querySelectorAll('.pp')
        for (var j = 0; j < pLIst3.length; j++) {
          // 记录值，开辟一个属性存储
          pLIst3[j].index = j + 1;
          pLIst3[j].onclick = function () {
            for (var i = 0; i < pLIst3.length; i++) {
              if (this == pLIst3[i]) this.innerHTML = '哈哈';
              //排他开始，自己显示的值为存储的属性
              else pLIst3[i].innerHTML = pLIst3[i].index;
            }
          }
        }
  ```

  

## 1.1 事件&&操作样式

 只要是被事件调用的函数，该函数身上都会有event环境参数(形参)

+ 鼠标事件

  ```js
  /*
  任何事件都有e&&event形参，该形参记录了鼠标的参数，及按键的参数
   1.onmousemove(鼠标移动事件)
   2.onmouseover(鼠标移入事件) -over&&-out为一组  给子元素添加行为时，把他的子节点看作一个单独的元素进行移入移出操作
   3.onmouseout(鼠标移出事件)
   4.onmounseenter(鼠标移入事件) -enter&&-leave为一组  给子元素添加行为时，会把整体看作一个元素，不管移入移出的哪个改变的都会父元素
   5.onmouseleave(鼠标移出事件)
   6.onclick(鼠标点击事件)
   7.onmousedown(鼠标按键按下事件)
   8.onmouseup(鼠标按键弹起事件)
   9.ondbclick(鼠标双击事件)
   10. 一个元素可以添加多个事件
   --------------------
   keyup&&keydown都有e或event形参，里面存的是输入键盘的编码及一些属性和方法，通过编码判断 e.keyCode可以做相关判断效果。回车编码为 13 ，也可以通过，e.key拿到键盘按下的字符串值
   可以判断按下的是否为 ctrl(ctrlKey可获得) alt(altKey可获得) shift(shiftKey可获得) 返回值都为布尔值
   11. onkeyup(键盘弹起事件) 
   12. onkeydown(键盘按下事件) 
   --------------------------
   13. onfocus(表单获得聚焦事件)  
   14. onblur(表单失去焦点事件)
   15. onmousedown(鼠标按下事件)
   16. onmouseup(鼠标弹起事件)
   ---------------------
   event环境变量
   event.target&&event.scrElement可以拿到触发此次调用的调用对象
  -----------------------------
  系统内部事件
  window.onload    系统加载事件，页面加载好后触发
  window.resize    系统浏览框改变事件，页面改变宽度触发
  window.onscroll  系统滚动事件，页面滚动触发
  */
  ```
  
+ 样式添加

  ```js
  /*
   js html css 都是对象型语言，在js中，可以通过获取元素对象，然后修改它的对象属性方式给它的值做一个修改。大类里管小类
  */
  var p = document.querySelector('.pp')
        p.onmouseenter = function () {
          p.style.width = 100 + 'px';
          p.style.height = 100 + 'px';
        } 
        p.onmouseleave = function () {
          this.style.width = 0;
          this.style.height = 0;
        }
  ```

+ 添加类

  ```js
  /*
   obj.classList 类下的属性可以帮助添加删除，检查类
   const div = document.createElement('div');
  div.className = 'foo';
  
  // 初始状态：<div class="foo"></div>
  console.log(div.outerHTML);
  
  // 使用 classList API 移除、添加类值
  div.classList.remove("foo");
  div.classList.add("anotherclass");
  
  // <div class="anotherclass"></div>
  console.log(div.outerHTML);
  
  // 如果 visible 类值已存在，则移除它，否则添加它
  div.classList.toggle("visible");
  
  // add/remove visible, depending on test conditional, i less than 10
  div.classList.toggle("visible", i < 10 );
  
  console.log(div.classList.contains("foo"));
  
  // 添加或移除多个类值
  div.classList.add("foo", "bar", "baz");
  div.classList.remove("foo", "bar", "baz");
  
  // 使用展开语法添加或移除多个类值
  const cls = ["foo", "bar"];
  div.classList.add(...cls);
  div.classList.remove(...cls);
  
  // 将类值 "foo" 替换成 "bar"
  div.classList.replace("foo", "bar");
  */
  ```

  

## 1.2 兼容设置

+ 兼容性

  ```js
  /*
   浏览器有，chrome,火狐，IE,Opera,safari ,IE8以下为低级浏览器，其他为高级浏览器
    function getOrSetAContent(obj, value) {
          if (arguments == 2) {
            if (obj.textContent) obj.textContent = value;
            else {
              obj.innerText = value;
            }
          } else {
            var result = '';
            if (obj.textContent) {
              result = obj.textContent;
            } else {
              result = obj.innerText;
            }
            return result;
          }
        }
   
  */
  ```

+ 随机改变颜色

  ```js
  /* 在js中，转换为html语义化标签需要把固定的字符使用字符串形式存储，把改变的参数用变量形式，字符串形式和变量参数之间用字符串拼接连接，html会自己隐式转换解析语义化标签*/
   var inputNode = document.querySelector('.inp');
        inputNode.onfocus = function (e) {
          this.value = ''
        }
        inputNode.onblur = function (e) {
          var randomColorRed = Math.floor(Math.random() * 256);
          var randomColorGreen = Math.floor(Math.random() * 256);
          var randomColorYellow = Math.floor(Math.random() * 256);
          this.style.backgroundColor = 'rgb(' + randomColorRed + ',' + randomColorGreen + ',' + randomColorYellow + ')';
          console.log(randomColorRed, randomColorGreen, randomColorYellow)
        }
  ```

+ div获取焦点

  ```js
  /*
   1. 本质上div只能添加点击事件，无法获取焦点事件，但是给div 添加 tabindex='正数' 即可获取添加焦点事件,tabindex只能写在元素行内才能生效
  */
    <div style="width: 100px;height: 100px;background-color: pink;" tabindex='100'>1</div>
   var divFocus = document.queryselector('div')
  divFocus.onfocus = function(){
      this.style.backgroundColor = 'red';
  }
  ```

  

## 1.3 节点

+ 概念

  ```js
  /*
   1.节点，文档树对它所包含的东西称为节点，最关注的是元素节点
   
   2.节点分为12种，3种常用，元素，文本，属性
   节点类型(node.nodeType)： 文本为3  元素为1   注释为8
   节点名称(node.nodeName)： 空格文本为#text 标签则转换为大写 如 <p></p> 为P 注释文本为 #comment
   节点值(node.nodeValue): 打印当前自己点的值，没有值为null(标签值为null) 
   
   3.查找节点是相对查找到的，查找对象是离它最近的一个子节点，子节点下的节点不参与查找
   
   4.html和body的文本节点可以直接用
   html: document.documentElement
   body: document.body
   html是特殊的，它不会那他附近的空白节点，只能打印 head,text,body
   
   5. 在打印节点时，注释节点也会被打印
  */
  ```

+ 子节点&&子元素&&父节点&&父元素

  ```js
  /*
   1. 子节点(node.childNodes)
   高级浏览器，获取，元素，文本，注释
   低级浏览器，获取，元素，文本，注释
   2.子元素(node.children)
   高级浏览器，元素
   低级浏览器，元素，注释
   
   3.父节点(parentNode)&&父元素(node.parentElement)
   拿到最近的父级元素，父节点和父元素拿到的值相同
   
  
  */
  ```

+ 获取其他节点

  | 节点功能               | 兼容性     | 语法                   |
  | ---------------------- | ---------- | ---------------------- |
  | 获取第一个子节点       | 都认识     | firstChild             |
  | 获得第一个子元素节点   | 高级浏览器 | firstElementChild      |
  | 获取最后一个节点       | 都认识     | lastChild              |
  | 获取最后一个元素节点   | 高级浏览器 | lastElementChild       |
  | 获取上一个兄弟节点     | 都认识     | previousSibling        |
  | 获取上一个兄弟元素节点 | 高级浏览器 | previousElementSibling |
  | 获取下一个兄弟节点     | 都认识     | nextSibling            |
  | 获取下一个兄弟元素节点 | 高级浏览器 | nextElementSibling     |

+ 节点常见操作

  ```js
  /*
  节点操作都是父级相对子级进行的
   一. 创建节点
   1.document.write()文本写入，不用，会覆盖之前的数据
   2.obj.innerHTML()做文本拼接然后系统内部进行转义
   3.document.createElement('标签') 创建标签，然后使用obj.innerHTML&&obj.innerText写入数据，最后使用obj.appendChild(变量)添加
   注意：使用createElement()创建的元素不能删除，且创建一个就是一个标签的存在。appendChild()是在该元素中，子元素里添加一个新元素
   
   二. 插入节点
   1.在元素之前插入使用，object.insertBefore(元素，插入对象)
   
   三.替换节点
   object.replaceChild(元素，替换对象)
   
   四.删除节点
   object.removeChild(被删除节点)
  */
  
  ```

+ 节点的增删改查

  ```js
  // 1. 增
  var arr = [1, 2, 3, 4];
      var str = '<ul>';
      for (var i = 0; i < arr.length; i++)str += '<li>' + arr[i] + '</li>';
      str += '</ul>';
      document.body.innerHTML = str;
  //--------------------------------
   var arr = [1, 2, 3, 4];
      var ulNode = document.createElement('ul');//<ul></ul>
      for (var i = 0; i < arr.length; i++) {
        var liNode = document.createElement('li');//<li></li>
        liNode.innerHTML = arr[i];
        ulNode.appendChild(liNode);
      }
      document.body.append(ulNode);
  
  // 2.删
  
  var ulNode = document.querySelector('ul');
      var liLastNode = document.querySelector(' ul li:last-child');
      ulNode.removeChild(liLastNode)
  // 3.插入元素
   var ulNode = document.querySelector('ul');
      var liLastNode = document.querySelector(' ul li:last-child');
      var liNode = document.createElement('li');
      liNode.innerHTML = 5;
      ulNode.insertBefore(liNode, liLastNode)
  
  // 4. 替换元素
  var ulNode = document.querySelector('ul');
      var liLastNode = document.querySelector(' ul li:last-child');
      var liNode = document.createElement('li');
      liNode.innerHTML = 5;
      ulNode.replaceChild(liNode, liLastNode)
  
  ```

+ 表单添加元素

  ```js
  
      var ulNode = document.querySelector('ul');
      var input = document.querySelector('input');
      var button = document.querySelector('button');
      button.onclick = function () {
          //表单属性value下有一个trim()方法，可以去掉字符串两边的空格，字符串之间的多个空格打印只会显示一个
        if (input.value.trim()) {
          var liNode = document.createElement('li');
          liNode.innerHTML = input.value;
          ulNode.appendChild(liNode);
        } else {
          alert('请重新输入')
        }
      }
      input.onfocus = function () {
        input.onkeyup = function (e) {
          console.log(e)
          if (e.key == 'Enter') {
            if (input.value.trim()) {
              var liNode = document.createElement('li');
              liNode.innerHTML = input.value;
              ulNode.appendChild(liNode);
              input.value = '';
            } else {
              alert('请重新输入')
            }
          }
        }
      }
  ```

+ 事件绑定和解绑

  ```js
  /*
   1. (Dom0中，所有浏览器使用)事件解绑，让事件指向null值。给相同一个对象添加同样的属性(事件)，后面的会覆盖前面的 
   
   例，obj.onclick = function(){console.log(1)};(绑定)
       obj.onclick = null();(解绑)
   2. (Dom2中，IE10以上和高级浏览器使用) obj.addeventlistener('事件',函数(可为匿名函数&&普通函数，函数名调用(不需要加()因为事件会自己调用函数，只写函数名即可))，[true|false(是否冒泡,默认false)]，无论是false还是true都会触发冒泡，只不过入栈顺序不一样，false为即进即出，true为先进后出)，可以同时添加多个相同的事件，且同一事件不会被覆盖。
     (Dom2事件解绑) obj.removeEventlistener('事件',函数(可为匿名函数&&普通函数，函数名调用(不需要加()因为事件会自己调用函数，只写函数名即可))，[true|false(是否冒泡,默认false)])，解绑函数要和需要解绑的绑定函数一致
     
    例 obj.addEventListener('click',fn);(绑定)
    function fn(){console.log(1)}
    obj.removeEventListener('click',fn);(解绑)
    
    (Dom2,IE10以下使用的监听函数)
    obj.attachEvent('事件',函数)
    obj.detachEvent('事件'，函数)
    
    例 obj.attachEvent('onclick',fn)
    function fn(){console.log(1)}
      obj.detachEvent('onclick',fn)
      
  对比,dom0&&dom2绑定
  1.dom0绑定加.onclick且单个事件只能绑定一个函数，移出事件指向null
  2.dom2(addEventListener)绑定加'click'且单个事件可以绑定多个函数,移出事件使用removeEventListener,可以指定冒泡
  3.dom2(attachEvent)和addEventListener基本类似，不过没有冒泡，且事件前要加on。移出事件用detachEvent()
  */
  
  //事件绑定可以写匿名函数和普通函数两种，普通函数调用要把函数名给事件属性，两种调用方法本质上是吧函数在堆中的地址赋值给这个事件，当事件触发后就回去堆区调用这个函数。addeventlistener和onclick等事件同理
  obj.onclick = fn;
  // 利于复用
  function fn(){
      console.log('我被调用了');
  }
  // 点击事件私有函数
  obj.onclick = function(){
      console.log('我被调用了');
  }
  
  
  // 兼容性封装，使用addEventlistener&&attachEvent
   function addEvent(obj,eventType,fn,isBubble){
       if(obj.addEventListener){
           obj.addEventListener(eventType,fn,isBubble);
       }else{
           obj.attachEvent('on'+eventType,fn);
       }
   }
  // 使用removeEventListener&&detachEvent
  function removeEvent(obj,eventType,fn,isBubble){
       if(obj.removeEventListener){
           obj.removeEventListener(eventType,fn,isBubble);
       }else{
           obj.detachEvent('on'+eventType,fn);
       }
   }
  ```

  

## 1.4 BOM

+ 事件流

  ```js
  /*
   1. 捕获，从外到内进行传播
   2. 冒泡，从内到外进行传播
   3. 元素锁定阶段(标准DOM事件流)，捕获->找到元素->冒泡，捕获基本不用
  */
  ```

+ 冒泡

  ```js
  /*
  一.处理冒泡
   1.如果子盒子和父盒子都绑定了事件，则因为冒泡会把父盒子的事件也触发，这是我们不想看到的，所以就要阻止冒泡
   2.利用函数内的环境对象形参，使用 event.stopPropagation()阻止冒泡
   function fn1(event) {
        event.stopPropagation();
        console.log(1)
      }
   3.部分ie浏览器不支持函数形参event,而是直接封装到window下
   function fn1(event){
   	event = event || window.event; // 即可解决兼容性问题
   }
   二.事件委托(利用冒泡)
    当需要给很多子元素添加相同的某些行为时，可以给他的父元素添加事件，通过冒泡来触发此事件，在通过环境变量event.targer拿到触发时间的对象，并对它的行为进行修改
    ulNode.onmouseover = function (event) {
    	  event = event || window.event;
        var liNode = event.target || event.scrElement; 兼容处理
        liNode.style.backgroundColor = 'pink';
      }
  */
  ```

+ window

  ```js
  /*
   1. window是bom的顶级对象，一般来说调用方法可以以window.方法 ， 的方式进行调用，但window可以省略
   2. window.location 可以获取和更改(重定向)用户地址栏
   3. window.history history.back(-1)退回上一个页面 history.forward(1)进入下一个页面
   4. window.navigator 为一个只读对象，用来描述浏览器本身的信息，包括浏览器的名称，版本，语言，系统等，常用的有 navigator.appName&&navigator.appVersion
   5. window.screen 提供了用户显示屏幕的相关属性，屏幕的长度，宽度等
   6.window.onload页面加载完毕，onload事件才会触发
   7.window.onresize 改变窗口大小时会触发的事件
  */
  ```

+ event 对象

  ```js
  /*
  只有事件才有这个环境对象
   1.系统自动封装的对象，任何事件
   event.target 指向事件触发源(这个事件是哪个元素触发的)
   event.clientX&&event.clientY相对于用户(浏览器视口)视口计算
   event.offsetX&&offsetY 相对于自身元素位置计算
   event.pageX&&event.pageY 相对于页面位置(包括页面下滑，左滑距离)计算
   event.screenX&&screenY 相对于显示器位置计算(和浏览器在显示器的位置有关)
   以上三种定位都是相对于其对应点的原点(左上角原点建立xyhtml坐标系)计算的
  */
  ```
  
+ 定时器

  ```js
  /*
   定时器都是异步进行的，在进程中，其实都是一条线，只不过有异步属性的变量会穿插运行
   函数可以为匿名函数，或普通函数(不用加()，系统会自己调用)
   1. 单次定时器(单词打印)
   setTimeout(函数，时间)
   是window下的一个全局方法，需要两个参数，第一个为回调函数。第二个是时间(以ms为单位)
   2. 延迟定时器(多次打印)(以ms为单位)
   setInterval(函数，时间)，和setTimeout写法一致，不过是多次循环
   3.清除延时定时器
   clearTimeout('需要被关闭的定时器')多和setInterval搭配使用
   清除定时器后，定时器还是会继续执行完本次的代码，不会立刻退出
  */
  ```

+ 实时获得时间

  ```js
  // 事件和定时器都是系统自动调用，所以可填匿名函数或函数名
  // 匿名函数调用可以继续在函数内部调用这个函数然后进行一些操作
  // 函数名系统自调用只能执行调用函数体的函数功能，不能进行额外操作
  setInterval(function () {
        var str = getTimeCurrently();
  
      }, 1000);
      function getTimeCurrently() {
        var date = new Date();
        var years = date.getFullYear();
        var months = date.getMonth() + 1;
        var days = date.getDate();
        var hours = date.getHours();
        var minutes = date.getMinutes();
        var seconds = date.getSeconds();
        console.log('现在是' + years + '年' + months + '月' + days + '日', hours + '时' + minutes + '分' + seconds + '秒');
      }
  ```

+ 在js中拿到元素的宽和高

  ```js
  /*
   1. js中获取html页面的元素只能为行内形式，所以写在style里的样式js获取不到
   
   2. object.offsetWidth&&object.Height&&offsetLeft&&offsetTop
     object.offsetWidth &&object.offsetHeight拿到的是由元素内边距，边框，自己宽度整合起来的大小
   object.offsetTop &&object.Left拿到的是该盒子(Left,Top)到第一视口的距离的距离
   
   3.object.clientWidth && object.clientHeight && object.clientLeft && object.clientop 
   object.clientWdith && object.clientHeight拿到的是 内容，内边距
   object.clientLeft && object.clientop 拿到盒子边框的长度
   
   4.object.scrollWidth &&object.scrollHeight&& object.scrollLeft && object.scrollTop 
   object.scrollWidth &&object.scrollHeight 计算的是自己的内容，内边距大小
   object.scrollLeft && object.scrollTop  表示内容滚动的距离，以盒子左上角建立坐标系，记录往下滚动和往右滚动的值，最终往右滚动页面左临界点的值与原来左边临界点距离就为滚动的值，距离长度返回给原来的参照点。往下滚动同理
   ----------------------------
   兼容处理
   scroll有兼容性问题，不同浏览器认为的滚动参照点不同(有的为html,有的为body)  
   scroll = document.documentElement.scrollTop || document.body.scrollTop 即可解决
   
   5.视口宽度求法
   document.documentElement.clientWidth
   document.documentElement.clientHeight
  */
  ```

+ 初始包含块

  ```js
  /*
   在html很多属性都是来限制document的，html可以控制，但document不能控制
   html body head实质上都是一些可以控制的标签，可以规定元素显示范围，但为了爬虫好抓取数据一般不做样式修改。不管这些标签如何变化，初始包含块不会改变
   1. 初始包含块，浏览器的第一屏视口(即浏览器在第一次加载时显示的页面范围)叫做初始包含块(等于第一屏视口大小)，如果给一个元素添加绝对定位后，如果父级都没有设置相对定位，则次绝对定位元素相对于初始包含块定位
   2. 初始包含块的上层才是document元素 所以层级为 document > 初始包含块 > html > body || head > 标签嵌套
   3. 如果给html或body的一个设置overflow属性，则此样式控制的是document,如果html和body都有设置overflow属性，则body控制页面的滚动条，html控制document的滚动条 。因此，想要禁止滚动条，需要给body和html分别设置overflow hidden(系统默认全自动)
   html,body{height:100%; overflow:hidden} 让html高度占满屏幕才能让多余的元素溢出隐藏，让这个overflow:hidden属性更有说服力
  */
  ```

+ 拖拽文本&&取消默认行为

  ```js
  document.onmousedown = function (event) {
        if (event.target == boxNode) {
          //算法1
          event = event || window.event;
          // 此算法可理解为，让鼠标坐标控制元素移动，主要是获取top和left值，使鼠标作表和实时定位坐标重合
          var boxstartWidth = event.offsetX;
          var bosStartHeight = event.offsetY;
          this.onmousemove = function (event) {
            if (event.screenX < 1600 && event.screenY < 900) {
              event = event || window.event;
              var boxTop = event.clientY - bosStartHeight;
              var boxLeft = event.clientX - boxstartWidth;
              boxNode.style.top = boxTop + 'px';
              boxNode.style.left = boxLeft + 'px';
            }
  
          }
          // 清除事件
          this.onmouseup = function () {
            this.onmouseup = this.onmousemove = null;
          }
  
          //----------------------------
          // 算法2
          // 用改变值后的x坐标-改变前的，然后+原来的初始位置
          event = event || window.event;
          var originX = event.clientX;
          var originY = event.clientY;
          var originTop = this.offsetTop;
          var originLeft = this.offsetLeft;
          this.onmousemove = function (event) {
            event = event || window.event;
            var finalX = event.clientX - originX + originLeft;
            var finalY = event.clientY - originY + originTop;
            this.style.top = finalY + 'px';
            this.style.left = finalX + 'px';
          }
        }
      
  // 取消浏览器默认行为(dom0中),取消默认行为的寻找需要看你使用的什么绑定事件的方式
  	return false;
  // (dom2中) event.preventdefault  
   // 低版本浏览器可能无法禁止，需要使用 object.setCapture&&object.setCapture(),全局捕获设置完毕，，需要立刻解除，因为所有的操作都被添加到此对象身上了，不立刻解除会出现进行不了正常操作的行为 。 释放操作：object.releaseCapture&&object.releaseCapture()
      
      }
  
  ```

+ getBoundingClientRect()

  ```js
  /*
   obj.getBoundingClientRect()可以获取一个对象的基本信息，如长度，宽度，x,y位置，left,top,right,bottom值
   obj.getBoundingClientRect().width(left等)
  */
  ```

+ 自制滚动条(pc端)

  ```js
  /*
   缩放比= 第一视口 / 内容高度
   scale = document.documentElement.clientHeight / content.offsetHeight;
   导航长度 = 第一视口*缩放比
   scrollNode.style.height = scale * document.documentElement.clientHeight + 'px';
  页面移动 = -移动距离/缩放比
  contentTop = -changeY / scale;
  */
  ```

+ 滚轮事件

  ```js
  /*
   1.滚轮事件只能用(dom2选择器中,obj.addEventListener())
   2. 使用 mousewheel(dom2) || DOMMousescroll(火狐浏览器专属)
   mousewheel事件中，有一个专属的环境变量属性event.wheelDelta,这个属性会获取鼠标滑轮的滑动情况，往上滑为120,往下滑为-120
   DOMMousescroll事件中，有一个专属的环境变量属性event.detail,这个属性会获取鼠标滑轮的滑动情况，往上滑为-3,往下滑为3
  */
  ```

  

