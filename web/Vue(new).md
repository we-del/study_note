#   Vue

+ 集合

  ```java
  /*
   1.<input type="number" > 表示限制输入的数据只能是数字，此时与vue的代理数据源绑定后，其接收的数据就为number类型
   2. == 和 === 区别 ， == 只会判断底层在隐式转换后的boolean值数据是否相等， === 除了对boolean值做判断，还会对它们的类型做判断(typeof)
   3. return 是同步执行的 （谁调用函数则return 就给谁，定时器中传入形参的函数的return是返回给调用者的，也就是j底层的）
   4. 正则的test方法可以判断一个正则是否匹配一个字符串，如果需要完美匹配，我们可以使用在正则上加上^$限制
   	const pattern = /^[1-9a-zA-Z_]+$/g; //以数字字母下划线开头一直匹配到结束都是包含 数字字母下划线的字符串为true，否则为false
   	pattern.test("123`1`+232"); // false ,test只会匹配一次且该一次是否匹配决定正则的返回值，如此匹配匹配123就结束了，因为其不满足结束条件
  */
  ```
  
+ html知识点

  ```java
  /*
   1.line-height 可以调整一个元素的行距，当和盒子高度相同时，就呈居中状态
   2.padding 可以挤压内容，在设置了宽高时不向外扩展，没有设置时，向外扩张
   3. white-space nowrap 可以将多个dom元素汇成一行
   4. margin & padding 在有些情况下会挤压盒子的大小(通常变大)，因此如果我们不希望这个变化，可以将其转换为一个弹性盒子(display: flex 即可)
  */
  ```
  
  

## 前言

![image-20220520174647313](D:\typora_import_images\typora-user-images\image-20220520174647313.png)

 + vue介绍

   ```java
   /*
   介绍
    	vue是一套用于构建用户界面的渐进式javascript框架
    特点
    	采用组件化开发，提高代码复用率(React同理)
    	声明式编码，提高开发效率(不用我们操作dom ,React同理)
    	使用虚拟DOM+Diff算法，提高页面重绘效率(React同理)
    	
    注意
    	渐进式指 可以自底向上逐层应用(简单应用：只需要一个清凉小巧的核心库，复杂应用：可以引入各种vue插件)
    	.vue文件中的html样式都为虚拟dom，最终会由vue翻译成真实dom
   */
   ```

 + React VS Vue

   ```java
   /*
    1. React和Vue的虚拟dom的处理非常相似，在其中都可以插入js表达式代码，最终会由其底层翻译为对应的真实dom 
    2. 都采取文档碎片和diff算法保存数据
    3. React在虚拟dom使用 js表达式 使用 {} 进行包裹，vue中。标签内容使用 {{}} 进行包裹，标签属性使用:或v-bind:开头
    	react中可以使用的js表达式包括该(t|j)sx文件中的变量和数据常量，js底层封装属性方法(直接写在{}中，如{true ? 1: 2})；vue中则只能使用其vue实例上的所有属性和方法（包括原型上的）和数据常量，js底层封装属性方法
    4. vue支持指令标签(最终会由vue底层完成识别最后转义为真实dom)，react不支持(只支持虚拟dom)
    5. Vue将每个需要使用的属性和方法封装成了一个对象块进行配置，而React不是，其文件就是他的对象块，所有属性和方法都在其中
    6. vue和react都需要给在遍历中产生的标签指定key值(给最外层父级)
    7. react和vue的虚拟diff算法一致，都是拿key当作唯一标识，页面重绘时，会对比前后文档锁片的key值，和虚拟dom树节点，相同的部分不做处理，不同的部分进行内容替换和局部更新
    8. react在遍历时需要我们手动指定key否则会报错，而vue在遍历时如果我们不手动指定key则其默认把索引当作key
    9. React和Vue的生命周期函数中的this都指向当前实例对象
    10. react和vue组件概念大致相同，相同的可以复用的代码(或一个页面代码过多时)可以抽象为一个组件，这样任何地方就能引入其进行运行(创建实例)
    11. react中 组件引入并在虚拟dom中时候后就可以使用创建实例(因为react在使用js表达式时，只能使用当前react文件中出现过的变量，js数据常量和js底层的属性和方法)，而vue中 组件引入后还需要在该vue组件实例对象(或vue实例对象)下注册后才能使用(因为vue在使用的js表达式时，只能使用vue实例上的所有属性和方法（包括原型上的）和数据常量，js底层封装属性方法)
    12. react和vue一致，组件使用大驼峰来完成命名，且需要见名知意(不能和html标签冲突)
    13. react中函数组件最终是把其返回的虚拟dom解析后返回到页面，类组件最终是把render生命周期函数返回的解析过后的虚拟dom返回到页面，而vue中组件最终是将template中的虚拟dom进行解析后返回页面(它们均是当状态改变后会重新渲染虚拟dom并进行diff差异比较，相同不做渲染，不相同的进行重新渲染(会去找前后相同的key进行配对))
    14. react和vue一致，最终执行 npm run build ,将react或vue项目打包为js，交给后端管理
    15. react和vue一致，区分一般组件(我们自己实例化)和路由组件(路由底层实例化)，再路由切换时，会先卸载掉上一个展示的路由然后再加载新路由
    差异集合
    	react使用js表达式时，当前文件下所有定义了的变量都可以在虚拟dom中使用。而vue只有其对象内部的data属性下定义的属性才能在虚拟dom使用(当使用该属性时，显示的是他的值)
    
    注意
    	js表达式指，有值在该行的语句(即地址)
    	
   */
   ```

 + vue虚拟dom生成

   ```java
   /*
    介绍
    	vue有两种挂载方式
    
    使用
    	// v3 写法
    	const EventHandling = {
           data() {
               return {
                   message: 'Hello Vue.js!'
               }
           },
           methods: {
               reverseMessage() {
                   this.message = this.message.split('').reverse().join('')
               }
           }
       }
       Vue.createApp(EventHandling).mount('#event-handling')
    
     // v2 写法
     	 new Vue({
           el:"#root",
           data(){
               return {
                   name:"小王"
               };
           }
       })
    注意
    	被vue管理的标签其被包裹的内容就是虚拟dom标签，最终会由vue完成真实dom的翻译并挂载到#root上
    	一个vue和其指定的虚拟dom是一对一管理，即一个vue实例对象只能接管一个虚拟dom
   */
   ```



+ vue实现页面重绘

  ```java
  /*
   vue2实现页面重绘
   	使用数据劫持(es5新特性Object.defineProperty)来完成页面重绘
   	数据劫持原理
   		多个虚拟dom节点的内容依赖于一个数据源得某个属性，当数据源的m改变后，所有引用了数据源相关值得部分也会一起改变(达到重绘目的)
   	使用(Object.defineProperty)
   		function defineReactive(obj, key, val) {
            Object.defineProperty(obj, key, {
              enumerable: true,
              configurable: true,
              get() {
                //收集依赖
                return val
              },
              set(newValue) {
                //触发依赖
                val = newValue
              },
            });
          }
     
      大致实现
      	<div id="root">
            {{name}}
          </div>
          <button>改变</button>
   	 	<script>
              let firstEntry = true;
              class MyVue {
                constructor(obj) {
                  let { el, data } = obj;
                  let targetDOM = document.querySelector(el);
                  if (firstEntry) {
                    let targetText = targetDOM.innerHTML.trim();
                    let pattern = /^{{(\w*)}}$/;
                    if (pattern.exec(targetText)[1] === "name") {
                      targetDOM.innerHTML = data["name"];
                      firstEntry = false;
                    }
                  } else {
                    targetDOM.innerHTML = data["name"];
                  }
                }
              }
              let obj1 = { el: "#root", data: { name: "张三" } };
              let obj2 = {};
              // 数据代理
              // definiProperty方法专门为了数据代理而推出得，因为它无法返回自身得属性或方法（会无线调用自身得get方法达到栈溢出）
              Object.defineProperty(obj2, 'name', {
                get() {
                  return obj1.data.name;
                },
                set(value) {
                  obj1.data.name = value;
                }
              })
              new MyVue(obj1); // 页面初次渲染
              let btn = document.querySelector("button");
              // 伪观察者模式，代理改变数据后，所有用到该数据得地方进行一次刷新
              btn.onclick = () => {
                obj2.name = obj2.name + "1";
                new MyVue(obj1);
              }
            </script>		
  */
  ```
  

## 集合

+ 特别注意

  ```java
  /*
   1. z-index 为负数时无法触发事件，无法进行滑动，及变为无法被选定状态(踩坑3小时)，在非必要时，不要给元素负的z-index(会影响交互).(因为dom事件是对当前点击部分的图层来触发的，谁在前触发的就是谁的点击事件，因此需要我们自己甄别)
   2. 出现bug时，需要对bug进行定位，分析为什么会出现这个bug(逐步调试),不要怀疑自己学过的知识
   3. vue和react的diff算法是根据前后目标的key值来做判断的，因此必须要给目标唯一的key值，且这个key值是固定的不会随时间移动改变的，在进行比较时，会去进行key值得配对(如果key值是随机得，则每次diff算法会将其当作一个新的的节点重新渲染，因此key值一定要是固定的，在更新前后都是相同的)，和虚拟dom树前后对比，重新渲染有差异得部分(相同不做处理)
   5. vue中 data(state)中的数据只能设置js常量初始值(完成初始化，确认每个属性的接收类型)，不能设置为vue收集的computed属性或$refs属性，因为在初始化data配置项时，那些配置项还没完成初始化，因此只能设置js常量，而后在mounted钩子函数中进行data(state)值的改变(此时vue实例的所有配置项都完成了初始化)
  */
  ```

  

+ 规范

  ```java
  /*
   1. 再虚拟dom中使用的js表达式尽量简洁不要有太多的层级(可以添加computed代理属性进行优化)
   2. 在编写vuex集中管理状态时，为了使对应的操作actions和mutations下的属性正确无误的调用通常搭配一个配置文件使用，该配置文件专门用于配置各条操作state的actions和mutations的属性(和redux的配置文件大致相同)
   3. vuex的actions和mutations的编写应该符合单一职责原则。即对每个actions和mutations所服务的业务功能进行分模块配置(如用户相关的一个模块，信息记录的一个模块)，然后再原vuex文件中的对应actions和mutations以 ...obj 展开
   4. vuex一般管理多组件共享数据和后台获取的数据
   5. 在后台数据还没有回到前端时，可以加载一个过渡动画来提示用户等待(同时也提高用户体验，不会出现闪屏现象)
   6. 在移动端项目中所有文字都应该设置像素（只有这样才会进行适配转换），否则默认为12px
   7. 写代码的时候一定要细心
  */
  ```
  
+ 注意

  ```java
  /*
   1. 如果下载的包和当前脚手架环境不兼容，则将版本即可
   2. 在vue中想要动态的引入本地图片(src使用js表达式) 需要使用require函数(外部资源则不需要，如网站资源)
   	如 <img :src="require(`../img/${index++}`)">
   3. flex使用时，会对当前父盒子的空间进行划分，基于此特性我们可以完成需要的布局
   4. 子盒子如果没有宽高会去继承直接父类的宽高
   5. 每次在F12 中的network(网络)下 更改网络速度后，记得重新调整为正确的速度
   6. 外部传入给一个组件实例的属性，如果该组件实例内没有使用props接收，则会被放到该组件实例的$attrs对象下
   7. flex布局完成溢出文字单行显示需要指定该盒子的宽度，否则会撑开盒子
   8. 执行data(state) 中的语句时，computed和$ref 还未初始化完毕，通常用来设置当前组件的状态(常量)
   9. 在v-for遍历出的元素，一定要给一个唯一的key值，否则会发送意想不到的错误(v-for遍历不提示key报错，而是默认把当前索引当作key值，这样很不友好，报错不提示)
  */
  ```

+ 差异

  ```java
  /*
   1. vue的样式scoped属性 可以避免组件间的命名冲突，当使用该属性后，会给当前组件的所有虚拟dom标签添加一个特殊的hash属性，再进行css属性选择的时候会使用该属性进行选择，避免了多组件间使用相同类名或标签名的冲突，而react没有此功能，因此只能写唯一的id或类
  */
  ```



+ Object常用得静态方法

  ```java
  /*
   Object.seal(obj) //对一个对象进行封存,不能对该对象进行删除和添加操作,返回一个封存后得对象(前后地址相同)
   Object.isSealed(obj) // 判断一个对象是否被封存，是则返回true，否则返回false
   Object.freeze(obj) // 对一个对象进行冻结，不能对其进行增删改操作
   Object.isFrozen(obj) // 判断一个对象是否被冻结，是则返回true，否则返回false
   Object.keys(obj) // 返回一个该对象所有key的数组
   Object.assign(obj1,obj2) // 将obj2的所有key-value追加到obj1中(相同key会进行覆盖)，会影响obj1且会把obj1作为方法的返回值 , 可以使用此方法(浅)拷贝一个对象(没有扩展运算符好用 ？)
   
  */
  ```

  

## 技巧

+ windows&jetbrans快捷操作

  ```java
  /*
  windows
   1. 选中需要的文本块
   	在起点位置点击一次，在终点位置按 shift 点击一次 即可选中区间文本片段
   2. 选中多个点位添加文本
   	按住alt 点击需要添加文本的节点(此时会开启一个文本出口)，即可同时添加多个相同的文本到出口
  jetbrans
  	ctrl + g 快速定位到指定行
  	ctrl home 快速定位到文件首部
  	ctrl end 快速定位到文本尾部
  */
  ```

  

+ 函数柯里化

  ```java
  /*
   前言
      string ,array,function 都可以获取长度属性，string和array为索引的长度，function为形参的长度
      柯里化（Currying）是一种关于函数的高阶技术。它不仅被用于 JavaScript，还被用于其他编程语言。
  	柯里化是一种函数的转换，它是指将一个函数从可调用的 f(a, b, c) 转换为可调用的 f(a)(b)(c)。
  	柯里化不会调用函数。它只是对函数进行转换。
  
   注意
   	在一个函数被柯里化后(即把一个函数当作形参传递给另一个函数，且该函数可以解耦调用)，我们称该函数为偏函数
   作用
   	柯里化可以对函数的调用进行解耦，实现闭包调用
   	
   使用
   	// 简单柯里化
   		function curry(f) { // curry(f) 执行柯里化转换
            return function(a) {
              return function(b) {
                return f(a, b);
              };
            };
          }
  
          // 用法
          function sum(a, b) {
            return a + b;
          }
  
          let curriedSum = curry(sum);
  
          alert( curriedSum(1)(2) ); // 3
  
  	// 高级柯里化
  		// func 是要转换的函数
          function curry(func) {
  
            return function curried(...args) {
              // 第一次 args为 1
              // 第二次 args为 1 2
              // 第三次 args为 1 2 3 满足条件
  
              if (args.length >= func.length) {// 直到满足该条件为止完成偏函数调用
                return func.apply(this, args);
              } else {
                // 不满足条件则实现一次递归
                return function (...args2) {
                  return curried.apply(this, args.concat(args2));
                }
              }
            };
  
          }
          curry((a, b, c) => { console.log(a, b, c) })(1)(2)(3)
  */
  ```

  

+ 关于原型，原型链（prototype,```__proto__```）

  ```java
  /*
   工作流程
   	js完成一次数据指向时，底层会帮我们将其数据进行包装(如 let a = 1 包装为 let a = new Number(1)) ，然后底层会调用其依赖的父类,完成加载，连接，初始化后,从顶级父类到当前子类实例执行属性，方法和构造器，并按栈结构顺序将属性和方法压入该地址内(最底层是Object基类，最顶层是当前子类实例对象)，然后收集该地址中的所有属性到最顶层，并划分原型区域，最后将新增的原型属性或方法加入到对应的区域内(只有构造函数原型和类圆形可以新增额外的属性和方法，且不会影响原类对象)，此时一次数据就创建完成了，(属性和方法查找时从当前地址顶部开始)
   	
   结论
   	只有构造函数和类有我们可以干预的原型属性(prototype), 且指向它们的构造者(类对象)，该原型(类对象)内收集着当前类上的所有方法(包括构造器，由于构造函数内部无法书写方法，因此其内部唯一的方法是构造器)，最终该原型属性(类对象)会以原型(继承)链的方式呈现在实例对象的地址上,(在实例化完一个对象后，该地址中从顶部到底部存储着 属性 ->当前类或构造函数的类对象(原型) -> 一级父类的类对象(原型) .... -> 顶级基类Object类对象(原型)（和java中创建实例对象的过程一致，不过js中有__proto__划分拿到对应类对象，且有属性提升(将继承而来的属性放到地址的最顶端)以及可以向原型(类对象)中添加新的属性和方法(不会影响原类对象数据)，而java只能进行查找使用）)
   	
   注意
   	instance 关键字就是判断一个实例对象的地址内是否包含目标构造器(从目标栈顶开始查找)
   	只有其构造函数原型和类原型身上有prototype属性，其实例对象下均没有
   	js中将类继承中的所有属性会放到该对象的地址顶部，类对象(原型)查找位置不变
   	我们可以手动向构造函数和类的原型属性(类对象)中添加我们需要的属性和方法（不会影响原型中的数据），来完成自己需要的继承特性，如向其类对象(原型)下添加属性和方法(java不可以)
   	
  */
  
  //验证代码
   /*
        js 有自己的一套原型处理机制
          其中 prototype指向其构造者（原型类或构造函数）相当于java中的原型类
              (只有函数和类才有构造者，因为函数有构造函数可以指向他的构造者（即java中的类对象），
                而普通函数没有)
          __proto__ 为当前对象的继承(原型)链 ，即再实例化这个对象的时候访问的上级父类或函数的类对象
      */
      const fn1 = () => {
        let a = 1;
      }
  
      console.log("fn_--------------------");
  
      console.log("prototype", fn1.prototype);
      console.log(fn1.__proto__);
      console.log(fn1.__proto__.__proto__);
      console.log(fn1.__proto__.__proto__.__proto__);
      const a = 1;
  
      console.log("a_--------------------");
      console.log(a);
      console.log("prototype", a.prototype);
      console.log(a.__proto__);
      console.log(a.__proto__.__proto__);
      console.log(a.__proto__.__proto__.__proto__);
  
  
      console.log("B_--------------------");
      class B { constructor() { this.a = 1 } hi() { let j = 1; } }
  
      console.log(B);
      console.log("prototype", B.prototype);
      console.log(new B());
      console.log(new B().prototype);
      console.log(new B().__proto__);
      console.log(new B().__proto__.__proto__);
  
      const Fn2 = function () {
        this.a = 1;
        let run = () => {
          let content = "我来run";
        }
      }
  
      console.log("fn2_--------------------");
      console.log("prototype", Fn2.prototype);
      console.log(new Fn2());
      console.log(new Fn2().prototype);
      console.log(new Fn2().__proto__);
      console.log(new Fn2().__proto__.__proto__);
  
      let obj2 = { a: 1, fn() { } };
      console.log("obj2--------------------");
      console.log(obj2);
      console.log("prototype", obj2.prototype);
      console.log(obj2.__proto__);
      console.log(obj2.__proto__.__proto__);
  
      class Parent {
        constructor() {
          this.money = 1999;
          this.name = "张三"
        }
        consumer() {
          console.log("我来消费");
        }
        rich() {
          console.log("我很rich");
        }
      }
      console.log("Parent_--------------------");
      console.log("prototype", Parent.prototype);
      console.log(new Parent());
      console.log(new Parent().prototype);
      console.log(new Parent().__proto__);
      console.log(new Parent().__proto__.__proto__);
      class Child extends Parent {
        constructor() {
          super();
          this.age = 19;
          this.ability = "practice"
        }
        say() {
          console.log("我来sping");
        }
        offer() {
          console.log("我来地宫");
        }
      }
  	Child.prototype.a = 1;
      console.log("Child_--------------------");
      console.log("prototype", Child.prototype);
      console.log(new Child());
      console.log(new Child().prototype);
      console.log(new Child().__proto__);
      console.log(new Child().__proto__.__proto__);
  ```

  

+ 注意

  ```java
  /*
   1. vue2 中,data里的数据类似于react中的状态，当其改变后会影响使用到该数据的位置。data里的数据都被加了数据代理(方便修改达到页面重绘)，method里的数据不会加数据代理，通常存储着方法(因为不需要改变)
   2. 在创建vue实例对象后，该data下的所有属性都会被vue加上数据劫持(也就是升级为React中的state)，而创建实例对象后新增在对象中的数据vue不会加数据劫持(也就没有升级为state) ，因此需要使用set方法来进行添加(set方法添加的数据vue会帮助做数据劫持)
   	 使用 Vue.set("data.xx","操作属性","属性值") || vue实例对象.$set("data.xx","操作属性"，“属性值"),需要注意的是，无法以data为根节点添加新的数据，而要以data.xx为根节点添加新的数据，否则会报错
   3. 目标源data数据(state) 存储在vue实例对象下的_data里，之所有vue最外层可以直接使用，因为其做了一个数据代理，把该数据挂载到了vue实例最外层
   4. vue底层只给指向数组元素的属性指定了数据劫持，没有给数组里的元素指向数据劫持，所以无法通过修改数组索引元素的方式重绘界面，但vue底层给常用增删方法绑定了数据劫持(push,pop,splice等.其对这些方法做了包装，当调用后改变数组里的内容，并进行一次页面重绘)，因此在该属性改变指向或使用了上述方法时也会产生页面重绘
   5. 组件指局部功能代码和资源的集合，可以把通用性的代码和冗余的代码拆分成一个组件(单一职责原则)，在进行引入组合
   6. 所有第三方引入的资源(如 bootstrap)可以在 index.html中进行引入
  */
  ```
  
  

## Vue基础

+ vue虚拟dom解析

  ```java
  /*
  使用
      <div id="root">
      	{{name}} // 虚拟dom内容使用js表达式时，使用 {{}} 包裹
          <a :href="url">点击</a> // 虚拟dom标签属性使用 js表达式时
          <input v-modal:value="input" || v-modal="input" > // 可以对data对象下的input属性进行读写操作
      </div>
      <script>
          new Vue({
              el: "#root",
              data() { // 此对象存储vue全局状态，当该其中的值改变后，引入该值的部分会被重绘(数据代理)
                  return {
                      name: "小王",
                      url: "https://www.baidu.com",
                      input: "1"
                  };
              }
          })
      </script>
   
   集合
   	1.在虚拟dom标签的内容区需要使用js表达式时，使用{{}}解析，在标签属性上需要使用js表达式可以在其属性的头部加上 v-bind: || : ,如 <a :href="true?1:2" || v-bind:href="true?2:3"></a> ，某个标签需要使用事件时，可以再标签属性内 使用 v-on:事件名="js表达式（函数）" || @事件名="js表达式(函数)"
   	2.input输入框(不限input类型)可以使用 v-modal:value || v-modal进行vue实例里,data属性指定数据的读写 
   	3.class 和 style 也可以插入 js表达式(再其数显头部加上v-bind:或:)，此时class可以为数组和对象(当为数组时必须是字符串,且会把每个数组索引上的值映射到该类名上;为对象时，属性为最终显示的class数据(string),其值为一个布尔值，控制该属性再class中是否存在，最终会把所有为值为true的属性映射到类名上)，style必须为一个对象(写法和react中的写法一致)
   	4.v-if , v-show 可以控制某个虚拟dom标签的显示与影藏 ，v-if是完全删除，v-show是添加display:none属性，v-if 和 js中的分支控制大致相同，可以组合写法 ，如 v-if v-else-if v-else(此时结构不能打乱否则报错)
   	5.v-for 可以遍历一个目标，得到每一个枚举的结果，如果遍历的是对象拿到其每一组key-value，是数组拿到每个索引的元素(和当前索引位置)，遍历的是字符串拿到每一个字符(和当前索引位置)，遍历的是数字则拿到1-该数的值（有 v-for="(i,j) in dataSource" || v-for="(i,j) of dataSource" 两周写法，都是相同效果）（使用v-for后会再该虚拟dom标签包裹层级内产生一个临时作用域，该作用域可以使用v-for遍历出来的数据，并生成对应数量的虚拟dom节点）
   	6. v-text 可以拿到当前js表达式，并替换掉当前虚拟dom即将显的内容 (不会进行html标签简写)
   	7. v-html 可以拿到当前js表达式，并替换掉当前虚拟dom即将显的内容 (会进行html标签简写)
   	8. v-cloak 为一个标记属性，当vue加载完后会删除此属性，借助该特性，在vue未完全加载时，可以使用属性选择器选择使用了该 v-cloak 属性的标签进行隐藏([v-cloak]{display:none}) ,在vue加载完后进行显示(添加用户体验)
   	9. v-once  只会读取一次data(state)中的初始数据，在该data改变后也不会影响器v-once
   	10. v-pre 当给某个标签加上此属性后，vue在读取到该虚拟dom标签后，不会对其进行进一步解析(比如是否使用了js表达式等) 
   	注意
   		4-10 都是vue底层自定义指令，再使用时实际上也时其再帮助我们操作dom（directive调用的函数第一个参数拿到真实dom节点，第二个参数拿到环境信息，来完成对应的dom操作）
   	
   注意
   	vue的虚拟dom使用js表达式时，只能使用原生得js属性和方法，数据常量和vue实例对象下的字段
   	v-if为false时会删除当前的虚拟dom(包括key的使用),v0show为false时实际上添加了display:none不会删除虚拟dom
   	v-modal也可以添加装饰符，如 v-modal.number="number" 表示接收input的value值会先被转换为number类型数据在传入data(状态)中;v-modal.lazy="data" 表示表单失去焦点后才收集数据;v-modal.trim="data"表示收集到的数据去掉其头尾的空格
   	v-modal默认对应该input表单的value值进行读写，当v-modal作用在不同类型表达中有不同写法
   		当作用在 text等输入框时 ， 对应data(状态)通常为一个 ""，输入框输入什么该data就显示什么
   		当作用在单选框时 (需要指定value)， 对应data(状态)通常为一个 "",当被选中时value就会被收集到其中
   		当作用在多个复选框时 (需要指定value),对应data(状态)通常为一个 数组[]，选中的复选框的value会被放		  到数组中,当对应的data为一个字符串时,默认收集该复选框的状态(true|false),通常用于单个复选框的勾选
      	当作用在 下拉框时 ， 对应data(状态)通常为一个 ""，下拉框选中什么，就收集对应的value值到其中
      在网站上动态渲染任意HTML是非常危险的，容易导致XSS攻击(用户cookie丢失 ,document.cookie 可以携带当前用户cookie)，一定要在可信的内容上使用v-html,不要再用户提交的内容上使用	
   		
  */
  ```

+ el和data的两种写法

  ```java
  /*
  
   // v2 中 
       data写法分为函数式和对象式（函数写法每次调用时返回一个不同地址相同数据的一个对象，而对象式再使用时，每次操作的都是一个地址，可能造成值得冲突）
       	 new Vue({
              el: "#root",
              data() { // 函数写法,该函数由vue底层调用
                  return {
                      name: "小王",
                      url: "https://www.baidu.com"
                  };
              }
               data: { // 对象写法
                  name: "小王",
                  url: "https://www.baidu.com"
             		 }
          })
       el写法分为创建vue实例时挂载，和创建后再挂载
            new Vue({
              el: "#root", // 创建时挂载
              data() { 
                  return {
                      name: "小王",
                      url: "https://www.baidu.com"
                  };
              }
          }).$mount("#root"); // 创建后挂载
  */
  ```

+ vue事件

  ```java
  /*
   介绍
   	vue想使用事件，可以再标签属性内 使用 v-on:事件名="js表达式（函数）" || @事件名="js表达式(函数)"
   事件写法
   	<button @click="()=>{console.log(1)}">Btn</button> // 第一种写法
   	<button @click="vueMehtod">Btn</button> // 第二种写法,此时会去找vue实例下的vueMethod方法调用
   	<button @click="(e)=>{vueMehtod(1)}">Btn</button> // 第三种写法，代理函数帮助调用函数并传参
   	<button @click="vuemethod(1,$event)">Btn</button> // 第四种写法，vue底层帮我们包了一层代理函数,使用$event参数可以拿到当前事件的事件对象
   	注意
   		底层会做一个判断，如果传入js表达式是一个函数则不做任何处理，如果不是则在其外部装饰一个代理函数（如写法四），该代理函数使用$event接收环境变量，因此我们可以使用$event参数传递给我需要的方法
   
   常用事件集
   	@scroll(滚动条移动触发) | @click | @wheel(鼠标滚轮移动触发) | @keydown | @keyup
   
   事件修饰符
   	介绍
   		e事件环境对象下常用配置，都被vue抽象为了其事件下的包装属性
   	常用修饰符
   		通用
              prevent 
                  阻止默认行为，如 @click.prevent="()=>{console.log(1)}"
              stop
                  阻止冒泡行为，如 @click.stop="()=>{console.log(1)}"
              once
                  再组件挂载后，事件只会触发一次
              capture
                  事件再捕获阶段就执行
              self
                  只有event.target是当前绑定self装饰属性操作的元素是才触发事件
              passive
                  事件默认行为立即执行，无需等待整个函数执行完毕(通常为事件函数执行完毕后，默认行为才执行，默认行为包括，点击跳转，滚动条移动等)
           键盘事件专用
           	enter | delete | esc | space | tab | up | down | left | right | caps-lock(大写)
           		只有按下对应的按键时才会调用此 @keyup | @keydown 事件
           注意
           	tab 只能使用 @keydown 事件绑定，因为其抬起来后光标已经切走了，keyup事件无法感知
           	系统修饰键,ctrl,alt,shift,mate 配合keydown时正常触发，配合keyup时需要按下其他键才能触发
           	事件装饰符可以链式编写，如一个事件想阻止默认行为和冒泡时，可以写为 @click.stop.prevent="xx"
         		键盘装饰符的链式编写， @keyup.ctrl.y="xx" 当按下ctrl+y才执行此事件，其余同理
   			
  */
  ```

+ vue实例配置对象可选值(v2)

  ```java
  /*
   let vue Config = {
   	el: "#root", // 把真实dom节点的包裹内容当作虚拟dom解析
      data: {} | ()=>:{}, // 做了数据劫持，当data更新后会重绘页面(类似于React中的state)
      method:{fn1 ....}, //  此处添加vue可能用到的方法，不会做数据劫持(即get xxx() | set xxx())
   	computed:{} , // 和 defineProperty 属性用法一致，可以自定义一个数据代理(get,set)属性(该属性可以是多个data字段的集合) ，最终会呈现在vue实例的最外层
   	watch:{}, //  可以检测某个属性的修改
   	filters:{} , // 过滤器，可以对需要加工的数据进行处理(和管道同理)
   	directives:{}, // 用于封装自定义指令(为一个函数，在使用时需要加上 v-函数名="js表达式")
   	components:{},  // 此处编写该实例vue使用的vue组件实例(这次后即可)
   	template:`` // 编写显示的虚拟dom
   }
   
   注意
   	method中的方法再页面重绘后会进行调用，而computed代理属性中定义的方法不会，只有前后数据不一致时才会重新调用get()获取数据
      el 存储当前升级为虚拟dom的标签；data指定刷新的状态；method多用于指定事件回调；computed多用于指定页面渲染时，需要加工的data数据(状态),多用于处理复杂的data组合渲染逻辑；watch用于监听data的变化
      computed属性再计算时，无法完成异步任务(因为是依赖其他状态，且需要返回 ,而js中的return是同步执行的)，再执行异步任务的时候可以使用watch属性来完成(没有使用return,通过改变另一个data中的数据来达到预期效果)
  
  =================================
   computed 使用
   	  let vm= new Vue({
          el: "#app",
          data() {
              return {name: "张", nameNick: "三"};
          },
          
          computed:{
              fullName:{ // 定义了 一个 fullName属性挂载到了 vue最外侧，使用时调用其get()方法，修改时调用set()方法 ， 如果没写get方法则无法使用此属性，没写set方法则无法修改依赖的值
                  get(){
                      console.log(this);
                      return this.name+"-"+this.nameNick;
                  },
                  set(value){
                      this.name =  value.split("-")[0];
                      this.nameNick = value.split("-")[1];
                  }
              }
          }
      })
      
      computed属性简写
   		前言
   			当某个computed代理属性只用读取data中的某些数据的组合时(get)可以进行简写
   		使用
   			 computed:{
   			 	// 只需读取fullName的属性时，可以写为此形式，底层会做转换相当于 fullName:{get(){}}，
              	fullName(){ 
                      console.log(this);
                      return this.name+"-"+this.nameNick;
                  }
              }
          }
   	注意
   		computed写的属性最终也会被挂载到vue实例的最外侧
   		method和computed都可以完成相同的功能,但是method每次页面重绘时会重新执行.而computed属性代理的数据再第一次执行后会进行缓存,再进行重绘时,会比较前后数据是否一致,如果一致则不更新，不一致才更新(diff算法)
   
   ========================================
    watch
     介绍
     	watch可以监视当前实例对象中data的变化，当改变后，会把该值当作handler函数的第一个参数传入，旧值当作handler函数第二个参数传入
     使用
    	new Vue({
          el: "#container",
          data() {
              return {isHot: false,count:1}
          },
          computed: {
              info() { // 使用了这个代理属性时执行该函数，底层会将该函数转换为get
                  return this.isHot ? "热" : "冷";
              }
          },
          watch: { // vue创建实例对象时配置
              isHot: {
              	deep: true , // 为true开启深度监视，即监视该状态下的所有属性(无论包裹多少层级),为false为浅度监视，默认只监视一层，且需要指向的地址发生改变后才生效
                  immediate: true, // 为true表示 再组件初次加载时立即执行
                  handler(newValue, oldValue) {
                      console.log(newValue, oldValue);
                  }
              }，
              count(newValue,oldValue){ // 当只需要使用handler方法时，可以简写为方法，底层会做对应转换
              
              }
          }
      }).$watch("isHot", { // vue创建实例对象后使用$watch方法配置
          immediate: true, // 为true表示 再组件初次加载时立即执行
          handler(newValue, oldValue) {
              console.log(newValue, oldValue);
  
          }
      });
      .$watch("isHot",function (newValue, oldValue){ // 当只用到handler方法时，$watch的简写
              console.log(newValue, oldValue);
  
          }
      );
      注意
      	watch里的数据默认只监视data数据里的变化，如果需要监视data嵌套里的某个数据则按其嵌套书写即可，规则为 "father.child.target" 
  
  ===========================
   filter
   	介绍
   		filter可以对前一个表达式的结果进行过滤包装(通常进行简单的包装处理)，返回一个新的表达式到该行，可以手动调用，也可让vue底层调用(在调用时会把上个表达式的结果当作第一个参数，我们传入的参数在其后传入)
   	使用
   		filter全局过滤器
   			Vue.filter("filterFun",(value)=>value+"我来返回") 
   		
   		filter局部过滤器(只有创建该过滤器的实例vue可以使用)
   		  <p>尝试一下过滤{{"尝试"|filterFun}}</p>
   		  new Vue({
              el: "#container",
              filters:{
                  filterFun(val){ return val+"我来完成过滤"}
              }
          });
   	注意
   		过滤器本质上是一个函数(js表达式)，因此可以在虚拟dom节点中使用
   		可以使用多个管道来对数据进行处理
  
  ===========================
   directives
   	介绍
   		可以在该对象下实现自定义指令(为一个函数(方法)，该函数接收两个形参第一个为使用该指令的dom节点，第二个形参为其内容的环境对象) ， 在使用时加上 v-函数名="js表达式"使用
   	使用（定义局部指令，只有当前vue实例对象才能使用）
   		// 函数简写形式(指令在元素成功绑定和页面重绘时会重新执行,即简写时，vue会帮助转换为 bigTwenty:{bind(){// 简写函数体..},update(){// 简写函数体..}}
              <p v-big-twenty="number">我会变大{{number}}</p>
              new Vue({
              directives:{
                  bigTwenty(ele,context){  
                      let {value} = context;
                      let htmlText = ele.innerText;
                      let finalChange = value * 10;
                      ele.innerText=  htmlText.replace(value,finalChange);
                  }
              }
              })
          
          // 对象写法
          	 bigTwenty: {
          	 	// 在初次绑定时调用
                  bind(ele, context) {
                      {
                          let {value} = context;
                          let htmlText = ele.innerText;
                          let finalChange = value * 10;
                          ele.innerText = htmlText.replace(value, finalChange);
                          console.log(ele, context);
                      }
                  },
                  // 在dom节点插入后调用
                  inserted(ele, context) {
                      ele.focus();
                  },
                  // 在页面重绘时调用
                  update(ele, context) {
                      {
                          let {value} = context;
                          let htmlText = ele.innerText;
                          let finalChange = value * 10;
                          ele.innerText = htmlText.replace(value, finalChange);
                          console.log(ele, context);
                      }
                  }
              }
      使用(定义全局指令)
      	Vue.directive("指令名",func | {}) // 传入func时底层会转换为 bind(){} update()
      	Vue.directive("change",{
          	 	// 在初次绑定时调用
                  bind(ele, context) {
                      {
                          let {value} = context;
                          let htmlText = ele.innerText;
                          let finalChange = value * 10;
                          ele.innerText = htmlText.replace(value, finalChange);
                          console.log(ele, context);
                      }
                  },
                  // 在dom节点插入后调用
                  inserted(ele, context) {
                      ele.focus();
                  },
                  // 在页面重绘时调用
                  update(ele, context) {
                      {
                          let {value} = context;
                          let htmlText = ele.innerText;
                          let finalChange = value * 10;
                          ele.innerText = htmlText.replace(value, finalChange);
                          console.log(ele, context);
                      }
                  })
   	注意
   		该函数内容通常直接来操作dom完成需求
   		每次页面重绘时，指令都会执行,指定详细分为 bind(){} ,组件初次加载时执行;inserted(){}，dom元素插入时执行;update(),页面更新时执行
   		如果html样式要求没有达到预期，试试对象写法把(如input组件只有在挂载到dom页面中后才能获取焦点，因此需要在inserted钩子函数中执行)
   		自定义属性定义为函数时，vue底层会将该函数包装为 bind(){}和update(){}
   		有多个单词组成的自定义指令使用小驼峰命令，在虚拟dom中使用时，需要使用 -分隔(vue底层会进行解析识别)，如 指令：numberShow(){} , 虚拟dom v-number-show="xx"(此时该虚拟dom使用的就是numberShow就这个自定义指令)
   		directives的this指向的是window
  */
  ```
  



+ 生命周期函数

  ```java
  /*
   初始化时(挂载时)调用的生命周期函数顺序
   	beforeCreate() （此时正在初始化，数据代理还未开始）  ->   created()（此时完成数据劫持和数据代理）      ->beforeMount() （此时页面呈现的是未经Vue编译的DOM结构(虚拟dom)） -> mounted()（此时vue已经完成了虚拟dom的解析，并将其挂载到页面上）
   	
   更新时调用的生命周期函数顺序(改变data(state)里的数据)
   	beforeUpdate()（此时数据已经更新，但页面还未重绘，此时对新旧虚拟dom比较） -> updated()（此时页面已经重绘）
   卸载时调用的生命周期函数(当卸载后该实例对象地址就被回收了，并断开了与其他组件的连接)
   	beforeDestroy() （组件即将销毁，通常做组件收尾工作，关闭定时器，关闭连接等） -> destroy()（）
   	
  */
  ```


+ 组件

  ```java
  /*
   介绍
   	vue组件概念和react相同，但分为非单文件组件(.html 其中有多个vue实例对象)和单文件组件(.vue)
   
   使用(非单文件组件)
   	<div id="root">
      	<School></School>
      </div>
      <script src="./js/vue.js"></script>
      <script>
          let School = Vue.extend({
              template:`
                      <div>
                          <p>我来编写标签 {{name}}.今天为{{years}}</p>
                      </div>
              `,
              data() {
                  return {name: "工程大学", years: 2022};
              }
          });
         let vm =  new Vue({
              el: "#root",
              components:{School}  // 局部注册组件，只有当前vue组件实例可以使用
          });
          console.log(vm); // 此时该组件挂载到了vue实例对象下的$children属性下
      </script>
   	Vue.component("School",School) // 全局注册组件，第一个参数为组件名，第二个参数为组件实例对象，注册后，该组件在全局都可以使用
   
     使用(单文件写法)
     		<template>
     			<div></div>
     		</template> // 编写虚拟dom
     		<script>
     			export defualt { // 只需向外暴露一个对象即可，vue底层在注册组件时会做判断，如果是一个vue组件则直接使用，否则会将其包装为组件(必须是一个对象)
     				name:"test", // 指定此vue实例的命名
     				data(){ return {name:"张三"}} , // 设置状态
     			}
     		</script> // 编写js代码 
     		<style></style> // 编写样式
   		注意
   			当其他文件引入此x.vue文件后，该文件下的template和style标签的内容会由vue底层加入到该对象属性下，且当该对象在此引入组件中注册完毕后，就相当于把该构造函数挂载到了该文件下(vue底层会完全自动装箱)，此文件在虚拟dom中使用此组件标签后，底层会帮助创建一个实例对象(即，每次在标签中引入了一个组件后，就相当于创建了一个改组件的实例对象)，并将其组件实例对象的虚拟dom挂载到该组件上(相当于该组件实例对象在此区域占位，显示的是template展示的虚拟dom，后续可以通过事件，生命周期函数等改变状态后，重新渲染虚拟dom，和react大致相同) 	
  
   注意
   	React和Vue一致，在使用组件标签时，就是创建了一个该组件的实例对象
   	vue组件和vue实例对象的配置项大致相同，但vue组件没有el属性，因为其只能当作组件使用，没有能力把虚拟dom挂载到真实dom上;且data属性必须写为函数形式，因为其可以被任意组件创建实例所以应该有一个单独的对象地址,否则其他组件在引入这个组件后(引入时就创建了一个该组件的实例对象)，操作的就是同一个data(state)
   	vue组件也有template和name属性，template属性用于编写虚拟dom ，当该组件被实例化时，展示的也是template属性下的虚拟dom； name属性为命名空间，可以修改开发者工具中呈现的名字
   	在vue组件实例或vue实例在使用某个组件时，除了引入外，还需要再该组件实例对象中进行注册，这样其组件上才会挂载引入的组件构造函数
   	我们可以使用Vue.extend({..})方式直接创建一个组件然后再vue实例对象的components属性下注册，也可以直接传入一个对象到vue实例中的components属性下，由其底层完成组件实例化并注册 
   	当使用了Vue.extend({...}) 之后，返回了一个对应该组件的VueComponent(options){} 构造函数(和类相同用法)，当使用 <Component/> 加载到虚拟dom中时，实际上是创建了一个该组件的实例对象
   	Vue组件就是一个青春版的vue实例，vue实例的大多功能vue组件上都有(没有el属性，必须由vue实例使用)
   	在组件开发中，我们只用暴露出一个组件配置对象即可，最终会由vue实例对象注册所有组件的父类后，其父类组件下的组件会继承的完成构造函数装箱(即可以理解为在对象属性的components:{}书写的组件默认会被包装为构造函数组件)，因此在引入一个组件的组件配置对象并注册后，就可以看作为 在本组件中注册了该组件的构造函数,在虚拟dom使用该组件标签后，就是创建了一个构造函数的实例对象
   	每个vue组件可以看作是一个构造函数的源文件，当引入配置对象(该配置对象会隐式挂载该组件中编写的虚拟dom和样式)，并注册后，就相当于把该vue组件构造函数挂载到了其组件上(在虚拟dom使用该组件标签就是创建了一个该组件实例对象)
  */
  ```
  



+ vue脚手架

  ```java
  /*
  安装
  	npm i -g @vue/cli
   使用
   	vue create 文件名 //即可在指定路径创建脚手架，如 vue create app   
   	
   vue脚手架文件解析
   	.gitignore 为 配置不上传到git仓库的文件或目录
   	babel.config.js 为 babel的配置文件
   	package-lock.json 为当前pakage.josn中用到的依赖包的详细信息，如依赖的下载地址，包名，版本号等，在package.json使用 npm i 默认安装时，优先按照 package-lock.json中的版号下载对应依赖(这样不会出现兼容问题)
   	README.md 为当前项目的说明书，再此编写需要其他用户知道的信息
   	public 为公共目录，在服务器上时，该路径为根路径
   		index.html 最终挂载页面
   	src 为当前源码文件
   		App.vue 为汇总文件，即所有文件的父类
   		main.js 是vue实例的入口文件，在此对所有vue配置对象进行包装
   		components 放置所有组件
   		assets 放置静态资源，如图片，视频等
   
   关于main.js中vue组件的挂载
   	在之前我们习惯于创建vue组件实例化，再通过template使用组件标签，达到挂载效果，但由于vue脚手架默认引入的是一个运行版的vue 不支持 该写法，因此只能使用完整版的vue(在该目录下的dist目录中的vue.js,体积过大)或使用render函数，使其帮助我们渲染
   	render配置项作用
  	   vue实例下的render函数会由vue底层调用，并传入一个虚拟dom解析函数到该函数的第一个形参，调用该虚拟dom解析函数并传入内容(组件或虚拟dom)就会返回一个完整的虚拟dom到该行，将其返回后，最终会挂载到指定的目标真实dom节点上。当vue引入的是一个青春版的vue时，创建了vue实例后就需要render属性帮助我们完成渲染，写为 new Vue({render:h => h("组件")}).$moun("#app")。
   	
   脚手架默认配置
   	使用指令
   		vue inspect > output.js 可以输出配置文件信息到指定文件(无法对原配置造成影响)
   	自定义配置项(重写)
   		定义一个vue.config.js并放到项目根目录下(和package.json同级)
   		使用
   			module.exports = {
                pages: {
                  index: {
                    // page 的入口
                    entry: 'src/index/main.js',
                    // 模板来源
                    template: 'public/index.html',
                    // 在 dist/index.html 的输出
                    filename: 'index.html',
                    // 当使用 title 选项时，
                    // template 中的 title 标签需要是 <title><%= htmlWebpackPlugin.options.title %></title>
                    title: 'Index Page',
                    // 在这个页面中包含的块，默认情况下会包含
                    // 提取出来的通用 chunk 和 vendor chunk。
                    chunks: ['chunk-vendors', 'chunk-common', 'index']
                  },
                  // 当使用只有入口的字符串格式时，
                  // 模板会被推导为 `public/subpage.html`
                  // 并且如果找不到的话，就回退到 `public/index.html`。
                  // 输出文件名会被推导为 `subpage.html`。
                  subpage: 'src/subpage/main.js'
                }
              }
   	注意
   		我们只能更改vue允许我们更改的配置项(在官网中可以查看 https://cli.vuejs.org/zh/config/)
   	
   注意
   	脚手架使用原则，选新不选旧，新版本会兼容旧版本脚手架
   	vue-cli // cli 指 command line interface
   	在引入第三方库时，如果只写目录名，则会去该目录的package.josn中找该目录的入口文件(引入 module属性指向的目录)
  */
  ```

+ vue组件常用属性(技术)

  ```java
  /*
  注意
  	vue组件中的data和react中的state非常相似，在data改变后页面就会进行一次重绘
  	
   ref
   	介绍
   		vue的ref和react的ref属性用法相同都是收集目标dom元素，vue的ref 更像是 即将废弃的react的ref写法，它会将所有写了ref属性的虚拟dom标签和组件标签收集到 该组件实例对象下的$ref属性下，key值为你在收集时指定的值
   		
   props
   	介绍
   		vue的props属性和react功能类似(都是父类向子类实例传递属性)，但写法不同(react直接挂载到组件的props属性下，vue则需要进行props声明才能使用(声明方式由数组和对象))
   	使用
   		{
   			// 方式一
   			props:["name","age"]  // 当为数组时表示可以从外部接收有限几个属性
   			
   			// 方式二
   			props:{
   				name: String, // 当为对象时表示可以从外部接收有限几个属性，且限制了接收类型
   				age : Number
   			}
   			
   			// 方式三
   				方式三由三个配置项 type(限定类型),require(限定是否必要),default(在没传递时的默认值)
   			props:{ // 当为对象时表示可以从外部接收有限几个属性，且限制了接收类型和是否必须存在
   				name:{
   					type: String, // 表示接收类型
   					require: true //  表示必须传递
   				},
   				age:{
   					type: Number. // 表示接收类型
   					default: 18 // 表示在没有传递时的默认值
   				}
   			}
   		}
   	注意
   		外部传入的props属性会被vue挂载到组件实例对象的最外侧
   		
   minin(混入)
   	介绍
   		vue 允许将多个有相同代码的配置项提取为一个单独的文件，在使用minin属性进行引入，达到代码复用目的
   	
   	使用
   		局部混合(只有局部会添加混入的代码)
   			{
   				mixin: [] // 混合配置项可以接收一个数组，意味着可以进行配置多个混合(属性配置项复用)
   			}
   		全局混合
   			Vue.mixin("混合") // 全局配置混合，所有组件属性上都会添加此混合配置项属性
   	注意
   		混合属性引入的配置项(属性)会和原来的配置项(属性)进行合并
   		
   插件
   	介绍
   		插件就相当于node的express中的中间件(express包使用的中间就规范为一个函数，而vue没有此规范)，在使用后就改插件可以往Vue全局上挂载一些东西
   		插件本质上就是一个对象，对象下包含n个方法，当我们使用了该插件后，该对象下的install方法就会被vue调用并把Vue构造函数当作第一个参数传入
   	使用
   		Vue.use("插件"); // 使用了一个插件，并调用了改对象下的install方法
   
   style属性 scoped
   	介绍
   		vue样式特有属性scoped，它可以限制该组件中的样式只服务于本组件的虚拟dom(不会出现样式冲突情况，外部最终给本组件中的每一个虚拟dom变迁了一个唯一的哈希值属性，然后使用属性选择器去选择该组件中的dom标签)，react无此属性，因此存在可能的样式冲突
   
    style属性 lang（默认为css）
    	介绍
    		lang属性可以指定当前style标签内使用的css预处理语言(sass,scss,less,stylus,css) , 当选定了对应的预处理语言后，需要下载对应的loader（由于 webpack版本更新 可能新旧不兼容，建议下载老版本的loader）
    
   $nextTick钩子函数
   	介绍
   		在所有DOM操作执行完后执行的钩子函数
   	语法
   		this.$nextTick(函数) // 底层会在合适的时间点调用该函数
  */
  ```
  
+ 自定义事件

  ```java
  /*
   介绍
   	自定义事件是对props传入方法时，事件调用此方法的一个优化(无法获得返回值)，它可以向组件实例绑定一个自定义事件后，目标组件可以使用 this.$emit("事件名","..传入参数") 触发此事件调用此方法；这样可以不把方法暴露给其他组件的同时调用此方法(迪米特法则 ， 组件间应该尽可能的减少了解)，但无法获得该方法的返回值
   优点
   	父组件无需向父组件暴露方法对方即可完成传参调用
   错点
   	无法获得该方法的返回值
   
   自定义事件 vs props传参
   	自定义事件 不需要暴露方法到其他组件，即可让其他组件完成调用，但无法返回方法的返回值
   	props传参 需要暴露方法到其他组件，通过调用该方法，获取返回值
   
   使用
   	自定义事件(直接绑定，缺少灵活性)
   		 // 父类
   		<Component @myselfEvent="()=>{console.log("绑定自定义事件")}"/> 
  		// Component.vue
           {
           	methods:{
           		handler(){
           			this.$emit("myselfEvent","我来充当参数") // 调用该事件并传入了一个参数
           		}
           	}
           }
      
      自定义事件(延迟绑定) 
      	// 父类
      	<Component ref="component"/>
      	{
      		method:{
      			event(){
      				console.log("我被调用了"); 
      			}
      		},
      		mounted(param){
      			// 可以自己定义事件的绑定时间点，更具有灵活性
      			this.$ref.component.$on("event",this.event);
      			this.$ref.component.$once("event",this.event); // 绑定只能触发一次的自定义事件
      		}
      	}
      	
      	// Component.vue
           {
           	methods:{
           		handler(){
           			// 触发该事件实际上就等于调用了其事件的方法，我们可以传入适当的参数到其上
           			this.$emit("event","我来充当参数") // 调用该事件并传入了一个参数
           		}
           	}
           }
      	
    注意
    	自定义事件是对props传递方法的一个优化，它可以在不暴露该方法的情况下，完成该方法的调用(符合迪米特法则)，因此，如果以后需要传递给方法让其他组件调用(且该组件不依赖此方法的返回值)时，就使用自定义事件吧，这样可以在不暴露方法的情况下，完成方法调用(如果一个方法依赖父类的返回值则还是需要使用props传递方法)
    	自定义事件传递调用的方法，其返回值无法接到，$emit()返回的是当前组件实例对象
    	在一个组件被卸载后，vue会现在掉他和其他组件键的联系，并取消掉绑定到该组件上的自定义事件
    	methods中的this指向的是当前组件实例对象，使用 传入匿名函数this.$on("event",function(){}) 绑定事件时，this为事件的调用者，因此如果想要this指向当前实例对象要么使用箭头函数，要么传入methods对象下的方法
    	给组件绑定的事件默认为自定义事件，如果想绑定原生dom事件需要使用事件装饰属性 native,这样该事件就会被绑定给该组件的最外侧包裹盒子,如 @click.native="click"
    	自定义事件实现了子组件向父组件通信
    	
   解绑自定义事件
   	介绍
   		不需要使用的自定义事件，需要及时解绑
   	语法
   		 this.$off("自定义事件名") // 解绑指定自定义事件
   		 this.%off(["1","2"]) // 解绑多个自定义事件
   		 this.$off()  // 解绑全部自定义事件
  */
  ```

+ 全局事件总线

  ```java
  /*
   介绍
   	可以实现全局组件通信
   原理
   	在vue原型上挂载一个vue实例对象(该对象有$on,$emit,$off等方法，可以挂载和卸载事件)
   使用
   	new Vue({
   		el: "#app",
   		render:(h)=> h(App),
   		beforeCreate(){
   			Vue.prototype.$bus = this; // 在创建初始化状态之前往Vue原型上挂载一个vue实例对象(包含$on,$emit.$off等方法)
   		}
   	})
   注意
   	当某个组件即将卸载时，需要删除其在事件总线上绑定的方法，避免资源浪费
   	该总线上可以绑定属性和方法，及自定义事件(通常绑定自定义事件)
  */
  ```

+ PubSub

  ```java
  /*
   介绍
   	和React中用法相同
   安装
   	npm i pubsub-js
   语法
   	pubsub.subsribe("test",(_,data)=>{}); // 订阅消息 ， 返回一个订阅者id
   	public.publish("test",11); // 向指定端口发送消息(订阅该端口的位置都会接收到该消息)
   	public.unsubscribe("subId"); // 删除指定订阅，需要传入订阅者id
  */
  ```


+ 过渡与动画

  ```java
  /*
   介绍
   	vue实现的过渡和动画需要使用 transition标签进行包裹，被包裹内容在进行动画时，会由vue底层添加(在没有指定name属性时) v-enter-active和 v-leave-active 类，如果指定了name属性则对应添加的样式为 name属性值-enter-active 和 name属性值-leave-active(可以更灵活的设置动画效果)
   语法
   	vue动画指定了三个出现和三个离开时的类
   	v-enter,v-enter-active,v-enter-to  // 出现时会挂载的三个类
   	v-leave,v-leave-active,v-leave-to // 离开时挂载的三个类
   	通常再 v-enter-active和v-leave-active施加过渡效果 ，v-enter类中书写开始的位置，v-enter-to书写开始的目标位置，v-leave书写离开的位置，v-leave-to编写离开的终点位置
   	
   细节
   	一个transition抱歉只能给一个外层包裹标签添加过渡，如果需要使用多个过渡效果需要使用 transiton-group包裹，且需要给每一个虚拟dom标签一个唯一的key值
   
   扩展
   	集成第三方动画库
   		安装
   			npm i animate.css  // 一个样式库
   		简单使用
   			    <div>
                      <button @click="flag=!flag">展示/</button>
                      <transition
                          name="animate__animated animate__bounce"
                          enter-active-class="animate__lightSpeedInLeft"
                          leave-active-class="animate__rotateOut"
                      >
                          <div v-show="flag">我来展示</div>
                      </transition>
                  </div>
                  import "animate.css";
   注意
   	transition标签有一个属性 appear表示再初次加载时执行此动画
   	再transition标签中没有添加name属性时，默认向包裹标签挂载 v-enter...,v-leave等三个类，如果书写了name属性会挂载 name属性值-enter...,name属性值-leave等三个类(书写了什么类就挂载对应的类)
   	该过渡同样也是事件驱动的，当我们触发事件导致目标transtion包裹元素的显示与隐藏时，该包裹标签身上就会挂载  v-enter,v-enter-active,v-enter-to(显示时调用) , v-leave,v-leave-active,v-leave-to(隐藏时调用) 标签(这些标签符合开闭原则，即在不修改源码结构的情况下 扩展功能)
  */
  ```

+ 前端代理

  ```java
  /*
   介绍
   	当后端不配合我们使用 jsonp,cors解决跨域时，前端的一个解决跨域的方法(访问接口),他还可以共享指定代理后端的public文件下的内容，如图片，音频文件等
   作用
   	帮助转发请求，和获取后端public文件下的资源
   注意
   	前端代理会先去当前域名下查找此资源，如果没找到指定资源才会去后端的public下查找，因此指定域名要和当前域名的地址保持一致，否则无法完成代理查找(端口使用不受限制)
   	
   配置方法(只能使用一种方法)
   	方式一(此方法只能配置一个代理)
   		再前端没有此资源的时候，会去转发给指定url路径，请求其的资源
   		再vue.config.js暴露对象的最外层中添加
  			devServer:{
  				"proxy":"url" 
  			}
   	方式二(此方法可以配置多个代理)
   		在匹配代理时，需要加上指定query前缀，才会触发对应的代理转发数据，需要注意的是，在转发请求时要重写请求query路径，否则会照原样发送给后端，因此可能获取不到指定资源
   		再vue.config.js暴露对象的最外层中添加
  			devServer:{
  				"proxy":{
  					"/api":{ // 如果请求前缀是api则触发此代理，需要注意的是，必须重写此请求路径，否则会照源路径转发请求，导致资源请求失败	
                          target:"http://localhost:5000",
                          // 如果后端匹配路由需要此前缀就不重写，不需要则进行重写
                          pathRewrite:{"^/api":""}, // 重写掉代理匹配前缀
                          ws:true, // 用于支持webssocket
  				      changeOrigin: true // 为true时请求发起地址和目标服务器一致，为false显示原始请求发起地址
  					},
  					"/interface":{ // 如果请求前缀是api则触发此代理，需要注意的是，必须重写此请求路径，否则会照源路径转发请求，导致资源请求失败	
                          target:"http://localhost:6000",
                          pathRewrite:{"^/interface":""}, // 重写掉代理匹配前缀
                          ws:true, // 用于支持webssocket
  				      changeOrigin: true // 为true时请求发起地址和目标服务器一致，为false显示原始请求发起地址
  					}
  				}
  			}
   	注意
      	前端代理匹配前缀是否重写，取决于后端暴露的接口，如果后端以 "/api/add" , "/api/remove"等编写接口，且前端代理以 "/api"当作匹配前缀则无需重写， 否则则需要重写
   工作过程
   	会先在当前端口下查找是否可以解决，解决不了会由代理转发请求，去另一个端口下查找解决
  */
  ```



+ vue-resource

  ```java
  /*
   介绍
   	在v1.0版本主流发布请求的库，已经被axios替代(功能和axios非常相似，语法几乎相同)，是vue的一个插件
   安装
   	npm i vue-resource 
   使用
   	Vue.use(vueResource) // 使用该插件，vue底层会调用该对象下的install方法，并把Vue构造函数刚做第一个参数传递，此时会向其原型上挂载 $http 属性
   	vue.$http("url").then(res=>console.log(res)) 
  */
  ```



+ 插槽(slot)

  ```java
  /*
   介绍
   	插槽和React标签组件的props.children 属性有点类似，都是将组件实例标签的被包裹虚拟dom(包括组件)载入到指定位置(react中是将其加入到该组件的props的children属性下，vue是将其加入到该组件的插槽中)，但插槽将载入的虚拟dom又进行了slot分层，而React则只能全部使用那些虚拟dom(react的children属性将包裹结构放到了一个数组中，我们可以对该结构进行分析达到相同效果，不过vue插槽更直观)
  
   
    默认插槽
    	介绍
    		直接在实例组件标签中书写的虚拟dom，会被收集到该组件的默认插槽中，该组件可以其内使用默认插槽来展示那些被包裹的虚拟dom
    	使用
    		 // Father.vue
    		  <Child>  // 所有没有指定slot属性的虚拟dom标签会被放到该组件实例的默认插槽中
                <h1>孩子你真棒</h1>
                <h1>你真nb</h1>
             </Child>
            
            // Child.vue
                <div>
                   <slot>我来展示该组件实例标签的包裹虚拟dom</slot>
                </div>
   
    具名插槽
    	介绍
    		具名插槽可以渲染被指定放置到该插槽上的虚拟dom标签
    	使用
    		// Father.vue
    		// template标签包裹
    		<template>
              <Child>
                  <h1>孩子你真棒</h1>
                  // 多个虚拟dom标签都需要载入到具名插槽中时，可以使用一个虚拟dom包裹，并再其属性上指定即可，但这样会导致多的包裹层级出现，因此可以使用template标签包裹(不会产生dom结构)且可以使用简写形式
                  <template v-slot:nb> 
                      <h1>你真nb</h1>
                      <h2>可以给我签个名吗</h2>
                  </template>
              </Child>
          </template>
          
          // 虚拟dom标签包裹写法
          <template>
              <Child>
                  <h1>孩子你真棒</h1>
                  // 多个虚拟dom标签都需要载入到具名插槽中时，可以使用一个虚拟dom包裹，并再其属性上指定即可，slot="具体slot name属性"
                  <div slot="nb"> // 如 此虚拟dom标签会被 name为nb的slot插槽收集
                      <h1>你真nb</h1>
                      <h2>可以给我签个名吗</h2>
                  </div>
              </Child>
          </template>
          
          // Children.vue
          <template>
              <div>
                  <slot>我来展示该组件实例标签的包裹虚拟dom</slot>
                  <slot name="nb">我来展示该组件实例标签的包裹虚拟dom</slot>
              </div>
          </template>
          
     作用域插槽
     	 介绍
     	 	slot允许向待传入的被包裹的虚拟dom传入其当前组件的属性和方法，但其必须使用template标签进行包裹，且使用 v-slot:插槽名="接收名"接收(scope接收的数据就是js表达式,且是一个对象)
     	 使用
     	 	// Child.vue
     	 	    <div>
                  <slot>我来展示该组件实例标签的包裹虚拟dom</slot>
                  <slot name="nb" :dataSource="desc">我来展示该组件实例标签的包裹虚拟dom</slot>
              </div>
              
          // Father.vue
          	<Child>
                  <h1>孩子你真棒</h1>
                  <template v-slot:nb="{dataSource}">
                      <h1>你真nb</h1>
                      <h2>可以给我签个名吗</h2>
                      <h2>{{dataSource}}</h2>
                  </template>
              </Child>
   
   注意
   	<slot>标签也可以载入虚拟dom,在其没有接收到父类组件传入的虚拟dom时,显示的就是该标签所包裹的内容
   	想一次向一个具名插槽传入多个虚拟dom可以使用一个标签进行包裹，然后再其标签上指定插槽即可(无需每个虚拟dom都指定插槽)，但此方式会多产生一层dom结构，因此可以使用template标签进行包裹(不会产生dom结构)，且其标签在指定插槽时，可以进行简写为 <template v-slot:插槽名>...</template> 即可向指定插槽中载入虚拟dom，当该插槽再该组件实例中使用时，即可渲染此内容
  */
  ```



## Vuex

+ 注意

  ```java
  /*
   1. vuex 管理分模块的状态时，必须使用 属性(key)-值(value)的方式编写，不能使用语法糖(只写一个变量)，这样底层不会识别
   2. mapState,mapActions等 实际上是返回一个对象和一系列指定的方法(方法名和返回的结果由程序员指定)，因此我们需要使用 ... 对其进行解构，然后将mapState映射的数据放到computed下(底层对computed对象下的函数会做处理，会将其转换为该属性的get方法,在使用后会调用get返回获得代理数据),mapActions解构后放到methods中,等待调用执行
  */
  ```

  

+ 介绍

  ```java
  /* 
   vuex是专门用于再Vue中实现集中式状态管理得一个Vue插件，对vue应用中多个组件得共享状态进行集中管理(和react中的redux相似)
  */
  ```

  

  ![image-20220527102409888](D:\typora_import_images\typora-user-images\image-20220527102409888.png)



+ vuex基本使用

  ```java
  /*
   安装 vuex
   	npm i vuex
   注意
   	Vue 3+ 匹配 Vuex 4 版本 ，Vue 2 匹配 Vuex 3 版本 
   	学完了 Vue 3+ 在学 Vuex 4
   使用
   	// vuex 3.x 写法
   	import Vue from "vue";
  	import Vuex from "vuex";
   	Vue.use(Vuex); // 再使用了该插件后(调用了该对象下的install方法并把Vue构造函数传入了)，vue实例对象属性中就允许传入一个store配置对象(管理着全局的状态)，并再所有vue组件实例中挂载了该store的仓库($store)
  
      export default new Vuex.Store({
          actions:{
          // actions下的所有方法接收两个参数，第一个参数为当前actions的上下文(相当于一个mini版的store)，是一个对象，第二个参数为即将改变的值
          	["add"](context,value){
          		context.commit("add",value); //使用commit方法后会去调用mutations对象下对应的属性方法，参数由vue底层帮忙传入(底层会调用对应属性的方法，并传入两个参数，第一个参数为state对象，第二个参数为value值)
          	}
          },
          mutations:{
          // mutations下的所有方法接收两个参数，第一个参数为当前状态对象，第二个参数为actions传递的即将		改变的值
          	["add"](state,value){
          		state.sum +=value;
          	}
          },
          // 再state对想想编写 vuex集中管理的状态
          state:{ 
          	num = 0;
          }，
          // getters属性下的属性方法默认接收一个参数state,相当于一个全局版的computed，可以对state的状态进行加工，拿到需要的状态
      	getters:{
  			bigSum(state){ // 相当于computed属性简写形式，使用时为 $store.getters.bigSum
  				return state.sum*10; // 将数值扩大
  			}    	
      	}
      })
      
      // 客户端使用vuex管理状态
      	{
      		mounted(){
      			this.$store.dispatch("add",1); // 调用属性名为 add的函数，并传入了即将修改的值(底层会调用对应的action，传入的第一个参数为当前上下文，第二个参数为即将修改的值)
      		}
      	}
      
   注意
   	actions下的方法就像一个缓冲层(用于处理业务和发起请求，该actions函数通过调用store下的dispatch方法调用(disptach的第一个参数为调用的actions，第二个参数为即将修改的状态)，)
   	如果再确认不许要使用到actions缓冲层下的方法时，可以直接使用 store下的commit方法调用mutations下的方法进行处理状态(commit的第一个参数为调用的mutations下的属性，第二个参数为即将修改的状态，底层收到参数后会去调用对应mutations下的属性方法，并传入参数(第一个形参为当前状态对象，第二个形参为即将修改的状态))
   	actions中的属性方法的第一个参数为当前是一个对象，包括 commit,dispatch,getters,rootGetters, rootState,state属性
   	再actions中的属性方法也可以直接操作状态，但非常不推荐这样做，因为vuex状态工具检测不到本次状态改变
   	当store管理的状态改变后，使用该状态的地方就会进行重绘
   	可以使用computed代理属性，去得到store中需要用到的状态，避免再虚拟dom中使用过多的属性层级获取状态
   	
  */
  ```
  



+ 常用map映射store数据到computed代理属性中

  ```java
  /*
    mapState和mapGetters使用
    	import {mapState,mapGetters} from "vuex";
  	export default {
  		computed:{
  			// 对象写法，展开后相当于 count(){return this.$store.state.count}
  			...mapState({count:"count"}), // 属性名会被当作代理属性名，"count"为从store对象中映射的状态，被当作get方法的返回结果
  			// 数组写法，如果代理属性名和状态名一致可以写为数组写法
  			// 相当于 count(){return this.$store.state.count}
  			...mapState(["count"]);
  		
  			// 相当于 bigNum(){return this.$store.getters.bigNum}
  			..mapGetters({bigNum:"bigNum"}), // 和mapState对象写法一致
  			...mapGetters(["bigNum"]); // 和mapState数组写法一致
  		}
  	}
    
    mapActions和mapMutations使用
    	import {mapActions,mapMutations} from "vuex";
  	export default {
  		methods:{
  			// 对象写法，展开后相当于 count(n){this.$store.dispatch("count",n)}
  			...mapActions({count:"count"}), // 属性名会被当作方法名，"count"为调用的actions对象下的属性方法，并默认把count函数的第一个形参当作value形参传入
  			// 数组写法，如果方法名和状态名一致可以写为数组写法
  			// 相当于 count(n){this.$store.dispatch("count",n)}
  			...mapActions(["count"]);
  		
  			// 相当于 count(n){this.$store.commit("count",n)}
  			..mapMutations({count:"count"}), // 和mapState对象写法一致
  			...mapMutations(["count"]); // 和mapState数组写法一致
  		}
  	}
    
   注意
   	如果需要自定义代理属性名则适用对象写法，如无特殊要求可适用数组写法
  */
  ```
  
+ Vuex模块化管理状态·

  ```java
  /*
  介绍
  	可以分对象的管理vue状态，避免所有actions和muations出现在同一个对象中，和redux模块化很像
   使用
   	const countConfig = {
   		namespaced:true, // 命名空间为true后，使用 mapState,mapActions等可以直接获得指定模块下的actions,state等 ， 如  ...mapState("count",["count"]) // 获得count模块 下的count状态
   		actions:{},
   		mutations:{},
   		state:{},
   		getters:{}
   	}
   	export default new Vuex.Store({
   		modules:{
   			count: countConfig, 
   		}
   	})
   注意
   	再使用store模块管理后，需要配置命名空间选项为true(namespaced:true)才能通过指定模块完成mapState, mapActions等获取其的action,state等; 如果需要手动调用store完成完成api(actions或commit等) 则需要使用 "模块属性/操作属性"的方式完成调用，如 this.$store.commit("count/add",1)
  */
  ```

  



+ 使用案例

  ```java
  //Computer.vue
  <template>
      <div>
          <h2>当前总数为{{count}}</h2>
          <select v-model="n" id="default">
              <option :value="1" selected>1</option>
              <option :value="2">2</option>
              <option :value="3">3</option>
          </select>
          <button @click="subtract(n)">sub</button>
          <button @click="add(n)">add</button>
          <h1>放大的数为{{magnify}}</h1>
      </div>
  </template>
  
  <script>
      import {mapState, mapActions,mapGetters} from "vuex";
      import {ADD, SUBTRACT} from "./config";
  
      export default {
          name: "Computer",
          data() {
              return {n: 1};
          },
          computed: {
              ...mapState("countOpt", ["count"]),
              ...mapGetters("countOpt",["magnify"])
          },
          methods: {
              ...mapActions("countOpt", [ADD, SUBTRACT]),
          },
          mounted(){
              console.log(this);
          }
      }
  </script>
      
  // count.js
      import {ADD, SUBTRACT} from "./config";
  
  export default {
      namespaced:true,
      actions: {
          [ADD](context, value) {
              console.log(context);
              console.log(value);
              context.commit(ADD, value);
          },
          [SUBTRACT](context, value) {
  
              console.log(value);
              context.commit(SUBTRACT, value);
          },
      },
      state: {
          count: 0,
      },
      mutations: {
          [ADD](state, value) {
              console.log(state,value);
              state.count += value;
          },
          [SUBTRACT](state, value) {
              state.count -= value;
          },
  
      },
      getters:{
          magnify(state){
              console.log(state);
              return state.count*10;
          }
      }
  }
  // config.js
  export const ADD = "add";
  export const SUBTRACT = "subtract";
  
  // store.js
  import Vue from "vue";
  import Vuex from "vuex"; 
  import count from "./count";
  Vue.use(Vuex);
  
  export default new Vuex.Store({
      modules:{
          countOpt: count
      }
  });
  ```

  





## 路由

+ 路由踩坑

  ```java
  /*
   1. 在写路由时，如果效果和预期不符，则可以添加路由守卫对本次路由切换进行拦截，查看此次请求的来源和去向。只有路由的路径正确则一定会匹配
  */
  ```
  
  
  
+ 介绍

  ```java
  /*
   vue路由概念和react路由概念一致，用于单页面应用跳转(SPA,single page application)
   
   注意
   	vue3 的路由配置写法有更新，需要单独学习
  
  */
  ```

  ![image-20220528140409371](D:\typora_import_images\typora-user-images\image-20220528140409371.png)

+ 基本使用

  ```java
  /*
   安装路由插件
   	npm i vue-router
   
   简单使用
   	配置 路由环境
   		// router.js
   		import VueRouter from "vue-router";
          import First from "./First";
          import Last from "./Last";
          import Act from "./Act";
          import Ope from "./Ope";
          export default new VueRouter({ // 配置路由规则并导出
              routes:[
                  {
                      path: "/first",
                      component: First,
                      children:[ // 多级路由嵌套和react路由一致，子路由无需携带/ ,当前端路由的路径为/first/act时，匹配此路由
                      	{
                      		path:"act",
                      		component: Act
                      	},
                      	{
                      		path:":id",
                      		component:Ope,
                      		props: true // 匹配到的params参数以props传参的方式传递给Ope组件实例对象
                      		// props也可以写为对象形式，但无法动态的改变值(少用，不灵活，用来传递不会改变的数据)
                      	}
                      ]
                  },
                  {
                      path:"/last",
                      component:Last
                  }
              ]
          });
          
          // Last.vue
          <template>
              <h1>i am a last</h1>
          </template>
  
          <script>
              export default {
                  name: "Last"
              }
          </script>
          
          // First.vue
          <template>
              <h1>i am a first</h1>
          </template>
  
          <script>
              export default {
                  name: "First"
              }
          </script>
          
         // main.js
          import Vue from 'vue'
          import VueRouter from "vue-router";
          import router from "./test/router";
          import App from './App.vue'
          Vue.config.productionTip = false
  
          Vue.use(VueRouter); // 使用vue-router插件，vue全局都可以使用其路由标签
          new Vue({
              render: h => h(App),
              router, // 全局使用此路由规则
          }).$mount('#app')
          
          // app.vue
          <template>
            <div id="app">
              <router-link to="/first">first go</router-link> // 相当于React中的 Link标签
              <router-link to="/last">last go</router-link>
              <router-view></router-view> // 指定出口位置，相当于React中的 Outlet组件(Outlet组件和此虚拟dom标签大致相同)
            </div>
          </template>
  
          <script>
          import TodoList from "./test/todolist/TodoList";
          import Computer from "./test/vuex/Computer";
          // import Father from "./test/custom/Father";
          import BusTest1 from "./test/bus/BusTest1";
          import BusTest2 from "./test/bus/BusTest2";
          import Transi from "./test/transition/Transi";
          import ReqShow from "./test/req/ReqShow";
          import Father from "./test/slot/Father";
          export default {
            name: 'App',
            components: {
             TodoList,Father,BusTest1,BusTest2,Transi,ReqShow,Computer
            },
            mounted(){
              console.log(this);
            }
          }
          </script>
  */
  ```



+ 路由基本配置

  ```java
  /*
   const routes = [
   	{
   		path: "/", // 该路由的匹配路径
   		component: Component, // 路由匹配成功后渲染组件
   		redirect: "/login", // 重定向 url路径
   		meta:{} , // 配置该路由的meta信息
   		children:[ // 其子路由
   		
   		]
   	}
   ];
  */
  ```

  

+ 路由虚拟dom标签

  ```java
  /*
   介绍
   	只有使用了vue-router 组件后，全局才会挂载以下虚拟dom标签
   
   <router-link></router-link> // 相当于react中的 Link组件，其也有一个to属性，用于路由跳转
   	active-class属性
   		当该路由匹配后使用该类样式
   	to属性
   		将url请求路径变为to指定的路径，有字符串和对象写法，当写为字符串时，为改变的url路径(query参数可以使用?xx=x携带)；当写为对象时，path属性为改变的url路径，query属性为携带的请求参数，params属性为携带的路径参数(在使用params后无法使用path属性,只能写name属性)，name属性可以指定跳转到的路由(需和路由配置中的name属性的值相同)，多在路由嵌套多层时使用，简化路由嵌套
   	replace属性 
   		用于开启replace替换浏览记录(每次打开一个replace的路由时，会替换掉当前栈顶的一个路由地址)
   	push属性(默认)
   		用于开启 push压入浏览记录(每次打开一个push路由时，会向栈顶压入一条数据)
   		注意
   			params参数为路径匹配参数，如路由的匹配路径为 /:id，表示匹配任意一级路径，且路径参数会收集到该组件实例中，为一个对象，key为路径占位符(上例为id),value为匹配到的路径 ，如 /name 则对象为{id:"name"}
   		总结
   			to属性通常写为字符串形式，但如果路由嵌套多层时，可以使用对象写法指定name属性跳转到的路由，来			完成简化开发目的
   <router-view></router-view> // 相当于react中的 Outlet组件，用于呈现路由组件
   
   <keep-alive></keep-alive> // 可以让被包裹路由组件在切换时不被卸载，默认全局都不卸载，可以指定include属性来指定那些路由组件不被卸载(即保持原有数据，在react中可以将数据保存到redux或localstore中，在首次加载时获取即可)
   	include属性
   		用于配置不卸载的路由组件(value为组件名),如果是单个写成字符串即可，如果是多个则写成数组形式
   	适用
   		 <keep-alive include="Component"></keep-alive> 
   		
   注意
   	路由组件其再创建组件实例对象时，其组件是实例上回多出来$route(存储着本路由的信息)和$router属性(存储着全部路由的信息)
   	再路由切换时，默认为先卸载掉前一个展示路由，然后挂载新的路由进行显示
  */
  ```



+ 路由组件生命周期函数

  ```java
  /*
   介绍
   	在使用了VueRouter插件后，所有的组件构造函数中都包含了	$route，$router,和两个生命周期函数等功能
   
   activated(){} // 此钩子函数当路由组件被激活时执行(展示到页面了)
   deactivated(){} // 此钩子函数当路由组件失效时执行(没有战士到页面)
   注意
   	以上两个生命周期函数为了解决在使用<keep-active>后,路由组件不卸载问题(mounted(只会在该组件首次加载时触发)和beforeDestroy会失效因为该组件不会被卸载),用来替换mounted,和beforeDestroy生命周期函数完成的工作, activated在该路由组件被激活时调用(代替mounted),deactivated在该路由组件失效时会调用(代理beforeDestroy)
   	vue路由组件在切换时，默认会进行卸载，但使用了<keep-active>后会进行路由组件缓存，并不会卸载
  */
  ```

  

+ 路由配置props传递参数

  ```java
  /*
   介绍
   	可以让路由组件更加轻松的使用query,params参数，不必使用$route.query.xx获取(把该逻辑封装到路由中)
   细节
   	路由配置props参数有三种写法，第一种为对象写法，此时传入的props参数被限定住了无法更改，因此只有在参数不变时适用此方法。第二种为布尔形式，为true表示将接收的params参数当作当作props参数传入给匹配的路由组件。第三种为函数形式，该函数返回一个对象，并接收一个形参$route,底层会调用该函数并传入$route，此时可以根据我们的需要灵活的返回props参数到该路由组件
   使用
   	export default new VueRoute({
   		routes:[
   			{
   				path:"/:id",
   				props: true,  // 布尔形式，将param参数当作props参数传递给该路由
   				props:{name:1}, // 对象形式 ，不灵活，适用于传递不变的数据。vue会将对象进行解构并当作props属性传递该该组件
   				props($route){ // 函数形式，可以传递给需要的props属性给当前路由组件，该组件在适用时就无需返回的从$route中获取数据
   					return {id:$route.query.id}; // vue将返回的对象进行解构后，当作props参数传入给该组件实例
   				}
   			}
   		]
   	})
   	
   注意
   	当向一个组件传递props参数，如果该组件实例没有接收时，则会放置到该组件实例的$attrs对象下，如果接收则不会放置到其中
  */
  ```

+ $route,$router

  ```java
  /*
   $route属性下有 fullPath(显示当前url的前端路径) ，hash ,mete,name(当前路由的name)，params(匹配的路由参数)，path(当前匹配的路径) ,qeury(当前携带的查询参数)
   $router属性中常用的数据和方法(原型链(继承链)上)
  	back() 回退一次  ，forward() 前进一次 ， 
  	go(number)  根据number数执行前级你和后退，如 -1为后退一步，2为前进两步
  */
  ```




+ 路由守卫

  ```java
  /*
   前置路由守卫
   	介绍
   		该方法在初始化和每次路由切换之前vue底层进行调用，可以使用该守卫完成页面重定向，权限验证等操作
  	语法(在路由匹配成功时调用)
  		router.beforeEach((to,from,next)={}) ,第一个形参to为需要去往的路由，第二个形参from为从哪个路由出发的，第三个形参next为进行路由放行(即执行本次路由跳转)
  	
  	注意
  		meta对象被称为路由源信息
  		在进行权限控制的时候，可以给需要添加权限年验证的路由添加一个特殊的属性，并放到该路由的meta对象下，此时只用判断每个路由mete对象下，是否包含需要权限控制的属性即可 
  	
      
   后置路由守卫
   	介绍
   		该方法在每次路由切换之后被vue底层进行调用,可以在该路由中书写路由切换后的配置代码，如存储用户token等信息
   	语法(在渲染了路由组件之前调用)
   		router.afterEach((to,from)=>{}),第一个形参to为需要去往的路由.第二个形参from为从哪个路由出发的
   
   前置&后置路由使用
   	const router = new VueROuter({})
   	router.beforeEach((to,from,next)=>{ // 前置路由守卫
          console.log(to,from);
          next();
      });
      router.afterEach((to,from)=>{ // 后置路由守卫
          console.log(to,from);
      });
   
    独享路由守卫
    	介绍
    		独享路由守卫，即只给某个需要权限限制的路由下专门配置路由守卫,在配置了此路由守卫的路由被访问之前，该路由守卫就会被调用，进行权限判断
    	语法(在路由匹配成功时调用)
    		beforeEnter(to,from,next){} // 在进入该路由时调用，必须使用next()放行，该路由组件才会渲染
    	使用
      	 routes:[
              {
                  path: "/first",
                  component: First,
                  beforeEnter(to,from,next){
                      console.log(to,from);
                  }
              }
          ]
    		
    路由组件内路由守卫
    	介绍
    		组件内路由守卫，即在路由组件内定义的路由守卫
    	语法
    		beforeRouteEnter(to,from,next){} // 当通过路由，渲染该路由组件时调用 
    		beforeRouteLeave(to,from,next){} // 当通过路由，离开该路由组件时调用
    	使用
    		{
              name:"t",
              beforeRouteEnter(to,from,next){},
              beforeRouteLeave(to,from,next){}
    		}
  */
  ```

  

+ history模式和hash模式

  ```java
  /*
  前言
  	如果为history模式，则后端需要对路由进行一个区分(区分开前端路由和后端路由，可下载对应包进行处理)，如果后端不想区分路由则可以使用hash模式(hash模式不需要后端区分路由)
   介绍
   	vue-router 默认为hash模式，hash模式的路径以 /#/分隔，#前的路径在请求时，会发送完整路径到后端，#后的路径则不会发送(通常当作一个前端路径存在); 而 history模式的路径在向后端发起请求后，全部路径都会发送给后端(在项目上线后，点击路由，然后进行刷新，则默认向后端发起请求，此时会出现故障(后端并没有此路由)，因此需要特殊处理，要么进行路由匹配，要么使用中间件)
   语法
   	new VueRouter({mode:"history"}) // 切换为 hisotry模式
   	new VueRouter({mode:"hash"}) // 切换为 hash模式
   	
   node.js环境下，解决history请求路径问题
  	安装中间件
  		npm i connect-history-api-fallback
  	使用看下图
   注意
   	当前端使用history模式的前端路由向后端部署页面时，可以使用nginx或中间件来进行路由区分
   	解决history模式的路径问题需要后端俩解决，或者前端使用hash模式
  */
  ```
  
  ![image-20220529111032365](D:\typora_import_images\typora-user-images\image-20220529111032365.png)





## UI组件库

高度定制化的页面不适用于组件库

![image-20220529111251381](D:\typora_import_images\typora-user-images\image-20220529111251381.png)



## 常用库

+ mock.js

  ```java
  /*
   介绍
  	mockjs可以拦截ajax请求，并随机生成一个指定范围的返回数据
  	
   安装
   	npm i mockjs
   使用
   	// 拦截所有 向 /info 路径发起请求的接口(不受请求类型限制)
      Mock.mock("/info", {data: {status: 0, data: data.info}}); // 拦截后将此模板对象作为响应返回
  
      // 为函数时，接收一个opts配置对象 ，该配置对象有本次拦截的请求类型,地址，和body参数{body,type,url}
      Mock.mock("/goods", (opts) => { // 拦截 /goods 路由的请求，并调用此函数
          console.log(opts);
          return {data: {status: 0, data: data.goods}} // 返回对应数据
      });
  
      // 拦截向 /ratings 路由发起POST请求的接口,并返回该模板对象
      Mock.mock("/ratings", "POST", {data: {status: 0, data: data.ratings}});
   	
   注意
   	mockjs只能拦截ajax请求，只会访问对应的路由时获取不到数据
   	自己编写mock测试数据可以参考官网的示例
  */
  ```

  



## 移动端项目

项目说明

![image-20220529151344769](D:\typora_import_images\typora-user-images\image-20220529151344769.png)

![image-20220529151958867](D:\typora_import_images\typora-user-images\image-20220529151958867.png)

![image-20220529152144651](D:\typora_import_images\typora-user-images\image-20220529152144651.png)



![image-20220529153329089](D:\typora_import_images\typora-user-images\image-20220529153329089.png)



![image-20220529153955476](D:\typora_import_images\typora-user-images\image-20220529153955476.png)

+ api接口

  ```java
  /*
   api 指 接口信息，我们调用指定接口得到一系列返回数据
   接口文档 指 所有api的集合，一般包括，请求方式，请求路径，携带参数，响应结果
   对接口 指 测试每个接口，如有问题通知后端处理
  */
  ```



+ 移动端适配

  ```java
  /*
   百分比布局
   rem布局
   	rem是一个相对单位，rem为相对html标签的font-size大小的一个属性，如果html的font-size是50px，则1rem就是50px,我们通过将html的font-size修改为当前屏幕的1/10 来完成rem布局(即1rem也占屏幕1/10)
   响应式布局(媒体查询)
   	使用媒体查询监控当前页面的变化来达到像素控制目的
   viewport布局
     vw,vh把当前视图分为了100份，1vh或1vw就占1%
     
  viewport适配配置
  	安装包
  		npm install postcss-px-to-viewport --save-dev
  	配置
  		在项目根目录下创建 postcss.config.js
  		
          module.exports = {
            plugins: {
              'postcss-px-to-viewport': {
               unitToConvert: "px", // 要转化的单位       
               viewportWidth: 375, // UI设计稿的宽度       
               unitPrecision: 6, // 转换后的精度，即小数点位数       
               propList: ["*"], // 指定转换的css属性的单位，*代表全部css属性的单位都进行转换     
               viewportUnit: "vw", // 指定需要转换成的视窗单位，默认vw       
               fontViewportUnit: "vw", // 指定字体需要转换成的视窗单位，默认vw      selectorBlackList: ["wrap"], // 指定不转换为视窗单位的类名，       
               minPixelValue: 1, // 默认值1，小于或等于1px则不进行转换       
               mediaQuery: true, // 是否在媒体查询的css代码中也进行转换，默认false      
               replace: true, // 是否转换后直接更换属性值       
               exclude: [/node_modules/], // 设置忽略文件，用正则做目录名匹配       
              }
            }
          }
  	
  	处理三方组件库的兼容性问题
  		const path = require('path');
   
          module.exports = ({ webpack }) => {
            const viewWidth = webpack.resourcePath.includes(path.join('node_modules', 'vant')) ? 375 : 750;
            return {
              plugins: {
                autoprefixer: {},
                "postcss-px-to-viewport": {
                  unitToConvert: "px",
                  viewportWidth: viewWidth,
                  unitPrecision: 6,
                  propList: ["*"],
                  viewportUnit: "vw",
                  fontViewportUnit: "vw",
                  selectorBlackList: [],
                  minPixelValue: 1,
                  mediaQuery: true,
                  exclude: [],
                  landscape: false
                }
              }
            }
          }
  
  
   注意
   	常用适配方案 ，rem适配，viewport适配
   	在使用viewport适配时，只会给以类方式引入的样式才能进行viewport适配，行内的样式则不会
  */
  ```
  
  
  
  ![image-20220529164235951](D:\typora_import_images\typora-user-images\image-20220529164235951.png)
  
  ![image-20220529163937599](D:\typora_import_images\typora-user-images\image-20220529163937599.png)
  
  ![image-20220529165548542](D:\typora_import_images\typora-user-images\image-20220529165548542.png)
  
  > <meta name="viewport"
  >       content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
  
  

 

+ 解决webpack别名在webstorm中不提示问题

  ```java
  /*
  在项目根目录下创建一个 jsconfig.json 配置如下代码即可
   {
    "compilerOptions": {
      "target": "es6",
      "baseUrl": ".",
      "paths": {
        "@/*": ["src/*"]
      }
    },
    "exclude": ["node_modules", "dist"],
    "include": ["src/**/*"]
  }
  
  */
  ```

  

+ vue使用swiper达到轮播图效果

  ```java
  /*
   安装
   	注意：vue2使用swiper只能使用一下指定版本
   	npm i swiper@5
   	npm i vue-awesome-swiper@4
   	
   配置项
   <template>
      <div class="app-container">
          <!--主体为swiper标签-->
          <!--属性 :options 绑定的是data中设置的swiper配置项-->
          <!--属性 ref 用于获取该dom元素，在计算属性computed中将被调用-->
          <!--属性 id 为swiper最外层容器设置css样式-->
          <swiper :options="swiperOption" ref="mySwiper" id="mySwiper">
  
              <!--必须的组件-->
              <!--每页幻灯片使用swiper-slide标签-->
              <!--幻灯片组件生成的标签自带.swiper-slide的类名，但单类名选择器设置的部分css(如宽高)将被覆盖-->
              <swiper-slide class="swiper_slide_item">I'm Slide 1</swiper-slide>
              <swiper-slide class="swiper_slide_item">I'm Slide 2</swiper-slide>
              <swiper-slide class="swiper_slide_item">I'm Slide 3</swiper-slide>
  
              <!-- 可选的控件 -->
              <!--分页器-->
              <div class="swiper-pagination" slot="pagination"></div>
              <!--滚动条-->
              <div class="swiper-scrollbar" slot="scrollbar"></div>
              <!--前进后退按钮-->
              <div class="swiper-button-prev" slot="button-prev"></div>
              <div class="swiper-button-next" slot="button-next"></div>
          </swiper>
  
          <!--配置自定义的页面跳转按钮，to(page)为自定义方法，其内调用了swiper的内置方法-->
          <button @click="to(1)">点击跳转到Slide 1</button>
          <button @click="to(2)">点击跳转到Slide 2</button>
          <button @click="to(3)">点击跳转到Slide 3</button>
  
      </div>
  </template>
  
  <script>
      export default {
          name: "SwiperTest",
          data() {
              return {
                  //swiperOption：swiper配置项信息，需要绑定在swiper标签的 :option 属性中
                  swiperOption: {
  
                      //幻灯片放映方向
                      direction: 'vertical'|'horizontal', //通常不与左右按钮一同使用
  
                      //分页器配置项
                      pagination: {
                          el: ".swiper-pagination", //分页器的类名
                          clickable: true, // 点击分页器跳切换到相应的幻灯片
                          type: 'bullets' | 'progressbar' | 'fraction' , //小圆点|进度条|数字页码
                          dynamicBullets: true,  //动态小圆点(type:'bullets'时)
                          //自定义分页器，需设置样式
                          renderBullet(index, className) {
                              return `<span class="${className} swiper-pagination-bullet-custom">${index + 1}</span>`
                          },
                      },
  
                      //前进后退按钮,样式
                      navigation: {
                          nextEl: '.swiper-button-next',
                          prevEl: '.swiper-button-prev'
                      },
  
                      //滚动条
                      scrollbar: {
                          el: '.swiper-scrollbar',
                          hide: true
                      },
  
                      //幻灯片播放配置项
                      loop: true, //是否循环播放
                      speed: 1000, // 发生页面切换动画时，动画的切换速度
                      autoplay: {
                          delay: 2000, // 幻灯片停留时间
                          disableOnInteraction: false, // 用户操作swiper之后，是否禁止autoplay
                          stopOnLastSlide: true, // 切换到最后一个slide时是否停止自动切换。（loop模式下无效）。
                      },
                      on: {
                          slideChangeTransitionEnd: function () {
                              console.log(this.activeIndex); //每次切换结束时，在控制台打印现在是第几个slide
                          },
                      },
                  },
              };
          },
  
          //计算属性
          computed: {
              swiper() {
                  return this.$refs.mySwiper.$swiper;
              },
          },
  
          //设置自定义的按钮，点击后将前往目标也页
          methods: {
              to(index) {
                  this.swiper.slideTo(index)
                  console.log(this.swiper);
              }
          }
      };
  </script>
  <style>
    
      #mySwiper{
          width: 500px;
          height: 100px;
          background-color: aquamarine;
      }
  </style>
  */
  ```

  



+ 移动端点击300ms延迟问题

  ```java
  /*
   介绍
   	300ms延迟是为了应对移动端访问pc端界面的一个约定(在10年时大多网页都没有进行移动端适配，因此需要双击缩放)，当进行屏幕点击时，会有一个300ms的延迟(判断此时用户是否双击界面实现缩放，而如今我们会编写移动端界面，因此这个延迟是不需要的)
   解决方法
   	使用 touch事件替换click事件
   	使用 meta设置解决
   		 <!-- 1.禁用缩放 user-scalable=no -->
      <meta name="viewport" content="width=device-width, initial-scale=1.0,user-scalable=no">
  
   	使用 fastclick库 
   fastclick库原理
   	在thouchend事件中分发一个自定义的click事件，阻止浏览器300ms后分发的click事件
   注意
   	300ms延迟只对click事件有影响(需要判断用户是否执行双击屏幕操作(实现屏幕缩放))
  */
  ```
  
+ better-scroll

  ```java
  /*
   介绍
   	该库可以实现页面优雅的滑动(过渡效果),在使用插件后还可以实现轮播，动态加载资源等功能
   安装
   	npm install @better-scroll/core --save  // 下载核心包
   常用属性和方法
   	属性(配置)
   		probeType 在滑动中运行swiper触发scroll事件
   			probeType: 3 // 表示触发所有派发scroll事件
   		click 当为true时，表示运行向浏览器触发click事件(默认被禁止)
   
   注意
   	在实例化传入一个虚拟dom节点后(该虚拟dom必须有固定的宽高不能随内容撑开，否则无法作为一个包裹盒子，即无法完成滑动)，该节点的第一个子字节会升级为滑动块的内容
   踩坑1
   	better-scroll 会将指定元素(该元素必须有固定的高度或宽度，不能为100%)的第一个直接子节点包装为一个滑动块，因此如果需要使用一个包裹元素来包裹需要滑动的元素
   	如
   		 <div class="food-list" ref="scroll"> // 此时 .food-list 需要有固定的高度或宽度
   		 		// 将div.food-wrapper 包装为一个滑动块
                  <div class="food-wrapper">
                      <div class="category" v-for="(_,i) in 8">{{foodList[i].name}}
                          <Food v-for="(food,index) in foodList[i].foods" :key="index"
                                :name="food.name" :desc="food.description" :image="food.image"
                                :price="food.price" :rating="food.rating" :sell="food.sellCount"/>
                      </div>
                  </div>
              </div>
  
   踩坑2
   	better-scroll 设置的动画效果最好在所有数据获取并加载完毕后在进行挂载，可以配合$nextTick()钩子函数和watch完成
  
   踩坑3
   	所有容器相关的样式要求可参考官网示例，主要设置ref的样式和被展示的资源的样式
  */
  ```
  
  



## 项目编写技巧

+ 使用 jsconfig.json配置全局路径

  ```java
  /*
   介绍
   	此方式配置全局路径后，可以在任何位置使用该别名获取对应的路径
   创建
   	在项目根目录下(和src，package.json灯同级目录)创建 jsconfig.json 文件，并写入如下配置
   		{
            "compilerOptions": {
              "baseUrl": "./",
              "paths": {
                "@/*": [
                  "src/*"
                ]
              }
            },
            "exclude": [
              "node_modules",
              "dist"
            ]
          }
  */
  ```

  

+ vite+vue3+ts省略后缀

  ```java
  /*
   vite.config.ts
   	import {defineConfig} from "vite";
      import vue from "@vitejs/plugin-vue";
      import {join} from "path";
      // https://vitejs.dev/config/
      export default defineConfig({
          resolve: {
              alias: { // 配置文件别名
                  '@': join(__dirname, "src"),
              }
          },
          plugins: [
              vue()
          ],
      });
  
   tsconfig.json 中添加如下配置
   {
   	...
   	"compilerOptions":{
   		... // 省略其他compiler配置
   		   "baseUrl": ".",
              "paths": {
                "@/*": [
                  "src/*"
                ]
              }
   	}
   	...
   }
  */
  ```

  

## vue3

+ vue3 vs react

  ```java
  /*
   1. 在状态方面
   	vue
   		vue3状态修改后，会重新调用所有get方法重新获得数据并进行虚拟dom树对比渲染有差异的部分(非状态数据只会在初始状态时显示一次，之后它们的数据改变后页面也不会进行呈现，只会状态进行更新)
    	react
    		状态修改后，类组件重新走一遍更新时的生命周期函数，函数组件为重新执行一次本函数内容，并进行前后虚拟dom树对比渲染有差异的部分
  */
  ```

  

+ 集合

  ```java
  /*
   vue2 和vue3开发工具互不兼容，需要独立下载
   vue3 兼容 vue2的所有写法，但推荐使用vue3的写法(不要使用vue2的写法，在setup方法中无法获取到这些属性和方法)
  */
  ```

  

+ vue3优势

  ```java
  /*
   1. 打包速度，效率得到了极大提升
   2. 源码升级，使用proxy代替defineproperty实现响应式，重写了diff算法，使其更高效。适配ts编写
   3. 新特性， componentApi
  */
  ```

  

+ vue3项目创建方式

  使用 vue create app 选择vue3构建

  使用 vite 构建

  vite优势：按需加载使用的组件，webpack未全部打包后在呈现给服务器

  ![image-20220616113111177](D:\typora_import_images\typora-user-images\image-20220616113111177.png)

  ![image-20220616113342978](D:\typora_import_images\typora-user-images\image-20220616113342978.png)



+ vue3 main.js和vue2 main.js的差异

  ```java
  /*
  	vue2 中 main.js需要我们自己实例化一个Vue的示例对象，然后通过render函数解析虚拟dom挂载到对于的真实dom上(太重了，为了一个资源挂载导致创建了一个过大的实例对象)
  	vue3 中 main.js则使用一个其封装的工厂函数，我们调用该工厂函数，传入组件或虚拟dom即可返回一个轻量的对象(该对象上有一系列常用的vue的属性和方法)，然后使用mount方法完成挂载
  	vue3 中 组件可以没有根标签，但vue2则必须有
  	
   注意
   	在使用 vue3插件时
  *
  ```

### composition api

+ 集合

  ```java
  /*
   1. vue3的组合api实际上是对vue2中繁重的配置项进行了解耦，做了到了需要什么属性则调用对应的api进行包装(效果一致)，有效的减轻了打包体积
   2. 组合api 只能在setup方法中使用
   3. 一个由ref生成的引用代理对象，在script中使用时，需要使用.value获得其代理的value属性，在template中则直接使用该引用代理对象即可，vue底层会取获取该ref引用对象的value属性
  */
  ```
  
  

+ setup

  ```java
  /*
   介绍
   	setup是vue3中一个新的配置项，值为一个函数，是所有配置项的一个集合(data(state),methods等)，在其中书写的属性和方法经过返回后会被转换为 状态(data)和方法
   	使用
   		// 返回一个对象时，暴露的所有属性会被包装成状态，暴露的所有方法会包装成方法
   		export default {
                name: 'App',
                components:{
                  // HelloWorld,
                },
                setup(){
                  let name = "张三";
                  let age = 19;
                  const show = ()=>{
                    return `${name},${age}`;
                  }
                  return {name,age,show};
                }
              }
  		
          // 返回一个函数时，则该组件实例渲染的内容以该函数的返回内容为准
           import {h} from "vue";
          export default {
              name: 'App',
              setup() {
                  let name = "张三";
                  let age = 19;
                  const show = () => {
                      return `${name},${age}`;
                  }
                  return () => h('h1', `${name},${age}`);
              }
  
          }
  注意
  	当setup方法返回的是一个函数时，则默认为模板函数，最终该组件显示的内容以该模板返回的为准
  	setup函数在beforeCreate之前执行，且this指向为undefined
  */
  ```

  ![image-20220616172128045](D:\typora_import_images\typora-user-images\image-20220616172128045.png)

  ![image-20220617120308856](D:\typora_import_images\typora-user-images\image-20220617120308856.png)

  父组件给子组件绑定自定义事件时，在vue3中，需要使用emits:[] 配置项配置接收的自定义事件，否则会出现警告，不影响使用



+ ref函数

  ```java
  /*
   介绍
   	ref函数是vue3新增的一个函数，和$ref收集的虚拟dom属性相同名但不冲突。ref可以将一个变量包装成一个状态，在调用了该函数后返回一个引用对象（refImpl ,全称为 reference implement）,该引用对象对该值进行了一个代理(和观察者模式的包装)，当改变其值后，页面会进行重绘
   	
   使用
      <template>
          <div>我叫{{name}}</div>
          <div>我{{age}}岁了</div>
          <button @click="change">改变</button>
      </template>
  
      <script>
          import { ref} from "vue";
  
          export default {
              name: 'App',
              setup() {
              	// 对 name,age进行包装，包装成一个引用对象，并进行观察者模式监听
                  let name = ref("张三");
                  let age = ref(19);
                  const show = () => {
                      return `${name},${age}`;
                  }
                  const change = () => {
                  	// 调用该对象的代理属性的set方法进行状态改变，从而重绘页面
                      name.value = "李四";
                      age.value = 10;
                  }
                  return {name, age, show, change};
              }
  
          }
      </script>
  
   注意
   	setup中使用 ref函数完成状态包装，
   	ref包装的数据为引用对象，在虚拟dom中使用时可以直接使用该对象，vue3底层会拿到该引用对象的value属性
   	vue3中的ref对不同数据做不同的状态包装，当为基础数据类型时，使用数据劫持+代理完成(状态维护)页面重绘，当为复杂数据类型时，使用es6新特性 proxy完成数据代理及页面重绘
   	复杂数据类型和基本数据类型都可以使用ref进行包装，升级为状态
  */
  ```



+ reactive

  ```java
  /*
   介绍
   	reactive用来快速将一个复杂数据类型包装成一个状态(进行代理包装)，并且可以无需像ref包装一样，使用value属性来获取代理，状态的改变更加方便
   使用
   	<template>
          <ul>
              <li v-for="i in hobby">
                  {{i}}
              </li>
  
          </ul>
  
          <ul>
              <li v-for="(val,key) in behavior">
                  {{key}} --- {{val}}
              </li>
          </ul>
          <button @click="change">改变</button>
      </template>
  
      <script>
          import {ref, reactive} from "vue";
  
          export default {
              name: 'App',
              setup() {
                  let hobby = reactive(["basketball", "football"]);
                  let behavior = reactive({
                      like: "1",
                      we: "3"
                  });
                  const change = () => {
                      hobby[0] = "soccer";
                      behavior.like = "study";
                  }
                  return {change, hobby, behavior};
              }
  
          }
      </script>
   注意
   	reactive 只能将复杂数据类型包装成一个状态
   	reactive 对于复杂数据类型包装状态的处理要优于ref,因为其直接使用的proxy代理包赚状态，而ref使用数据绑定+代理，获取及修改状态比较复杂
  */
  ```

  ![image-20220616203710070](D:\typora_import_images\typora-user-images\image-20220616203710070.png)



+ reactive vs ref

  ![image-20220617104842504](D:\typora_import_images\typora-user-images\image-20220617104842504.png)



+ vue2绑定状态 vs vue3 绑定状态

  ```java
  /*
   vu2 使用 数据劫持+数据代理完成状态绑定
   	当我们使用 一个状态后，实际上时调用了get方法其返回了原有的一个状态，当我们修改一个状态时，实际上时调用了set方法修改了状态，并且vue在其中进行了一次页面重回
   	如果修改了一个没有set方法的数据则不会页面重绘
   	vue 只会都某个属性进行数据劫持，如果是一个数组时，无法通过改变索引的原型来达到页面重绘(因为其没有绑定set方法，对象中的属性则绑定了set方法)，所以对数组进行修改时，要么使用vue封装过的push,splice等操作数组的方法(其的封装主要做了页面重绘)，要么就实现浅拷贝然后传入一个新数组
  
  vue3 使用 Proxy+Reflect和数据劫持来实现状态绑定
  	介绍
  		Proxy和Reflect是es6提出的新特性,Proxy它可以直接代理一个复杂数据类型，当使用代理操作原数据时，对其的修改进行拦截监视；Reflect则可以快速映射一个对象的地址，并对其属性进行修改或获取某个属性
  	使用
  		const {log} = console;
      	 const person = {
            name: "张三",
            age: 13
          };
          // target为代理的目标对象，propsName为操作的属性，value为改变的值，以上对于的函数形参上的代表值，可以为任何数据，但获取值的位置不能发生改变
          const p = new Proxy(person, {
            // 使用该代理获取一个目标对象(包括数组)属性时会被该代理实例对象拦截，并调用get方法返回数据
            get(target, propsName) {
              log(target, propsName);
              return Reflect.get(target,propsName); // 得到目标对象中的一个属性
  
            },
            // 通过该代理删除一个属性时调用
            set(target, propsName, value) {
            	log();
              log(target, propsName, value);
              Reflect.set(target,propsName,value); // 修改目标对象中的属性
            },
            // 通过该代理删除一个属性时调用
            deleteProperty(target, propsName) {
              // delete 关键字可以删除一个数据，并返回一个布尔值(表示是否删除成功)
              return Reflect.deletePropty(target,propsName); // 删除目标对象中的属性
            }
  
          });
  	注意        
  		proxy代理实现了对一个复杂数据类型的深度监视，即只要其发生修改，vue就能检测到，并且进行页面重绘
  		proxy代理是一个增强版的Object.defineProperty,它可以完成一个目标对象的代理，当通过代理改变数据时，通过配置项的get,set方法等完成操作，并作出反馈（如在set方法中进行原状态修改并进行页面重绘）
  */
  ```



+ computed

  ```java
  /*
   介绍
   	vue3的computed和vue2的computed写法基本一致，只不过vue3在vue2组件实例的基础上对每个功能进行了解耦，如果需要某个属性则需要引入对于的api然后调用
   
   使用
   	<template>
          <ul>
              <li v-for="i in hobby">
                  {{i}}
              </li>
          </ul>
          <ul>
              <li v-for="(val,key) in behavior">
                  {{key}} --- {{val}}
              </li>
          </ul>
          <button @click="change">改变</button>
      </template>
  
      <script>
          import {reactive,computed} from "vue";
  
          export default {
              name: 'App',
              setup() {
                  let hobby = reactive(["basketball", "football"]);
                  let behavior = reactive({
                      like: "1",
                      we: "3"
                  });
                  // 创建了一个计算属性
                  let yourLike = computed(()=> behavior.like +"_"+behavior.we);
                  const change = () => {
                      hobby[0] = "soccer";
                      behavior.like = "study";
                  }
                  // 将其挂载到状态中，一遍页面重绘
                  behavior.yourLike = yourLike;
                  return {change, hobby, behavior};
              }
  
          }
      </script>
  
  注意
  	使用computed包装后的计算属性，返回的是一个 ComputedRefImpl 包装对象，在使用该计算属性时就调用get方法获得数据
  	computed包装后的属性最终需挂载到状态中
  	vue3和vue2computed属性写法一致(完整配置为一个Object.definedProperty对象，简单配置为一个函数，vue底层会将其转换为该对象的get方法)，只是做了解耦，提高了效率
  */
  ```



+ watch

  ```java
  /*
   介绍
   	vue2和vue3的watch作用和方法一致，就是配置方法不同，vue3使用函数完成配置
   语法
   	import {watch} from "vue";
   	watch(target,()=>{},{}); 第一个参数为监听的状态，第二个参数为状态改变后执行的回调，第三个参数为携带的配置项，如 {deep: true,immediate:true} 
   
   使用
   	<template>
          <h2>{{count}}</h2>
          <button @click="count+=1">+1</button>
          <ul>
              <li v-for="(val,key) in behavior">
                  {{key}} --- {{val}}
              </li>
          </ul>
          <button @click="change">改变</button>
      </template>
  
      <script>
          import {reactive,computed,watch,ref} from "vue";
  
          export default {
              name: 'App',
              setup() {
                  let hobby = reactive(["basketball", "football"]);
                  let behavior = reactive({
                      like: "1",
                      we: "3"
                  });
                  let count = ref(0);
                  let yourLike = computed(()=> behavior.like +"_"+behavior.we);
                  const change = () => {
                      hobby[0] = "soccer";
                      behavior.like = "study";
                  }
                  watch(behavior,(newV,oldV)=>{
                      console.log(newV,oldV);
                  },{immediate:true});
                  watch(count,(newV,oldV)=>{
                      console.log(newV,oldV);
                  },{immediate:true});
                  return {change, hobby, behavior,count};
              }
  
          }
      </script>
   注意
   	watch如果需要监听多个状态可以使用多个watch 1对1编写；也可以使用一个watch一对多编写，此时需要使用数组存储，在其中一个状态改变后，watch回调就会执行，其中第一个参数为当前全部状态的更新后的状态，第二个参数为当前全部状态的更新前的状态
   	watch在监视Proxy实例对象时，newValue和oldValue都为最新的值(后期可能会优化？)，且默认为深度监视，无法关闭；监听简单数据类型则没有此问题
   	如果需要监听Proxy实例对象的某个属性时(该数据是一个状态)，第一个参数需要写成一个函数，然后函数的返回值返回需要监听的状态来完成监听，此时watch会去调用这个函数拿到监视目标，(如果有多个则写成一个数组，其中一个状态改变时，就会调用形参2，并把改变前后的状态数组传入)。如果以此方式监听的是一个复杂数据类型，则需要配置 {deep:true}
   	
  
  watchEffect函数
  	介绍
  		该函数可以完成状态的按需监听，哪个状态需要监听则写入该函数中即可，当该状态改变后，对应使用该状态的watchEffect 就会执行
  	使用
      	<template>
      <ul>
          <li v-for="i in hobby">
              {{i}}
          </li>
  
      </ul>
      <h2>{{count}}</h2>
      <button @click="count+=1">+1</button>
      <ul>
          <li v-for="(val,key) in behavior">
              {{key}} --- {{val}}
          </li>
      </ul>
      <button @click="change">改变</button>
  </template>
  
  <script>
      import {reactive,computed,watch,ref,watchEffect} from "vue";
  
      export default {
          name: 'App',
          setup() {
              let hobby = reactive(["basketball", "football"]);
              let behavior = reactive({
                  like: "1",
                  we: "3"
              });
              let count = ref(0);
              let yourLike = computed(()=> behavior.like +"_"+behavior.we);
              const change = () => {
                  hobby[0] = "soccer";
                  behavior.like = "study";
              }
              watchEffect(()=>{ // 当count 改变后，该函数则执行
                  let a = count.value;
                  console.log("count改变了");
              })
              watchEffect(()=>{ // 当  behavior.like 改变后该函数则执行
                  let b = behavior.like;
                  console.log("behavior.like改变了");
              })
              behavior.yourLike = yourLike;
              return {change, hobby, behavior,count};
          }
  
      }
  </script>
  
  */
  ```





+ vue3 vs vue2 (生命周期函数)

  ```java
  /*
   介绍
   	vue3 生命周期函数 在 vue2基础上做了完善，使用了 beforeUnmount() 和 unmount() 去代替了 beforeDestroy() 和 destroy() ,(进行了改名)
   	vue3 生命周期函数在 beforeCreate() 和 created()阶段和 vue2不同，vue2为只要你创建了一个vue组件实例，则会进行beforeCreate() 和 created() 初始化，在判断是否有挂载节点，如果没有则进行等待 。 而vue3在创建实例的时候需要有挂载节点才会进行生命周期函数调用
  	在vue3使用生命周期函数的时候可以当作方法写在配置项外层，也可以当作一个组合api 写在 setup 方法里，对应关系如下 
  */
  ```

  ![image-20220617175552145](D:\typora_import_images\typora-user-images\image-20220617175552145.png)





+ 自定义钩子函数

  ```java
  /*
   介绍
   	自定义钩子函数和react钩子函数概念相似，语法为 use函数名() 完成调用并获取钩子函数返回值，达到了代码复用和程序解耦目的
   
   注意
   	自定义钩子函数只能在setup中使用，且该函数中编写的都是组合 api
  */
  ```

  使用![image-20220617182114381](D:\typora_import_images\typora-user-images\image-20220617182114381.png)

  ![image-20220617182204366](D:\typora_import_images\typora-user-images\image-20220617182204366.png)

  



+ toRef& toRefs

  ```java
  /*
   介绍
   	toRef和toRefs可以将一个从复杂数据类型里读取出来的所有属性进行一次数据代理(不使用toRefs则为一个普通的地址)，且该代理操作的还是原复杂数据类型里的数据(相当做了一个桥接)
   语法
   	toRef(obj,"name"); //  获取该对象下的name属性，并做了一个(ref)数据代理，返回引用对象到该行
  	toRefs(obj); // 将该对象下的所有属性进行(ref)数据代理,返回引用对象到该行
   
   使用
   	<template>
          <ul>
              <li v-for="i in hobby">
                  {{i}}
              </li>
  
          </ul>
          <h2>{{count}}</h2>
          <button @click="count+=1">+1</button>
          <ul>
              <li v-for="(val,key) in behavior">
                  {{key}} --- {{val}}
              </li>
          </ul>
          <button @click="change">改变</button>
          {{like}} -- {{we}} --- {{job.salary}}
          <button @click="like = we = job.salary = 1">data</button>
          {{behavior}}
      </template>
  
      <script>
          import {reactive, computed, toRefs, ref} from "vue";
  
          export default {
              name: 'App',
              setup() {
                  let hobby = reactive(["basketball", "football"]);
                  let behavior = reactive({
                      like: "1",
                      we: "3",
                      job: {
                          salary: 4
                      }
                  });
                  let count = ref(0);
                  let yourLike = computed(() => behavior.like + "_" + behavior.we);
                  const change = () => {
                      hobby[0] = "soccer";
                      behavior.like = "study";
                  }
                  behavior.yourLike = yourLike;
                  console.log(behavior);
                  return {change, hobby, ...toRefs(behavior), count,behavior};
              }
  
          }
      </script>
  
   注意
   	toRefs会将该Proxy对象下的最外层属性包装成一个ref引用对象，在操作这些属性时，实际上实在操作Proxy代理的复杂数据类型数据
   	ref也可以将一个对象下的属性包装成引用对象，但此方式是读取一个属性的值后，在进行引用包装，无法对原对象里的属性产生修改(toRefs是保留原对象的映射在进行引用包装，当其改变后是改变的原对象里的属性)
  */ 
  ```



+ shallowRef & shallowReactive

  ```java
  /*
   介绍
   	shallowRef和shallowReactive只会将当前最外层的数据包装成一个引用对象(状态)，而不会提柜去包装其内部属性，ref和reactive则会进行深度包装(引用对象)
   注意
   	使用shallowRef包装一个数据时，只会对简单数据类型做一个引用包装，复杂类型则不会(ref包装复杂数据类型使用数据代理+proxy，而proxy属于第二层因此不会进行包装，但可以通过修改地址指向来控制页面重绘，因为包装了第一层状态)
  */
  ```

+ readonly & shallowReadOnly

  ```java
  /*
   介绍
   	将一个地址包装成一个只读状态，即只能读取不能修改， readonly 对该地址中所有的属性包装成只读，readonly 只会对该地址的第一层属性包装成只读
  */
  ```

  ![image-20220618112108441](D:\typora_import_images\typora-user-images\image-20220618112108441.png)



+ toRaw & markRaw

  ```java
  /*
   介绍
   	可以将一个收到vue3 组合api包装过的引用对象转换为原来的数据
    注意
    	toRaw 组合api 只能转换由 proxy代理的引用对象(状态)
    	markRaw 可以标记一个地址为原始地址，当其被传入一个状态中时，不会被转换为状态，而是呈现默认地址中的值(默认会转换)
  */
  ```

  ![image-20220618113736318](D:\typora_import_images\typora-user-images\image-20220618113736318.png)



+ customRef

  ```java
  /*
   介绍
   	自定义ref可以帮助我们改变内部逻辑，实现扩展功能(使用较少？我们可以在外部进行逻辑校验？)，可以对依赖项跟踪和更新触发进行显示控制
      自定义ref可以让我们完成get(获取状态)和set(修改状态)的封装
   使用
   	  // 通过调用本函数获得一个自定义绑定的一个状态(ref)
   	  function myRef(value){
   	  			// 底层会调用我们传入的函数，获取状态返回规则
                  return customRef((track, trigger)=>({
                  	// 每次读取自定义ref调用的就是get方法获取数据
                      get(){
                        track(); // 只有使用了track函数，才能返回自定义状态
                        return value;
                      },
                      // 每次修改自定义ref状态，调用的就是set方法修改数据
                      set(newValue){
                          value = newValue;
                          // 提层也使用了trigger函数进行页面重绘？
                          trigger();// 只有使用了trigger页面才会进行重绘
                      }
                  }))
              }
   
  */
  ```



+ provide & inject

  ```java
  /*
   介绍
   	provide 和inject 可以实现 当前组件与该组件实例的任意包裹层级实现数据传递。当前组件使用provide 组合api , 需要使用数据方使用 inject组合api 声明
   	和react的context类似，但比context更简洁
   使用
   	// grandFather.vue
   	<template>
          <Father/>
      </template>
  
      <script>
          import {provide, reactive} from "vue";
          import Father from "./Father";
  
          export default {
              name: "GrandFather",
              components: {
                  Father
              },
              setup() {
                  const behavior = reactive({
                      purpose: "help me(grandFather) to coding"
                  });
                  provide("behavior", behavior); // 向被包裹组件提供begivor状态
                  return {
                      behavior
                  };
              }
          }
      </script>
      
      // Father.value
      <template>
          <Child/>
          <div>i am father</div>
      </template>
  
      <script>
          import Child from "./Child";
          export default {
              name: "Father",
              components:{
                  Child
              }
          }
      </script>
      
      // Child.vue
      <template>
          我是child
          {{behavior}}
      </template>
  
      <script>
          import {inject} from "vue";
  
          export default {
              name: "Child",
              setup() {
              	// 被包裹组件实例Child 使用提供的状态
                  const behavior = inject("behavior");
                  console.log("child====",behavior);
                  return {behavior};
              }
          }
      </script>
  */
  ```

  

+ 响应式数据判断

  ```java
  /*
   介绍
   	响应式数据判断也是一个 组合 api ，它们可以判断一个数据是否是由对应响应式对象生成的
  */
  ```

  ![image-20220618153319146](D:\typora_import_images\typora-user-images\image-20220618153319146.png)

  ![image-20220618153331609](D:\typora_import_images\typora-user-images\image-20220618153331609.png)



+ 组合api(需要按需引入) vs 配置api(vue2时写的配置)

  ```java
  /*
   vue2 虽然对每个配置实现了功能划分，但一旦功能点较多则会显得单个文件体积较大(不符合单一职责原则)，因此vue3 引入了comopistion api 对该现象进行了处理，通过配合 自定义hooks 来完成对应功能点的封装，然后使用方，直接使用该钩子获得返回值即可
  */
  ```



+ Fragment

  ```java
  /*
   介绍
   	在vue中，组件必须有一个根标签，但在vue3中，组件可以没有跟变迁，内部会将多个标签包含在一个Fragment虚拟元素中，这样减少了标签层级和内存占用
  */
  ```



+ teleport

  ```java
  /*
   介绍
   	teleport 可以选择一个组件实例或一个虚拟dom载入的位置，其有一个属性to(to的值为一个dom标签，为插入的位置),指定后，该组件渲染时，就会将其插入倒该标签内
   使用
   	 <teleport to="body"> 我来插入倒#app中</teleport> // teleport 包裹的内容会被插入到body标签下
   注意
   	to 的值 只能是一个dom标签，可以使用js的选择器进行选择，如 .app ,#root,body
  */
  ```



+ suspense

  ```java
  /*
   介绍
   	vue3 支持组件异步引入，即支持先加载完的组件实例先展示,后加载的组件后展示(vue2不能，必须等到所有的组件加载完毕才能显示), 和react按需加载类似，在一个组件还未加载完毕时，显示fallback里的内容，加载完后显示该组件(vue为显示default插槽里的内容)
   语法
   	import {defineAsyncComponent} from "vue"
   	defineAsyncComponent(()=>import("./Child")) // 异步引入Child组件
   细节
   	在使用动态加载(异步引入)时，通常还配合Suspense组件实例一次使用，在该组件还未加载时，Suspense会进行一个过渡效果展示
   	Suspense组件内容为两个插槽，需要异步加载的组件放到default插槽内，异步组件的过渡效果则放入fallback插槽内
  */
  ```

  ![image-20220618163127247](D:\typora_import_images\typora-user-images\image-20220618163127247.png)

  







+ vue全局api转移

  ![image-20220618164005235](D:\typora_import_images\typora-user-images\image-20220618164005235.png)

  

  

+ vue其他修改

  移除了键盘事件的自定义修饰符

  

  ![image-20220618164855204](D:\typora_import_images\typora-user-images\image-20220618164855204.png)

  移除了native修饰符，vue3现在需要声明一个组件可以接收的自定义事件，没有声明的事件默认当原生事件处理![image-20220618165120541](D:\typora_import_images\typora-user-images\image-20220618165120541.png)

  移除了过滤器

  
  
  
  
  

+ 使用nextTick()钩子函数

  ```java
  /*
   介绍 
   	功能和vue2中的nextTick相同，只是组合api中采用了按需引入方式
   使用
   	import {nextTick} from "vue";
  */
  ```

  



#### setup语法糖

+ 注意

  ```java
  /*
   1. 使用setup语法糖时，可以使用await关键字(vue底层会自适应判断，在适当位置添加async)
   2. 在template虚拟dom中，使用的ref和reactive包装的状态 会默认获取其包装的值，如ref包装一个简单数据类型时，则获取的是value值，包装的为复杂数据类型时，则获取的是value下的proxy代理的target值
   3. 在直接操作一个proxy对象时，实际上是在操作其代理的值，可以使用其值的属性和方法
   4. vue2中 props:[] 进行了声明后就能直接使用，而vue3中 组合api使用defineProps声明后，是放到一个对象中进行保存，因此需要.属性或结果获取
   5. 使用props,provide-inject 接收的父组件状态在进行获取对应的属性或方法后(原生属性或方法)原生响应式就丢失了(state),因为其改变了地址引用(此时不再是引用的状态，在重绘时也不会进行更新)，因此需要使用computed属性进行计算，得到当前状态的最新需要得到的数据(如果想进行原生状态对象的属性状态扩展可以使用toRef&toRefs)
   6. 组合api computed和vue2中computed作用相同，配置方法也相同，只不过写法不同，在每次刷新都会获取状态过滤后的最新值(相当于对原状态右进行了一次代理过滤)；每次页面重绘时，使用了的computed属性的get方法就会重新调用一次，如果两次数据相同就不重新渲染，反之重新渲染
  */
  ```

+ 踩坑

  ```java
  /*
   1. 使用 组合api编写vue后需要注意，每次页面重绘只会对状态和computed计算数据进行一个值的重新渲染，其他数据的改变只会依赖于页面首次渲染时的值(页面值会改变，但不会呈现在页面上，而是内存中，如果你想让其呈现把其定义为computed或状态即可)
  */
  ```

  

+ 介绍

  ```java
  /*
     组合式编程覆盖了配置项式编程的大多功能，并基于此做了简化，setup语法糖便是这种简化的载体，如果确认使用组合式api编程，则可以使用此语法糖写法，该写法底层和配置项式编程原理一致，但做了一些简化，如在引入一个组件文件时，我们不需要进行注册(vue底层完成注册)；编写的所有变量我们不需要return交给vue底层(vue底层会自己获取这些变量)，就可以在template中使用;我们不需要手动暴露组件(由vue底层完成组件暴露)
    即在使用了setup语法糖后，我们不需要注册，return setup函数内容和暴露组件文件了，这些都由vue底层完成(配置项编写时，这些工作需要我们完成)
   注意
   	在使用setup语法糖后，就必须使用组合api获取特定的数据，如ref,reactive,computed,useStore等
   	组合式api完美诠释了按需引入(需要什么特性就进行引入，符合react中函数组件的初衷)
  */
  ```
  



+ 在组合api中使用 props,ref,emits,state(状态管理)

  ```java
  /*
   1. 使用 props接收父组件传入子组件的参数
   	介绍
   		需要在从vue引入一个函数， defineProps ,调用此函数传入可接受的数据(和vue2props配置方法一致)
  	使用
      	import { defineProps } from 'vue'
          const props = defineProps(["name","age"])  // 可以从外部接收一个name,age属性
  
   2. 使用 ref 收集一个组件实例对象或一个虚拟dom
   	介绍
   		vue3依然支持ref属性收集一个属性或一个虚拟dom，在组合api中使用时，使用 const ref属性值 = ref() 去接收对应的ref收集的数据
   		在vue3中，当ref收集到的是一个组件实例时，获取的是其暴露的defineExpose({})里的内容
  	使用
      	// 在vue3中，ref收集的是一个组件实例时，在组合api中使用 const ref属性值 = ref() 接收时(接收的属性值要和ref收集时的相同)，拿到的是该组件使用 defineExpose() 暴露出来的值
      	// 子组件
      		<template>
                  <p>{{data }}</p>
              </template>
  
              <script setup>
              import {
                reactive,
                toRefs
              } from 'vue'
  
              const data = reactive({
                modelVisible: false,
                historyVisible: false, 
                reportVisible: false, 
              })
              // 当该组件实例对象被ref收集时，暴露出形参1
              defineExpose({
                ...toRefs(data),
              })
              </script>
  		
          // 父组件
          	<template>
                  <button @click="onClickSetUp">点击</button>
                  <Content ref="content" /> // 把该组件隐射到 ref中，取了一个别名content
              </template>
  
              <script setup>
              import {ref} from 'vue'
  
              // content组件ref
              const content = ref() // 获取 ref对象content收集的映射 ，
              // 点击设置
              const onClickSetUp = ({ key }) => {
                 content.value.modelVisible = true
              }
  
              </script>
              <style scoped lang="less">
              </style>
  	注意
      	当ref函数返回值接收的变量名和ref在template接收的组件实例和虚拟dom 的属性值是相同时，则表示接收的是template中的组件实例或虚拟dom的映射 。 如果是一个组件实例，则此时获取的是该组件使用 defineExpose 暴露出来的数据 ； 也就是说 在const 变量 = ref(); 接收一个ref()包装的数据后,如果变量名和template中ref="xx"的属性名("xx")相同，则会优先获取这个虚拟dom或该组件暴露出来的数据
      	
   3. 使用 defineEmits 限制 可接收的自定义事件
   	介绍
   		使用 defineEmits 可以声明一个组件客接收的自定义事件，并将emit方法进行返回，此时我们可以调用此方法触发对应的事件即可(和vue2中触发自定义事件方法一致)
   	<template>
         <a-button @click="isOk">
           确定
         </a-button>
      </template>
      <script setup>
      import { defineEmits } from 'vue';
  
      // emit
      const emit = defineEmits(['visible'])
      // 点击确定按钮
      const isOk = () => {
        emit('visible'); // 触发visible事件(可以携带参数)
      }
      </script>
  
  4. 使用ref& reactive创建一个自定义状态(ref还可以用来接收一个虚拟dom或组件的映射，此时需要接收的变量和ref收集时的变量相同)
   注意
   	组合api用于按需引入，原写法和vue2中一致，达到的效果也一致，但完成功能的方式不同
  */
  ```
  
  



### ts编写

+ 注意

  ```java
  /*
   1. 计算值将根据返回值自动推断类型
   	如 const doubleCount = computed(() => count.value * 2) // 此时为number类型
   2. 在获取一个变量下的属性或方法时，必须指明其的变量类型否则会报错
   	如 在dom事件中，必须只能事件对象，否则无法获取需要的值
   		const handleChange = (evt: Event) => {
            console.log((evt.target as HTMLInputElement).value)
          }
   3. 其他ts规范和react中编写的一致        
   4. 在vue3+ts的编写中，我们可以使用泛型来指定传入的类型，此时有更好的代码提示，和更少的发生错误
   5. vue3+ts在大多情况下都是使用泛型进行类型约束，在可能为null 的部分进行类型断言即可
  */
  ```

+ 遇到bug

  ```java
  /*
   1. 现阶段的vue 从一个 .vue文件中引入 组件和接口时，接口会进行警告(不会报错)，把该接口单独放到一个接口文件z
  */
  ```

  

+ 创建ts-vue3项目

  ```java
  /*
       # 1. Install Vue CLI, 如果尚未安装
      npm install --global @vue/cli@next
  
      # 2. 创建一个新项目, 选择 "Manually select features" 选项 ，选择typescript即可
      vue create my-project-name
  
      # 3. 如果已经有一个不存在TypeScript的 Vue CLI项目，请添加适当的 Vue CLI插件：
      vue add typescript
  */
  ```

+ props属性中使用接口方式

  ```java
  /*
    import {defineComponent, PropType} from 'vue';
  
      interface Book {
          name: string;
          behavior: () => void;
          price: number;
      }
  
      export default defineComponent({
          name: 'HelloWorld',
          props: {
              msg: Object as PropType<Book>,
              callback: {
       		  type: Function as PropType<() => void>
      		},
          },
      });
  */ 
  ```



+ reactive生成代理状态时，可以使用接口限制写为

  ```java
  /*
      当声明类型 reactive property，我们可以使用接口：
  
          import { defineComponent, reactive } from 'vue'
  
          interface Book {
            title: string
            year?: number
          }
  
          export default defineComponent({
            name: 'HelloWorld',
            setup() {
              const book = reactive<Book>({ title: 'Vue 3 Guide' })
              // or
              const book: Book = reactive({ title: 'Vue 3 Guide' })
              // or
              const book = reactive({ title: 'Vue 3 Guide' }) as Book
            }
          })
  */
  ```




+ ref收集一个虚拟dom或组件实例对象时的写法

  ```java
  /*
   1. 得到虚拟dom
   	<script setup lang="ts">
          import { ref, onMounted } from 'vue'
  
          const el = ref<HTMLInputElement | null>(null)
  
          onMounted(() => {
            el.value?.focus()
          })
          </script>
  
          <template>
            <input ref="el" />
          </template>
     </script>
  
   2. 得到组件实例对象暴露的值
   	<script setup lang="ts">
          import MyModal from './MyModal.vue'
  
          const modal = ref<InstanceType<typeof MyModal> | null>(null)
  
          const openModal = () => {
            modal.value?.open()
          }
      </script>
  */
  ```

  

### vuex4

+ 介绍

  ```java
  /*
   vuex4 在3的基础上进行了一些微小的改动，如新增了一个钩子函数 useState,可以获取当前vuex管理的状态(只能在setup中使用) ； 创建一个store 对象时使用 createStore 工厂函数创建(传入配置项后返回一个store对象)
  */
  ```

+ 使用

  ```java
  /*
   import { useStore } from 'vuex'
  
  export default {
    setup () {
      const store = useStore()
  
      return {
        // 使用 mutation
        increment: () => store.commit('increment'),
  
        // 使用 action
        asyncIncrement: () => store.dispatch('asyncIncrement')
      }
    }
  }
  
  注意
  	vuex4 仍然兼容 mapState,mapActions等，但这些方法只能用在配置式编写vue中，无法用在组合式编写vue中，复合式编写vue 只能使用 store获得状态对象，并进行进一步处理
  */
  ```

  



### vue-router4

+ 踩坑

  ```java
  /*
   1. 编程式路由跳转到当前路由(参数不变)，多次执行会抛出NavigationDuplicated的警告错误
   	因为 编程式路由导航(push,replace)默认有一个promise返回值，如果多次请求且不做处理时，会抛出此错误，可以传入两个空的函数，此时promise状态就为null 因此就解决了此问题
   
   2. 在使用vue-router4时，需要确认安装的版本是正确的
   3. vue-router4的push-replace完成变成路由导航时，与 vue-devtool 不兼容，导致无法跳转，因此需要关闭打开工具(以后会解决)
  */
  ```

  

+ vue-router3  ===> vue-router4 变化

  ```java
  /*
   1. new Router 变成 createRouter
   	// 以前是
      // import Router from 'vue-router'
      // 现在是
      import { createRouter } from 'vue-router'
  
      const router = createRouter({
        // ...
      })
   
   2. 新的 history 配置取代 mode#
  	mode: 'history' 配置已经被一个更灵活的 history 配置所取代。根据你使用的模式，你必须用适当的函数替换它：(以下函数都需要从vue-router中进行引入)
      "history": createWebHistory()
      "hash": createWebHashHistory()
      "abstract": createMemoryHistory()
   
   3、 移动了 base 配置 
   		补充：
   			base路径指，在页面打开时默认添加上去的初始路径，之后所有的路由匹配都基于此基础路径后的部分开始路由匹配
      	如
      		import { createRouter, createWebHistory } from 'vue-router'
                  createRouter({
                    history: createWebHistory('/base/'), // 初始路径为 /base/ ，只有初始路径后的部分会参与路径匹配
                    routes: [],
                  })
   
   4. 删除了 fallback 属性
   5. 删除了 *（星标或通配符）路由
   	现在必须使用自定义的 regex 参数来定义所有路由(*、/*)：
   		推荐写法： { path: '/:pathMatch(.*)*', name: 'not-found', component: NotFound }, // 最后一个 * 号不能省略，否则在跳转时 / 会当作字符解析
  ... d 		
  */
  ```




+ 组合api

  ```java
  /*
   useRouter 在setup中 获得 $router对象
   useRoute  在setup中 获得 $route对象
   onBeforeRouteLeave 在 setup中 编写 组件内路由守卫
   onBeforeRouteUpdate 	
  */
  ```





### element-ui

+ 介绍

  ```java
  /*
   和ant design 一样同为组件库(服务于vue) ，用法基本相同，更多的是属性api和功能
   注意
   	按需引入，或更优引入查询官网
  */
  ```


+ 自动引入配置(vite版)

  ```java
  /*
  // 在vite.config.ts中添加如下内容
  import { defineConfig } from "vite";
  import vue from "@vitejs/plugin-vue";
  import AutoImport from "unplugin-auto-import/vite";
  import Components from "unplugin-vue-components/vite";
  import { ElementPlusResolver } from "unplugin-vue-components/resolvers";
  // https://vitejs.dev/config/
  export default defineConfig({
    plugins: [
      vue(),
      AutoImport({
        resolvers: [ElementPlusResolver()],
      }),
      Components({
        resolvers: [ElementPlusResolver()],
      }),
  
    ],
  });
  
  */
  ```




+ form常用属性和方法

  ```java
  /*
    el-form常用配置项
      ref 将该组件实例对象暴露出来得数据放到ref得指定属性下(在使用时使用相同属性名得变量名接收
        如 const 相同ref属性 = ref<泛型>()
      model 表示该form表单数据映射到得响应源对象
      status-icon 表示显示所有表单得icon
      rules 表示该form表单验证得操作函数(需要设置为reactive)
      label-width 设置表单左距离
    el-form-item
      labal 表示当前表单得头部显示内容
      props 表示该标签使用的一个数据验证属性(取值范围为 rules的reactive Proxy代理的属性)
  
   注意
   	在将一个el-form收集到ref的指定属性下后，使用该指定属性名作为变量接收ref()的返回值时，获取的是其暴露出来的东西。即操作该表单的方法，包括 （validate，resetFields等），详情见文档
  */
  ```

  


### Pinia

> 发音：piña ,  /peenya/ && 劈那 && 批俩 

> 参考：https://segmentfault.com/a/1190000041554023

+ 介绍

  ```java
  /*
   Pinia是新一代的状态管理工具，符合vuex5的所有提案(vue首推)，兼容性好，可以在vue2和vue3使用
  */
  ```

+ 响应式数据关联(重要)

  ```java
  <script lang="ts" setup>
      import {ref, reactive, toRefs} from "vue";
  
      // 1.总结使用 Proxy代理时，指向(代理的)是原复杂数据类型(数据源对象)
      // 2.使用 toRefs可以返回一个对象，该对象内是其源对象最外层响应式属性，
      // 通过改变这些值可以映射到源对象上，并进行页面重绘
      // 3.重要： 每次页面重绘 状态(和computed属性)都会重新调用get方法获得数据，
      // 并进行前后对比，相同不更新，不相同则进行更新
      // 4. pinia 的 storeTorefs 和 toRefs同理，都是将一个原对象的最外层属性进行一个ref对象包装，并使用对象方式返回，操作的都是原对象(storeToRefs是将该对象的state,和getters中的计算属性进行一个代理，并使用对象返回)
  	// 5. 重要： reactive是对一个源对象进行代理(proxy)，toRefs是对一个源对象的最外层属性进行ref代理(返回一个对象包裹，因此我们可以解构获取需要的ref代理) ； pinia调用一个store函数后，返回的是一整个原仓库的Proxy代理(包含state,actions里的方法，和getters里的计算属性以及一些操作状态的方法)，使用storeToRefs是将该store对象的state,和getters中的计算属性进行一个代理，并使用对象返回 ，因此我们可以解构，两种方式完成的状态代理都是操作的同一个数据源，在某一个引用改变状态后会进行页面重绘，重新调用get方法获得源代理数据
      // 6. 在页面重绘时，只有状态和computed属性（以及前后不相同key的资源三方引用）会进行重绘(重新调用get获取最新的数据)，其余非状态数据只有在页面首次加载时做初始化显示(在之后可以通过方法操作这些数据的变化，但不会呈现在页面上)
      const count = ref(0);
      const person = reactive({
          name: "张三",
          age: 13,
          sex: "male"
      });
  
      function computer() {
  
          // 在script中改变ref包装的引用对象时，
          // 需要使用value调用其get和set代理方法更新数据和重绘页面
          count.value++;
      }
  
      function change() {
          // person为一个Proxy代理对象，通过该代理对象修改属性时，
          // 会调用set方法，其内通过Reflect.set()修改原对象中的数据，并进行页面重绘
          person.name = `李${Math.floor(Math.random() * 10)}`;
          person.age = Math.floor(Math.random() * 10) + 10;
      }
  
      // 为了更好方便获取一个对象的某个响应式属性，因此需要使用toRefs
      // 将一个复杂数据类型的最外层进行响应式包装(包装为一个ref引用对象)
      // 该ref引用对象在script中使用时需要使用 .value得到或修改的代理值
      // 在template中则可以直接使用，vue底层会使用该ref对象的value属性得到状态
      // toRefs每调用一次就会返回一个对象，该对象内的所有属性就是其原对象最外层属性进行包装后的一个引用对象
      const obj = toRefs(person);
      console.log("person-toRefs", obj);
      const {name, age} = obj;
  </script>
  
  <template>
      <h2>
          {{name}} ---- {{age}}
          <button @click="change">change</button>
      </h2>
      <hr>
      {{count}}
      <button @click="computer">add</button>
  </template>
  
  
  <style scoped lang="scss">
  
  </style>
  
  ```

  

+ 技巧

  ```java
  /*
   1. 通常使用复杂逻辑才能完成状态的修改时，将其写在action中即可
   2. storeToRefs 用于将一个pinia store管理的proxy代理的最外层state对象中的最外层属性进行一次数据代理(升级为状态)，该代理后的数据可以直接解构(因为其已经包装为响应式的了)，并写入到template中(template中的数据可以直接获取一个代理对象的value属性)，在setup语法糖中则修改proxy代理的状态即可，页面会进行对应的状态更新(因为已经使用storeToRefs将其原对象中的最外层属性包装成了状态) ； 
   	总结使用 toRefs()和storeToRefs()包装的ref代理属性适用于template中(底层会直接获得value的值)，不使用这些包装也行，只不过需要xx.xx获取需要数据，它们会将源对象的最外层属性包装为一个ref引用对象进行代理，当这些值改变后，页面会进行对应重绘
   	注意
   		storeTorefs和toRefs都是将一个原对象的最外层属性包装为一个状态，和proxy代理操作的都是同一个地址，因此一个引用将状态改变后 所有地方都会重绘(底层调用trigger()方法)
   
  注意
  	vue3+pinia 达到最强状态，此时只需要配置单个store文件就能达到预期管理状态效果 ，reacrt-redux为4个，vuex为3个，redux-toolkit 为2个，pinia为一个
  */
  ```

  



+ Pinia& Vuex

  ```java
  /*
  Vuex： State、Gettes、Mutations(同步)、Actions(异步)
  
  Pinia： State、Gettes、Actions(同步异步都支持)
  
  Vuex 当前最新版是 4.x
      Vuex4 用于 Vue3
      Vuex3 用于 Vue2
  
  Pinia 当前最新版是 2.x
  	即支持 Vue2 也支持 Vue3
  
  注意
  	Pinia优于vuex推荐使用
  */
  ```

  

+ Pinia核心特性

  ```java
  /*
  1. Pinia 没有 Mutations
  2. Actions 支持同步和异步
  3. 没有模块的嵌套结构
  	Pinia 通过设计提供扁平结构，就是说每个 store 都是互相独立的，谁也不属于谁，也就是扁平化了，更好的代码分割且没有命名空间。当然你也可以通过在一个模块中导入另一个模块来隐式嵌套 store，甚至可以拥有 store 的循环依赖关系
  4. 更好的 TypeScript 支持
  	不需要再创建自定义的复杂包装器来支持 TypeScript 所有内容都类型化，并且 API 的设计方式也尽可能的使用 TS 类型推断
  5. 不需要注入、导入函数、调用它们，享受自动补全，让我们开发更加方便
  6. 无需手动添加 store，它的模块默认情况下创建就自动注册的
  7. Vue2 和 Vue3 都支持
  	除了初始化安装和SSR配置之外，两者使用上的API都是相同的
  8. 支持 Vue DevTools
  	跟踪 actions, mutations 的时间线
  	在使用了模块的组件中就可以观察到模块本身
  	支持 time-travel 更容易调试
  	在 Vue2 中 Pinia 会使用 Vuex 的所有接口，所以它俩不能一起使用
  	但是针对 Vue3 的调试工具支持还不够完美，比如还没有 time-travel 功能
  9. 模块热更新
  	无需重新加载页面就可以修改模块
  	热更新的时候会保持任何现有状态
  10. 支持使用插件扩展 Pinia 功能
  11. 支持服务端渲染
  */
  ```

  



+ pinia使用

  ```java
  /*
   安装
   	npm i pinia
   注意
   	pinia对比vuex，(pinia)一个为分布式管理状态(分模块管理状态，按需引入需要用到的状态)，(vuex)一个是集中式管理状态(把所有状态集中管理)
   	pinia管理的状态实际上是其内部做了一个proxy数据代理(以便页面重绘)，我们引入该状态文件后，调用其函数返回的就是其proxy代理的数据(。我们可以对其进行结构获取需要的状态，但此方法获取的数据并不是状态，需要storeToRefs进行浅响应式包装，和toRefs()原理一致)
   	使用 storeTorefs将一个pinia的proxy代理对象的最外层对象包装成状态后，可以进行解构赋值获取数据，但这个状态包装使用的是 ref引用对象方式，因此需要使用该对象的value属性才可以获取状态(不方便)，现阶段还是推荐使用直接获取的proxy代理获得状态
   	
   挂载pinia        
   	import { createPinia } from 'pinia'
  	createApp(App).use(createPinia()).mount('#app')
  
   pinia基本配置
   	import { defineStore } from 'pinia'
   	
   	// 和vuex3配置项大致相同
      export const userStore = defineStore('user', {
          state: () => {
              return {  // 编写该store片管理的状态
                  count: 1,
                  arr: []
              }
          },
          getters: { ... }, // 编写该状态库的计算属性
          actions: { ... } // 把复杂的逻辑操作状态逻辑进行封装 ,action中的方法可以接收一个形参，也可以不接收而使用this得到当前state(此时不能使用箭头函数，因为其this和外侧this一致)
      })
  
   访问state的方式
   	<template>
          <div>{{ user_store.count }}</div>
          <button @click="handleClick">按钮</button>
      </template>
      <script lang="ts" setup>
      import { userStore } from '../store'
      import {storeToRefs} from "pinia";
      const user_store = userStore() // 返回的是一个proxy代理，可以对其进行解构，但解构后的数据是其源数据(即如果是一个复杂数据类型则为一个Proxy代理，简单数据类型则为原类型)，此时简单数据类型没有数据绑定（即不是一个状态），因此需要使用 storeToRefs(userStore)包裹，将其最外层属性包装为状态(这种方式操作的是同一个对象，当改变后会映射到对应源对象的属性上)，需要注意的是 storeTorefs是将其外层进行一个ref数据代理,因此读取时需要使用.value才能触发其get()方法
      const handleClick = () => {
          // 方法一，直接使用该proxy代理触发状态修改
          user_store.count++
  
          // 方法二，需要修改多个数据，建议用 $patch 批量更新，传入一个对象
          user_store.$patch({
              count: user_store.count1++,
              // arr: user_store.arr.push(1) // 错误
              arr: [ ...user_store.arr, 1 ] // 可以，但是还得把整个数组都拿出来解构，就没必要
          })
  
          // 使用 $patch 性能更优，因为多个数据更新只会更新一次视图
  
          // 方法三，还是$patch，传入函数，第一个参数就是 state
          user_store.$patch( state => {
              state.count++
              state.arr.push(1)
          })
          
          // 方法四，当逻辑复杂时，使用该代理对象的action内的方法完成状态改变
          user_store.change(); // 调用action里的change方法 进行状态改变
      }
      </script>
  */
  ```

   



+ 综合使用

  ```java
  /*
   // computer.vue
   <script lang="ts" setup>
      import {storeToRefs} from "pinia";
      import {ref} from "vue";
      import computer from "@/test/store/computer";
  
      const curNumber = ref(1);
      const computerStore = computer();
      console.log("computer:", computerStore);
      // storeToRefs 可以包装原对象的最外层属性将其升级为状态(vue2代理)
      // 该解构适用于template直接编写，而不适用于setup语法糖中的编写
      // (ref引用对象在script中编写时每次获取值需要.value，而template中则不需要会直接获取value后的代理数据)
      const {count, enRich} = storeToRefs(computerStore);
  
      // 调用action里的方法改变状态
      function asyncAdd() {
          computerStore.asyncAdd(curNumber.value);
      }
  
      // 调用action里的方法改变状态
      function evenAdd() {
          computerStore.evenAdd(curNumber.value);
      }
  </script>
  
  <template>
      <h2>当前和为：{{count}}</h2>
      <h2>{{enRich}}</h2>
      <select name="default" v-model="curNumber">
          <option :value="1" id="default ">1</option>
          <option :value="2">2</option>
          <option :value="3">3</option>
      </select>
      <button @click="count+=curNumber">+</button>
      <button @click="count-=curNumber">-</button>
      <button @click="asyncAdd">异步+</button>
      <button @click="evenAdd">偶数+</button>
  </template>
  
  // computerStore.ts（）
   // 2022/6/26 19:39
    
  import {defineStore} from "pinia";
  
  const count = 0;
  export default defineStore("computer", {
      state() {
          return {count};
      },
      actions: {
          // 异步+
          asyncAdd(value: number): void {
              setTimeout(() => {
                  this.count += value;
              }, 1000)
          },
          // 偶数+
          evenAdd(value: number): void {
              if (!(this.count % 2)) {
                  this.count += value;
              }
          }
      }
      ,
      getters: {
          enRich(): number {
              return this.count * 10;
          }
      }
  });
  */
  ```

  



### vite

+ 介绍

  ```java
  /*
   vite是新一代版本控制工具，其将所用到的模块进行按需加载，因此大大提高了打包效率
  */
  ```

  

+ 使用

  ```java
  /*
   npm create vite@latest my-vue-app -- --template vue-ts // 创建一个vite项目
   注意
   	vite创建项目后，首次需要自己安装所有依赖，其只是做了一个依赖收集(npm i)
  */
  ```

  
