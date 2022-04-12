#  Vue(2.x)

在面试中一定要对功能有一个整体认识，并不需要把整体代码说出来

# 学完Vue开始使用vue搭建网页

snipaste 想要获取颜色时，可获的 220,232,123 方式和 #feea12方式，按shift切换获取模式，按c复制颜色

在js中写debugger;在执行时，执行到debugger行时页面会停住

Vue.config.productionTip = false,可以不在控制台上显示vue提示

在组件内使用vue下的属性时需要加 this.xxx ,在template或html中引入vue属性时直接 xxx即可

js中对象和对象原型下的方法不一样但都能使用call，apply,bind调用，可通过编译器提示调用想要的方法

为了方便项目维护，一般不要定义字面量，通常把字面量放到一个配置文件里，方便修改此值

+ bind改变指向

  ```js
  /*
     let obj = {
        arrow() {
          console.log(this)
        }
      }
  
      // bind用来改变this指向，该改变只会在该表达式中有效，需要持久化，则要一个变量进行指向地址
      obj.arrow.bind(this) // 对象里函数this改变仅在该条语句中有效  obj.arrow.bind(this)()可以调用此函数，此时this指向window
    let winThis = obj.arrow.bind(this)  // 此时winThis接收到这个函数，且this已经指向window
  */
  ```

  

+ 伪数组变真数组的方法

  ```js
  /*
   Array.from()方法可以把伪数组转换为真数组，使用其数组下的方法，也可把字符串转换为数组
   1. Array.from()转换伪数组
   	let lis = document.querySelectorAll('li') //获取的是伪数组
   	Array.from(lis) // 变为真数组
   	
   2. Array.from()转换字符串
   	Array.from(' 123') // 变为数组为 [' ','1','2','3'] ,把每一个字符串切割然后变为新数组
   	
   2. 使用Array.slice()转换伪数组
    slice方法本身对数组的操作就是浅克隆(只会克隆最外层的对象或数组，里面嵌套的数组或对象不会克隆)
    	let lis = document.querySelectorAll('li') //获取的是伪数组
   	Array.prototype.slice().call(lis )
   注意
   	Array.from()方法只能转变字符串和数组，转变其他的东西没有效果或报错
  */
  ```

  

+ 关于对象key解析

  ```js
  /*
   1.对象格式为 '字符串':数据类型  在js中通常简写为 a:true (实际会自动转换为'a':true)
   为了让对象的key值能对变量进行解析，应使用[]对变量进行包裹 如{
   	let a = '123'
   	let obj = {
   	// 被包裹的变量会先进行转换然后转换为对应的值
   		[a]:23 //本代码输出为['123']:23 
   	}
   	// 在解析变量时 也有两种写发
   	console.log(obj.123) // 123默认为字符串类型，该语句为 打印obj下的123key对应的值
   	console.log(obj[a]) // obj[a]表示打印obj下的a变量翻译后的值，即obj.123
   }
   
   注意
   	解析变量时可以使用 obj.xx.xx方式 ，如有对应变量引用也可以使用 obj[x][x]方法，和obj.xx.xx作用相同，不过[]内的变量可以经过变量转换，而obj.xx.xx就只是单纯的字符串
  */
  ```

  

+ 暴露模块的三种方法

  ```js
  /*
   1. es5 暴露
   	(function(w){
   		function a(){xxx}
   		window.a = a
   	})(window)
   2. es6 暴露
    function a(){}
    export {a}
   
   3. commonJS
    function a(){} 
    function b(){}
    exports {a,b}
  */
  ```




+ deBug调式

  ```js
  /*
   以后断点调试都用debug,把鼠标移动到对应变量上时，他会显示当前变量存入的值
   在一个代码出现问题时，通常会选择在网页中打断点或在代码内添加console.log打印控制台，通常网页打断点方式效率会高
   打断点方法： f12打开控制台，切换到sources，点击需要打断点的行即可
   
   操作按钮(从左到右依次)：重新调试，跳过下一个的调试函数(直接执行完)，进入下一个函数，跳出当前函数(函数会被执行完毕)，下一步
   
   目的
   	查找bug，不断所有可疑代码范围
   	查看程序的运行流程(熟悉代码在接收新代码时)
   
   
   注意
   	一个大型赋值语句中，无法在这个赋值语句中打断点，只能在接收变量前打断点
   	代码运行到断点即可暂定
   	除了网页中打断点，也可在代码中使用 debugger 打断点
  */
  ```

  ![image-20210926175641544](C:\Users\xx\AppData\Roaming\Typora\typora-user-images\image-20210926175641544.png)



+ documentFragment(文档碎片模型)

  ```js
  /*
   一个fragment对象就是内存中的节点容器(只存在内存中)
   // 获取dom文档碎片
      let ul = document.querySelector('div')
  
      // 创建文档碎片对象
      let fragment = document.createDocumentFragment()
      let childNode;
      while (childNode = ul.firstChild) {
        fragment.appendChild(childNode)
      }
      let fragNodes = fragment.children[0].children
      Array.prototype.slice.call(fragNodes).forEach(item => {
        item.innerHTML = ''
        ul.appendChild(item)
      })
  */


+ Vue文档中

  学习一栏很多分区都很重要，可以详细阅读，生态中，插件可反复查看

vue为一个渐进式javascript框架，用于动态构建(显示)用户界面

+ vue特点

  遵循MVVM模式

  编码简洁，体积小，运行效率高，是和移动/pc端开发

  它本身只关注UI,可以轻松引入vue插件或其他第三方库
  
  采用组件化模式。提高代码复用率，更好维护代码。(声明式编码，让编码人员无需操作DOM,命令式编码，自己操作DOM元素)
  
  采用diff算法对比差异渲染虚拟DOM

vue借鉴angular的模板和数据绑定和react的组件化和虚拟DOM技术



+ vue扩展插件

  vue-cli:vue脚手架

  vue-resource(axios):ajax请求

  vue-router：路由

  vuex:状态管理

  vue-lazyload: 图片懒加载

  vue-scroller:页面滑动相关

  mint-ui:基于vue的UI组件库(移动端)

  element-ui: 基于vue的UI组件库(PC端)

+ vue.js已指令和插值存在

  ```js
  /*
   指令：vue自定义标签属性，以v-开头，属性值是一个js的表达式 v-model="msg"
   插值：双大括号表达式，动态显示数据 {{msg}}，即可获取data里的数据，根据数据的不同类型可以调用其各自的方法
   
   <div id="test">
    <input type="text" v-model="msg"> 
    <p>hello {{msg}}</p> <!--{{msg}}为引入对应的vue实时数据-->
  </div>
  
   new Vue({ //配置对象：属性名是一些特定的名称，每个属性都是一个选项
        el: "#test", //指定html元素
        data: { // 数据项
          msg: 'hello world!' // 添加初始值 ，只要改变了数据页面就会重新加载，和React状态类似
        }
      })
  */
  ```

  

+ MVVM模式

  ```js
  /*
  Vue参口了MVVM模型，并构建了一个vue模型
   
   MVVM
   M 模型（Model）: 对应data中的数据
   V 视图(View) : 模板templete即html标签
   VM 视图模型(ViewModel): Vue实例对象
   工作过程：声明了vue实例并在html中使用，使用中映射到VM
  */
  ```
  
  

![image-20210830155151464](C:\Users\xx\AppData\Roaming\Typora\typora-user-images\image-20210830155151464.png)



+ Object.defineProperty

  ```js
  /*
  Object.keys(obj) keys方法可以把指定对象的所有key值提取出来变为一个数组
  
  Object.defineProperty属性调用都是一次性的会去效用里面的属性
  
   Object.defineProperty可以创建一个属性，并可以给这个配置对象自定义配置(属性描述符)。此方法添加对象对比 对象.xx的方式添加的对象具有以下优势
   	数据链接性，控制是可读可改可遍历性
   	
   基本语法 Object.defineProperty(指定对象，'添加的key值',{ "配置对象"
   	get(){ // get方法：在读取状地形对象属性值时，自动调用,this指向原对象
   	},
   	set(value){ // 在设置的key的value值改变后触发，有默认形参value(表示更改的数值)，用于重新更新该key值用到的属性的值
   	},
   	enumerable: true, // 可遍历
   	writable: true, // 可修改
   	configurable: true // 可删除
   })  以这种方式书写的属性不会真实出现在原对象中
  */
  ```
  
  

## 1.0 基础操作

若Vue实例中，data数据内在同一及对象中有两个相同属性，则在显示时以最后的属性为准

一个容器只能被一个vue实例接管(一对一关系)。若两个元素相同则接管第一个元素

vue 控制的所有回调的this都是vue组件对象，vue给v,定义了一些与data中的属性对应的属性 ，getter方法：当通过vm.xxx 读取属性时触发，setter方法：为通过vm.xx修改值时触发，他们的原值都保存在_data属性里

数据代理： vm._data.xxx ===> vm.xxx ,在模板中可以使用 this.xxx或xxx获取数据，数据代理用到了Objecxt.defineProperty()里的get,set方法来搭建

在模板中读数据，查找的都是vue实例对象



html标签默认会和data里的数据进行交互，当data数据有嵌套时，html想获取这个嵌套数据需要使用xx.xxx方法

在vue实例里，操作data里的数据需要使用this.xxx方式，使用xxx方式回去全局作用于查找元素

+ v-model

  ```js
  /*
   1.v-model 用于表单动态改变data值，且是双向数据绑定,v-model本质上是获得input框的value值
   	（1）如果给一个单选框绑定v-model则需要给他们分别指定对应的value值，这样才能获得对应的数据
   	
   	（2）如果给一个复选框绑定v-model，在没有给定value值时，默认管理获取checked值，所以需要分别管理复选框需要给data设置一个[]值，且每个复选框需要给唯一的value值，这样挡在复选框选定时，获取的数据就为 ['game','xxx']，如果给data设置一个''值则起不到分别管理的作用只会是一个单纯的布尔值 。所以在使用v-model给多组同一类别的复选框绑定时，应给每个复选框一个唯一value值，且在data中以数组的形式存储
   	应用： 在给爱好设置多个复选框时，应使用[]方式进行存储，因为有多个同类别的复选框。 在给是否同意协定设置复选框时，只需获取布尔值即可，因为只有一个可选择的同类别复选框
   	
   	（3） 当给 select 设置v-model时，给它的每个option设置value值即可
   	
   2. v-model修饰符
    v-model.number 可以将存储数据转换为number类型
    v-model.lazy  可以在text框使出标点后才收集data
    v-model.trim 去掉空格
  */
  ```

  

+ vue作用域范围选择

  ```js
  /*
  可以使用两种方式声明元素作用域
   1.  在 new vue({ el:''}) el中直接选择
   2、 在生成Vue实例对象使用显示原型上的v.$mount('')方法绑定
   */
  ```

+ vue实例中data两种写法

  ```js
  /*
    1. 对象式,全局共享data状态
    	new Vue({
    		el:'#root',
    		data:{
    			xxx
    		}
    	})
    2. 函数式，组件中私有data状态
    	new Vue({
    		el:'#root',
    		data(){
    			return {
    				xxx
    			}
    		}
    	})
  */
  ```

  

+ 指令

  ```js
  /*
  注意
    :xxx表示升级为Vue动态管理的xxx属性 可以解析表达式如 Date.now() ，和Vue里的属性，多用与解析data数据 
    在Vue作用域内，用到的Vue数据要是以标签属性方式写入要加:xx或v-bind:xx，以标签的内容写入要以{{xxx}}方式写入
    v-bind:或: 为单项数据绑定，数据中内容改变不会影响vue实例的data数据
    v-model 为双向数据绑定，两边改变数据都会改变data里的数据，v-model只能用在标签元素上
   
  指令1: 强制数据绑定
  	使用 v-bind:xxx 或 :xxx可强制绑定数据，如在给a标签的href设置一个可改变的地址应写为 <a v-bind:href="xxx"></a>或<a :href="xx"></a>
  	
  指令2: 指定事件监听
  	使用v-on:click="xxx"或@click="xxx"来绑定事件，同时xxx需在new Vue({method:{}})中声明，在vue中，xxx可以是一个函数也可以是一条或多条语句,如果传过来的是一个没有被调用的函数，vue会添加一个环境变量，如果传递过来的是一个调用了的函数(可以直接传参)或一些语句，vue会往外层套一个函数，想要传递这个环境变量可以使用$event传递形参，然后method方法函数中按位接收即可
  		事件修饰符
  			1.阻止默认行为 可以使用 @click.prevent="xxx"这样·及调用了函数也阻止了默认行为
  			2.组织冒泡行为 可以使用 @click.stop="xxx"这样·及调用了函数也阻止了冒泡行为
  			3.按键修饰符 可以使用 @keyup.13="xxx" 或 @keyup.enter="xxx" 这样及调用了函数也设置了按键触发,其他修饰符还有 .tab(tab必须配合keydown使用) .delete .esc .space .up .down .left .right
   系统修饰键：ctrl,alt,shift,meta  配合keyup使用需按下另外一个按键才有效果，配合keydown使用正常触发事件
  定义一个别名按键  Vue.config.keyCodes.huiche=13 // 给13(Enter)新增了一个别名       注意：案件修饰符可以连写如： @keyup.stop.prevent 先阻止冒泡，在阻止默认行为 ， 也可以和系统修饰键形成配合  @keydown.ctrl.c 只有先按下ctrl在按下c才会触发
  
  			4. @click.self = "xxx" .self修饰符只有当触发事件对象为自己的时候才会触发(如果是冒泡等形式不会触发)
  			5. @click.once="xxx" 事件只触发一次(常用)
  			6. @click.capture="xxx" 使用时间的捕获模式
  			7. @click.passive="xxx" 事件的默认行为立即执行，无需等待事件触发
  			
  指令3： 指定data数据
  	使用v-model=xxx可以获取vm实例下的data对应的值，通常该属性会配合input使用，实现动态改变数据，其他标签想要获取data值需使用插入方法{{xxx}}
  
  指令4： 指定动态class类名，静态class依然可用
  	通常把需要操作的样式都维护到data里
  	使用 :class="xxx" xxx可以是数组，对象和data参数，都包裹在""内
  	如 <p :class="{classA: true,classB:false}"/> 则使用classA类名，常用在类名切换中.
  	如 <p :class="[a,b,c]"/> 则使用a,b,c三个类名 
  
  指令5： 指定动态style
  	使用 :style="{color:activeColor，fontSize:fontSize+'px'}" 其中activeColor等都是data数据，可随时更改
  	
  指令6: 更新元素的textContent  v-text  会覆盖原有文本
  指令7: 更新元素的innerHTML v-html
  指令8： v-if 如果为true 当标签才会输出
  指令9： v-else 如果为false 当前标签才会输出
   v-if v-else-if v-else  和js中逻辑原理相同 必须对组连续存在 ，如 v-if v-else,v-if v-if v-if单独使用，v-if v-else-if v-else ,不按js逻辑使用则会报错
   
  指令10： 通过控制display样式才会输出到页面
  指令11: v-for 便管理数据/对象
  指令12： v-model 双向数据绑定  
  指令13： ref 为某个元素注册一个唯一标识，vue都可以通过$els属性访问这个元素对象
  指令14： v-show 可以设置标签的显示和隐藏，满足条件时，显示，不满足时，隐藏
  
  指令15： v-cloak 一个vue内置属性，在vue资源还没加载完成时显示此属性，加载完成后会自动移出此属性，可以通过给此属性增加样式 [v-cloak]{display:none}来完成引用vue属性的元素显示与隐藏
  	应用：当网速慢时，可以给所有用到vue属性的标签设置v-cloak属性，在配合[v-cloak]{display:none}，这样所有用到v-cloak的标签会隐藏，当加载完vue环境后，在显示    如：<div v-cloak>{{name}}</div>  css: [v-cloak]{display:none}
  
  指令16： v-once 所在的节点在初次动态渲染后，就变为静态节点，以后data改变内容也不会渲染，及给一个节点加了v-once后该节点只会初始化渲染一次
  
  指令17： v-pre vue会跳过有v-pre属性的节点，不会去解析改节点，可以给没有使用vue语法的节点使用该属性
  
  */
  ```
  
+ directives

  ```js
  /*
   directives用于给用户新增自定义属性
   1. 写在vue实例中
    	new Vue({
    		el:'#root',
    		directives:{
    		// big第一个环境形参是调用此指令的标签元素，第二个环境形参是一系列配置项，express为js表达式，name为指令函数名，rawName为节点中使用指令函数名，value为参数的值。big在元素成功绑定和所在模板被重新解析时都会被调用。
    			//在directives定义的函数都会被vue默认添加为指令,使用 v-xx:value="n"即可完成使用 如 v-big:value="x"，该函数没有返回值，而是直接对该元素的DOM进行操作
    			// big简写方式实际上只包含了 bind,update功能，及在bind和update状态时才会触发
    			big(el,binding){
    				... // 在此{}内可以对此el元素进行DOM操作来完成自己需要的指令效果
    			}
    		}
    	})
    
   2. directives配置函数
   	当在页面启动时，就想让标签正处在某种状态，需要利用其配置函数bind(){},inserted(),update()（每个配置函数都能收到el,binding形参）,否则页面首次加载预期效果不会达到(因为程序触发因果顺序受到干扰)
   	bind() 函数在指令与元素成功绑定时(首次加载绑定时触发)
   	inserted() 指令所在元素被插入页面时触发
   	update() 指令所在模板被重新解析时触发
   	
   		new Vue({
    		el:'#root',
    		directives:{
    			find:{
    				bind(){}, // 首次绑定时触发
    				inserted(){}, // 插入到节点时触发
    				update(){} // 更新数据时触发
    			}
    		}
    	})
    	
  3。 全局指令设置
  	Vue.directive('add',{
  		bind(el,binding){},
  		inserted(el,binding){},
  		update(el,binding){}
  	})  
  	或写成
  	Vue.directive('add',function(){})
    注意
    （1）当设置的指令包含多个单词时 需要写成 'big-number'(ele,binding){}形式
    （2）directives里所有函数的this指向windows
    （3）new Vue内的 directives里的指令为局部指令，要想为全局指令应为Vue.directive()
    （4）自定义完成的指令vue会默认给其添加v-前缀
  */
  ```

  

+ 条件渲染

  ```js
  /*
  1. if---else
   <p v-if='ok'>3</p>  //ok为vue data数据
   <p v-else>2</p>
   
   
   2. for--in
   <li v-for="(i,index) in people" :key="people.id">
          {{i.id}}---{{i.name}}-----{{i.age}}---
          <button @click="remove(index)">删除</button>---
          <button @click="update(index,{id:randomNumber,name:'mirry',age:randomNumber})">更新</button>
        </li>
    v-for会对当前标签进行遍历，使用 v-for="(i,index) in xxx"每个标签需指定一个不同的key用 :key方式,遍历的数据会暂存在内存中，需使用{{i.xxx}}进行插入显示
    key的重要性
    key是diff算法的唯一标识，所以每一个标签需要有一个不该改变且唯一的标识符，这样可以有效解决页面重绘重排问题。diff算法中，相同的key相同的内容不会重新渲染，只会重新渲染新增或没有缓存的key值对应的内容。如果key是一个变化值，就可能会造成key值不匹配元素导致可避免的重绘重排，以及input等输入框不匹配问题 ，出现input不匹配原因为：在diff对比中，两个相同key的内容不同，但input标签相同,导致渲染了内容，input标签进行保留
    diff算法对比过程：对比相同key的内容，如果相同则保留之前的渲染。如果不同则重新渲染，标签不同也会重新渲染，
  */
  ```

+ vue在内部监视数据的流程

  ```js
  /*
   1. 对象中的属性数据改变时 ，vue给属性添加setter方法，来改变数据，和配合页面重新渲染
   2. 数组中的元素数据时,vue重写了大部分操作数据方法，使其在改变时，可以重新渲染页面
   3. 通过下标改变数据的方式如果是复杂数据类型则在内部会改变，而不会重新渲染页面，简单数类型则可以正常重新渲染
  */
  ```

  

## 1.1 vue属性

只有配置在data里的数据才会进行数据代理(有getter.setter配置)，函数也可以放在data里，但这样会增加vue工作量(会生成对应的getter,setter代理)，所以为了简化vue工作量应放到method对象里

在html中的插值和v-xx指令只会解析vue实例变量和js表达式，不会解析js变量

html属性内以 :xxx或v-xx:开头的属性都会被当作js表达式解析

vue.$set(vm..xxx,’key值‘,''修改内容') 和vue.set(vm,’key值‘,'修改值‘)可以追加data响应式属性（对象追加属性方式）,但不能在data根对象下和vm下增加，只能在Data下的对象数据追加。vue.$set(vm.data.xxx,'index','修改内容')可以对数组进行重绘重排 

Vue实例中如果访问了一个undefined值不会在页面上显示

在Vue中只有有get set方法的属性，修改后页面才会重绘重排，对象都有get和set方法，数组没有，所以vue对数组的方法进行了封装，在使用了这些vue封装的方法后页面就会重绘重排，被vue封装过的能够改变页面重绘重排的方法包括push,pop,shift,unshift,splice,sort,reverse 。 新的数组替换旧的数组也可以触发页面重绘重排

Vue中只有改变对应对象的值页面才会重绘重排

在用indexOf查找数据时，如果没有此数据返回-1如果找到则返回该数据下标，如果是一个空字符串则返回0

```js
/*
	1. el 用来指定该vm实例的作用范围 如 ele:"#app"
	
	2. data 用来指定vm实例可用的动态数据 如 data:{a:1,b:2},可以是一个函数或对象，如果是函数则data私有化，如果是对象则是公有化
	
	3 method 用来指定vm实例可用的回调函数（和事件调用一起使用） method:{open(){xxx}}
	
	4. computed 用来实现各个data的交互，在页面第一次渲染和computed里的数据发生改变时都会触发，如动态拼接，修改数据等，格式为 computed:{e:{get(){},set(){}}} ，此属性在计算同一个值好似会产生缓存，在下次继续调用时用缓存即可，减少了空间浪费 . computed里的函数不用自己调用也不能自己调用，在他的值发生改变时，系统会自动调用
		computed内设置的函数需要在html或template中插入，而不能调用，这样才会触发效果。 如果一个计算属性只读可以写为一个函数 computed:{ f(){return 1}}，如果一个计算属性可读可改需要写为对象格式 computed:{w:{get(){},set(value){}}}  ,当获得该w属性是调用get 函数 ，当改变值时调用set函数，形参value表示修改为的值
		如 computed:{all(){return 1}}  <div>{{all}}</div>
		当引用到computed内的函数名后，get会被初始调用一次，此后，该值每修改一次，就会重新调用一次
		
	5.watch 当指定的值发生改变时自动调用，没有返回值，通常用在一个状态改变然后赋值给另一个状态 如 watch:{firstName(value){xxx}}
	
 	监视两种写法
 	（1）. 明确监视目标可在内部书写
	new Vue({
		el:'#root',
		data:{
			isHot:true
		},
		watch:{
			// 配置向写法
			'监视属性':{  // 检测的值包括但不限于 data和computed内的数据，不需要加this
				immediate:true, //初始化后是否立即执行，默认为false
				handler(newValue,oldValue){
					conosle.log('111')
				}
			}
			// 简洁写法 
			info(new,old){ // 简洁方法的函数和handler函数相同
				console.log('11')
			}
		}
	})
	
	（2）. 没想好监视目标可在外部插入
		const vm = new Vue()
		vm.$watch('fun',{
			immediate: true,
			handler(new,old){
				console.log(new,old)
			}
		})
		//简写 , 只写一个函数和handler函数用法相同
		vm.$watch('fun',function(new,old){console.log(11)})
	深度监视
	当data内的数据有多级嵌套式时，watch的监视则会失效(以地址判断是否改变值)，因为指挥监视该xxx只想的那个大对象而不会监视大对象里的数值，这时可以采用指定监视写法 如 ['xxx.a']或'xxx.a'指定监视，或增加deep配置项   mount:{deep: true }开启深度监视，这样被包括在大对象里的所有数据都将被监视
	
6. watch和computed的区别
	watch 擅长异步和同部计算，如开启一个定时器，按一定时间后进行改变状态，缺点，需要开启一个data值来存储状态值
	computed 擅长同步计算，可减少data值的数量从而减轻Vue压力
*/
```



+ vue实例对象方法

  ```js
  /*
  这些实例方法可以在 new Vue({}).xx 添加 也可以指定一个实例 let vm=new Vue({}) ;vm.xxx添加
   1. vm.$watch('属性名',回调函数)  和 watch内部属性同等功能
   2、vm.$mount('标签') 可以将data数据挂在到指定标签范围
  */
  ```

  

  


## 1.2 生命周期函数

+ 常用生命周期函数

  ```js
  /*
   mounted(){} 完成页面渲染后自动触发
   beforeDestroy(){} 在页面即将删除时触发，做收尾工作
  */
  ```

+ 全部生命周期函数

  ```js
  /*
   beforeCreate()正在完成vue初始化,此时无法通过vm访问data中的数据和method的方法
   created() 已经完成vue初始化，此时可过vm访问data中的数据和method的方法
   beforeMount() 页面即将挂载，此时页面呈现的是未经Vue编译的DOM结构
   mounted() 页面已经挂载，此时页面呈现的是经过Vue编译的DOM,可以再此阶段开启定时器，发送网络请求等初始化操作
   beforeUpdate() 页面即将更新
   updated() 页面就已经更新
   beforeDestroy() vm实例及数据即将销毁，一般用来关闭定时器，解绑自定义事件等，一般在这个阶段做的操作也会执行，但改变值的操作不会生效
   destroyed() vm实例及自身数据已经销毁(内容仍然保留)且断开与其他组件的连接 ，this.$destroy()用来触发销毁 ,销毁后当前组件的自定义事件都会失效 ，原生事件不受影响
   
   初始化显示
   beforeCreate()
   created()
   beforemount()
   mounted()
   
   更新数据
   beforeUpdate()
   updated()
   
   销毁vue实例
   beforeDestroy()
   Deswtroyed()
  */
  ```

  

## 1.3 过渡&动画

+ 动画

  ```js
  /*
   1. 需要使用vue特定api标签<transition/>包裹 ， 
   
   2.transition标签属性
    name
       transition标签有一个name属性，当被指定时，动画样式前缀名名也必须跟着变，
       如 <transition name="my"/ > 则操作动画的类名必须为 my-enter-active my-leave-active  
       如果没有指定name默认为 v-enter-active  v-leave-active
    appear
       可以指定页面第一次渲染时是否触发此动画，是一个布尔值 <transition :appear="true"/>  或 <transition appear/>
   
   注意： transition为vue标签，作用是给vue来标识这需要用到动画元素，在渲染中，这个标签不会被渲染
   	   transition标签只是在合适的时机调用合适的动画，关键动画还是需要自己完成
   
    xxx-enter-active 已经进入，产生变化中
    xxx-leave-active 正在离开，产生变化中
    
   3. transition-group
   	如果有多个标签需要用到过渡或动画必须使用 <transition-group>标签，且被包裹元素需要有一个唯一key标识符
   	如 <transition-group>
   		<h1 key="1">你好</h1>
   		<h1 key="2">我是</h1>
   	</transition-group>
  */
  ```

  ![image-20210902103135371](C:\Users\xx\AppData\Roaming\Typora\typora-user-images\image-20210902103135371.png)

+ 过渡

  ```js
  /*
    使用v-enter v-enter-to , v-leave v-leave-to 来帮助完成过渡，
    开始时：在 v-enter 设置起始状态  在 v-enter-to 中设置最终状态 
    结束时：在 v-leave-to 设置起始状态  在 v-enter 中设置最终状态，因这是一个状态的两面大致样式一定相同可以用并集选择器获取
     做动画标签设置transition: .4s all 样式开启过渡 或给v-enter-active和v-leave-active添加过渡，两者效果一致  
    
  */
  ```

+ 好用的动画库

  ```js
  /*
   npm 中的 Animate.css 库
   https://animate.style/ 官网
   安装 npm i animate.css   // 为一个样式文件 
   
   使用
   import 'animate.css' //直接以css引入即可
   <transition-group
    appear    // 第一次渲染时触发
    name="animate__animated animated__bounce"  //使用该库必须用指定名
    enter-active-class="animate__swing"  //在行类指定进入样式
    leave-active-class="animate__backOutUp" // 在行类指定移出样式
   >
   <h1>我是</h1>
   <1>time</h1>
   </transition-group>
  */
  ```

  

## 1.4 过滤器&插件

对要显示数据做格式化处理,不会改变原有数据，而是产生新的对应数据

+ 基础语法

  ```js
  /*
  1. 定义过滤器
      Vue.filter(firstName,function(value[,arg1,arg2,..]){return newValue}
  2. 使用过滤器
  	<div>{{myData|filterData}}</div>  第一个参数（myData）为传入过滤器函数的形参，第二个参数为过滤器函数，过滤器可以传入参数，会被放置在[,arg1,arg2,..]中
  	
    如 
    	<div id="app">
      <p>{{currentTime}}</p>
      <div>{{currentTime|timeFilter}}</div>
      <div>{{currentTime|timeFilter('MM-DD')}}</div>
      <div>{{currentTime|timeFilter('HH-MM-ss')}}</div>
    </div>
    
    <script>
      Vue.filter('timeFilter', (value, time = "YYYY_MM_DD_HH_MM:ss") => {
        return moment(value).format(time)
      })
      new Vue({
        el: '#app',
        data: {
          currentTime: Date.now()
        }
      })
    </script>
    
    3. Vue实例内配置filters
    new Vue({
    	el:"#root",
    	filters:{
    		timer(value,time='YYY年MM月DD日'){ //第一个参数接收原 | 的第一个形参，第二个参数接收过滤器函数传入的形参
    			return dayjs(value).format(time)
    		}
    	}
    })
    
    4. 过滤器串联写法
    <div id="root">{{time | timeCompute('111') | total}}</div> // 把time传给 timeCompute('111'),作为第一个形参，'111'作为timeCompute的第二个形参，这个过滤器函数计算完后，再把返回值传给 total 过滤器函数作为第一个形参，计算完后返回给root标签显示
    
   5. 写在Vue实例里的过滤器被成为局部过滤器，写在Vue原型里的过滤器为全局过滤器
    	全局过滤器创建，必须写在实例前
    	Vue.filter('函数名',function(value){
    		reutrn 111
    	})
   6. 过滤器可以应用在插值语法和单项数据绑定中，通常做一些简单处理
  */
  ```

  

+ 插件

  ```js
  /*
   引入插件包后需要 声明使用该插件包 Vue.use(MyPlugin) 这样才会对其内部的属性或方法进行解析使用
   
  */
  ```



## 1.5 Vue组件

组件是所有用到的资源的集合(包括html,css,js等)

 在vue框架下的html中 :开头的一般都是vue的动态属性或方法，采用 :xx=""方式声明。""没有实际意义,可理解为起包裹作用，不带表字符串，如果想表示字符串换必须使用 :xx=" 'xx方式' "

所有Vue组件都被vue以默认暴露的方式在底层呈现，使用时直接以默认方式引入即可

+ vue单文件组件

  ```js
  /*
   <template>
   	<div>xxx</div>
   </template>
   <script>
   	export default {
   		props: []/[],
   		data(){},
   		computed:{},
   		methods:{},
   		watch:{},
   		filters:{}
   		directives:{}
   		components:{}
   	}
   </script>
   <style scoped>
    xx
   </style>
  */
  ```

+ 组件基本概念

  ```js
  /*
   1. 组件:实现应用局部功能代码和资源的集合
  
   2. 非单文件组件：一个文件包含n个组件，单文件组件：一个文件只包含有一个组件
   
   3. 组件和Vue实例基本相同，但组件没有el配置项，因为组件最终都是由vue实例管理，层级关系 vue原型>vue实例>组件构造函数>组件实例 
   
   4. const x =Vue.extend({}) 可以创建一个组件，该组件没有el配置
   
   5.注册(引入)组件
   局部注册组件
   new Vue({
   	components:{
   		x
   	}
   })
   全局注册组件
   Vue.component('xxx',xxx)
   
   6.引入组件后使用组件标签推荐方式 
    	 （1） 全小写 myschool  大驼峰(只能在脚手架生效) MySchool 用-连接 my-school
       （2）在组件中可以使用name配置项来指定组件在开发工具中呈现的值
       （3）在html环境中把组件标签自闭和会出现显示问题，脚手架中不会
       （4）在html页面中组件也可以不写extend去生成，内部会自动做判断处理 
       如 s = {
       	template `123`,
       	data(){
       		return{
       			a:1
       		}
       	}
       }
   7. 每个组件的生成都是经过extend函数生成的，这个函数返回一个组件实例对象和一个每个组件都不同的地址值，所以每个组件实例的数据不互通，有各自独立的地址 . 组件的实例对象简称 vc ，vue的实例对象简称vm . vm管理着vc对象，可以在$children中查看
   
   8. 构造函数prototype和__proto__
   	只有构造函数和函数拥有prototype显示原型和__proto__隐式原型，其他类型数据(包括class,obj,arr等)只有__proto__属性(查找其构造者)。经过构造函数生成的实例其只有__proto__属性，且该属性指向构造函数，然后构造函数本身也有隐式原型...
   	
  9. 组件.prototype指向其构造函数，他的.prototype.__proto__ 则指向Vue原型(vue对其原型进行了重新指向)，通过 组件.prototype.__proto__可以直接连接到vue原型上，这标识着可以使用vue原型上的属性和方法
  */
  ```

  

+ 定义组件

  ```js
  /*
  1.定义组件 
  在创建组件时在最外侧必须有一个父盒子包裹
    Vue.component('button-component', {
        data() {
          return {
            count: 0
          }
        },
        template: `
          <button @click="count++">点击了{{count}}次</button>
          <p>{{count}}</p>
         
        `
      })
   
   2. 使用组件 
    <div>
    	<btn/>
    </div>
    
    3. 注意点
    	（1）创建的组件只有在指定的el范围内可使用
  */
  ```

  

+ props向子组件传递数据

  ```js
  /*
   props是在 Vue.component创建组件时的一个配置对象，它可以动态接收在html中传入的数据，并可以在组件中使用，这个数值会被放置在vue原型下和props下
   当父组件向子组件传递数据时用 :xxx="xxx" 方式，同时子组件接收这个值时，要进行接收声明，props:['xxx']
   1。props的使用
   	
   	<div id="app">
   		// 在组件行内传入对应值，这个值可以是变量
      	<button-component :title="1" :one="'我是props传递的参数'"></button-component>
    	</div>
   	
   	 Vue.component('button-component', {
        props: ['title', 'one'], //声明props下和vue原型下的属性
        template: `
          <div>
            <button @click="title++">点击了{{title}}次</button>
            <p>{{title}}</p>
            <div>{{one}}</div>
          </div>
         
        `
      })
      new Vue({
        el: '#app'
      })
   2.props使用注意点
   	（1）props声明key后，在html使用组件的行内里，必须传入相同Key的值以 :key="xx"方式
   	（2）props行内声明的值可以和vue设置的data状态进行交互
   	
   3.props对象声明方式	
   export default {
   	props:{
   		xxx: Object(声明数据类型)
   	}
   }
  */
  ```

  

+ .vue后缀文件

  ```js
  /*
   vue独有的文件 一般由 模板 样式 Js 三部分组成 ，
   vue文件有独有的高亮和语法提示，帮助底稿编写效率
  */
  ```

  

+ webpack打包文件

  ```js
  /*
   当把webpack局部安装时，想要执行webpack需要使用 npx webpack ，来执行webpack
  */
  ```
  
  



+ vue脚手架使用

  ```js
  /*
  
   npm config set registry https://registry.npm.taobao.org 切换下载源
   想要使用vue脚手架需要全局安装@vue/cli包
   1.  安装包：npm i @vue.cli -g
   2.  创建脚手架：vue create myVue
   3. 选择vue脚手架版本
   4. 在package的scripts中 serve用于develop中进行编译运行，build用于product中进行提交并把vue文件拆分为js html css文件,lint用于语法检查
   5. index.html配置
   	(1)<meta http-equiv="X-UA-Compatible" content="IE=edge"> //對症IE六老年期的一個特殊配置，表示让ie浏览器以最高渲染级别渲染
   	(2) <noscript></noscript> // 页面不支持js时执行此标签内的内容
   6. main.js中的render: h=>h(App)
   	因为vue默认引入的是一个残缺版的vue(vue.esm.js)他只能识别render进行渲染
   	如果你想以模板的方式渲染，需在该文件夹内引入../vue.js文件
   	render函数用于渲染页面，带一个环境形参reacteElement,该形参可以解析选定组件并渲染到页面上，其渲染结果需要返回，所以就精简为 render: h=>h(App)
   	vue.esm.js专门负责编译。而vue.js则是综合版什么都能做，但提交较大，在打包时，把编辑器等不需要的功能也会进行打包，这会造成资源的浪费，所以最好使用vue模板默认使用vue打包版本
   	
   7. vue inspect > output.js , 此命令可以把vue设置的webpack配置项打包成一个指定文件  ， 在vue官网中，出现在配置参考中的参数才可以修改  
   	如果想要自定义一些可选配置，需要在package.json同级下创建一个vue.config.js进行配置，具体可配置内容可安靠vue-cli官网
  */
  ```
  
  

+ ref属性

  ```js
  /*
   1.通过给标签添加一个ref="xxx",就可以收集此标签到$ref中，使用this.$ref.xxx就能获取该DOM标签 
   如 <h1 ref="title">我是标题</h1>  使用 this.$ref.title 即可拿到此h1DOM元素
   2. 给一个标签元素添加ref='xxx'时，使用时拿到的是标签元素，给一个组件添加ref='xxx'时，使用时拿到的是一个组件实例对象(可以使用其属性和方法)
   3. ref属性用来给元素或子组件注册引用信息
   
   应用
   	配合input框可以获得焦点 this.$refs.inpuTitle.focus() ,但因为vue是把当前包含块的语句都执行完才会渲染页面，所以this.$refs.inpuTitle.focus()不能获得焦点，需要把他转换为一个异步任务则可以，如在其外部加一个100毫秒的定时器
  */
  ```



+ props属性

  ```js
  /*
   props用来完成父向子通讯 ，传递方式以 标签属性方式传递，接收方以props属性声明接收 。 
   传递方式有两种：
   	name="xxx" 单纯的字符串传递值  ，传递的数据是一个字符串类型
   	:name "xxx" js表达式传递值 传递的类型是一个js表达式或vue属性
   	
   接收方式有：
   	props:['xxx','name'] //只声明，不限制传入类型
   	props:{  // 声明，且明确接收类型，还有其他配置项可选
   		name: Number,
   		xxx: String
   	}
   	props:{  // 声明，且严格限制接收
   		name:{
   			type:Stgring,
   			required: true //必填项
   		}，
   		xxx:{
   			type: Function,
   			default: 2   // 默认值，没有传递参数时采用此值
   		}
   	}
   	
   注意
   	接收到的props数值不能修改，如果想要对其显示值进行操作，可以内部声明属性接收那个props传入的数据，然后对声明属性进行修改。(自己组件修改自己组件的数据)
   	props接收项不能声明没有传入的参数，这样值为一个undefined
   	如果外部props接收一个变量内部已经存在，则外部接受的优先级更高
   	数据修改原则：自己组件内的数据的变量自己控制，不要在自己组件修改其他组件的数据
   	vue关键词(ref,key等)不能作为props值进行传递，其余的props都可以声明接收，且可以接收 :xxx和xxx方法，:xxx传递的是一个js表达式，xxx方式传递的是一个字符串
  */
  ```



+ mixin（混合）

  ```js
  /*
   mixin混合（复用配置）可以把多个组件相同的代码封装成一个单独的js，然后把这个js文件暴露出去，给需要使用混合文件的引入 并进行使用声明 mixins:[xxx]
   如 minxin.js   
   	export const mix = {
   		method:{
   			eating(){
   				console.log('我正在吃饭')
   			}
   		}
   	}
   	然后其他使用到该方法的组件可以进行引入 ，并在组件配置中进行mixins:[min]声明，声明后即可使用混合里的属性或方法
   	
   全局混合
   	直接挂载到Vue原型上，则所有组件和实例都可以使用混合里的语句
   	Vue.mixin(xxx)
   注意
   	生命周期钩子不受混合影响，如果混合里写了声明周期钩子，则混合里的先触发，后自己的再出发
   	查找顺序：如果使用过了混合先去混合查找，再去当前vue组件或vue实例查找，后面覆盖前面的，声明周期钩子不会被覆盖
  */
  ```
  



+ 插件

  ```js
  /*
   插件：用于增强Vue的功能，包含一个install方法的对象，install第一个参数是Vue,第二个参数以后是插件使用这传递的数据
   定义插件
   	Obj.install = function(Vue,option){Vue.filter(...)}
   	
   使用插件
   	在引入了插件后，使用use.use(xxx)使用插件
  */
  ```

+ scoped

  ```js
   /*
   	scoped用于解决每个Vue组件之前的命名冲突，在style中书写了scoped属性后，该组件内的所有标签就会被vue分配一个独一无二的属性值(data-xxx-xxx)，在用属性选择器选择他们的杜伊属性名,用来解决每个组件可能出现的命名冲突
   	
   	scoped通过给该vue中所有的标签添加独一无二的属性，然后在给该标签书写样式时，添加上属性选择器来替换原来的选择器，从而解决每个组件中可能出现的命名冲突
   */
  ```

+ style

  ```js
  /*
   在vue中style标签有一个language可以设置当前书写的语言，默认为css,可以设置为 lang="less" 表示用less书写
   如果less无法编译，表示vue没有下载 less-loader 需要使用 npm i less-loader 下载，如果下载失败则去下载之前版本即可， 如 npm i less-loader@7
  */
  ```




+ 自定义事件

  ```js
  /*
   绑定自定义事件
   this.$on('事件名',函数)   || this.$once('事件名',函数) 此事件只触发一次      
   
   触发自定义事件
   this.$emit('事件名'[,形参...]) 传入形参数需和事件函数的形参数一致，否则会出现undefined ,或形参使用...运算符
   
   解绑自定义事件
   this.$off('事件名') 解绑事件，这种方式一次只能解绑一次 
   this.$off(['事件名','事件名']) 解绑多个事件
   this.$off() 解绑所有自定义事件
   
   应用： 
   	在事件总线中，用于绑定和触发自定义事件
   	在一个标签或子组件上声明一个自定义事件(v-on:xxx="函数")，在经过某种手段使用  this.$emit('事件名',形参) 触发
   	自定义组件实现子组件向父组件通讯(在该组件身上绑定自定义事件，然后操作函数写在父组件上)，当子组件触发某种行为时，带动$emit触发该自定义事件，从而完成子向父通讯
   	props实现父向子组件通讯，父组件可以在自福建上传递非关键词属性，子组件可以通过props
   	
   	
   注意
   	自定义事件和系统自带事件一样都是放在底层的，所以打印组件时看不到
   	自定义事件只能通过this.$emit('事件名')方式触发
   	谁触发的自定义事件这个this就指向谁(仅限 this.$on('xx',function(){}写法)
  this.$on('xxx',函数) 写发this不受影响 ，因为写在method里的方法this指向本组件(vue内部改变了this指向) ，使用this.$on('xx',()=>{})写法,this指向向外找，如果是在声明钩子中this指向该组件实例
  	在一个组件中(<Stu @click="demo"/>)绑定原生事件，以@click="函数"方式绑定，vue会解读为自定义事件，仍然需要this.$emit('click')触发 。 以@click.native="函数"方式绑定，才可以成功给组件绑定原生click事件(给该组件的外层包裹元素绑定)，当点击该组件所在区域时，就会触发该事件
  */
  ```
  



+ nextTick

  ```js
  /*
   使用this.$nextTick(函数) 定义
   nextTick函数会在页面渲染后自动执行
   
   应用
   	获取input焦点信息 ，vue渲染页面是在当前语句快执行完后才开始的，所以如果想要获取input焦点则会失败，因为input还未挂砸到页面 ，这是使用this.$nextTick(()=>{input.focus()}) 即可解决 
  */
  ```

  

## 1.6 webpack配置vue

input 一般都为双向数据绑定

input:check  复选框选择中 v-model 控制checked属性

+ webpack配置

  ```js
  /*
   1. 局部下载 webapck && webapck-cli  npm i webpack webpack-cli -D,搭建webpack环境
  
   2.. 创建一个webpack.config.js 用于书写webpack对象配置，使用commonJS语法 
   
   3. 开发环境运行：自动打包运行webpack  npm i webpack-dev-server  -D
    在package.json中的script下输入以下操作
    	 "scripts": {
          "build": "webpack --mode product", // 输入 npm run build 开启生产模式
          "dev": "webpack-dev-server --mode development" // 输入 npm run dev 开启开发模式
    },
    在 webpack.config.js 中 添加 配置项
        // 配置静态服务器
        devServer: {
          port: 8000,  // 设置打开湍口
          open: true, // 是否自动打开
          quiet: true, // 减少日志提示
          proxy:{ // devServer自带proxy代理
          
          	'/api':{ // 匹配以/api开头的请求
          		target:'http://localhost:4000', // 转发目标地址
          		pathRewrite:{'^/api':''}, // 转发请求前去除路径的/api
          		changeOrigin: true // 支持协议名跨域
          	}
          }
        }
        
    4. source-map 调试，可以映射到那个文件的那个位置出现了错误，而不是直接在打包文件提示
  */
  ```



+ plugin配置

  ```js
  /*
   1.  局部下载  html-webpack-plugin ，npm i html-webpack-plugin -D,自动解析html ，是一个构造函数，书写在 plugins:[new HtmlWebpackPlugin({template:'index.html' // 绑定scr})]中
   
   2.  局部下载 clean-webpack-plugin, npm i clean-webpack-plugin -D,是一个构造函数，书写在 plugins:[new CleanWebpackPlugin() //清除dist目录]中
  */ 
  ```

  

+ loader配置

  ```js
  /*
    1. 打包处理ES6
    npm i babel-loader @babel/core @babel/preset-env -D
    在module:{rules:[
    	{
    		test:/\.js$/, // 设定可匹配类型
    		exclude:/{node_modules|bower_components}/, //忽视xx文件夹下
    		include:[path.resolve(__dirname,'src')]// 处理包含的文件
    		use:{
    		loader: 'babel-loader', // 使用loader
    		option:{
    		presets:['@babel/preset-env'] // 提前设置值
    		}
    		}
    	}
    ]} rules中设置 
    
    2. css 打包处理
     下载依赖包：npm i css-loader style-loader 
     配置
     	{
     		less:/\.css/,
     		use: [
     		// 操作顺序从吓到上，从右到左
     		'style-loader', // 将js中的css转换到style中
     		'css-loader'],   // 将css打包到js中
     		
     	}
     	
     3. 图片打包器
     下载依赖包：yarn add -D url-loader file-loader
     file-loader 不会进行base64处理，而  url-loader 会进行base64处理，但其依赖于file-loader包
     {
     	test:/\.pgn|jpe?g|git|svg$/,
     	loader:'url-loader',
     	options:{
     	limit: 10*1024, // 小于10k的图片进行base64处理
     	name:'static/img/[name].[hash:7].[ext]' // 相对输出图片位置 ,[name]为匹配任何id并原样输出，[hash:7]表示取hash值前7位，[ext]表示匹配任何格式，并原样输出
     	}
     }
  */
  ```

  

+ vue&&webpack

  ```js
  /*
  1. webpack适配
   下载包： npm i vue-loader vue-template-compiler -D
    npm i vue
    塔尖vue服务器
    选用 const VueLoaderPlugin = require('vue-loader/lib/plugin') 插件
    在plugin中 new VueLoaderPlugin()添加
    在modules中的rules下添加{test:'/\.vue$/',loader:'vue-loader'}
    在原cssloader中将 style-loader该外vue-style-loader，此vue-style-loader是style-loader的加强版本
    
   2. xx.vue组成
   由 template,script,style三部分组成 
   script 中要使用默认暴露方法，暴露script脚本
   然后Index.js引入这个Vue组件
  */ 
  ```

  

+ vue组件详情

  ```js
  /*
  注意
  引入组件在Script下进行，components要在export default 中声明
  export default{} 中书写的内容和 new Vue({})中的内容有很多相似 ，除了el:xxx
      <template>  // 用于书写模板
        <div></div>
      </template>
  
      <script type="text/ecmascript-6">  //用于引入和暴露模块
      import xx from 'xxx' //引入其他组件，这是可以使用其data里的参数
     
      export default {
      		components:{} // 引入组件后必须声明才可使用，此声明方式为局部声明，表示该组件只能在当前组	件中使用 .  若想全局声明需在index.js下进行声明
      };
      </script>
  
      <style scoped>  //用于给模板标签书写样式
      // scoped的原理，当前组件标签会添加一个特定的属性名，样式选择器在对其进行选择
      xxxx //给template内标签添加样式
      </style> 
    
    1. 关于注释
    在template和style中，只能使用 <!---->注释
    在script标签中可以使用 // 
    
    2. 关于引入汇总
     在index中进行汇总，需引入 import Vue from 'vue' 帮助编译，还需引入用到的组件块，然后创建 (1) new Vue({				进行渲染
                el: "#root",  // 选择#root标签范围，进行渲染
                render: h => h(App) // h是一个环境形参，是一个函数，用来返回解析渲染指定组件，韩式调用后的返回值必须 return转交出去
              })
     	
     	(2) 使用第二种渲染方式
     		new Vue({
     			components:{
     				App
     			},
     			template:'<App/>'
     		})
     		render性能比第二种渲染方式好，高效且简短
     		直接书写是会报错，原因是此方法不会自动寻找vue-compiler编译器，而render:xx会自动寻找vue-compiler编译器，解决方法，让webpack打包时引入代编译器的版本，在alias中设置  alias: {							，在引入时以@comps/App即可
        '@': resolve('src'), // 找到src目录
        '@comps': resolve('src/components'), //找到components目录
        'vue$': 'vue/dist/vue.esm.js'
      },
     	
    3、组件间通讯
    在script标签下引入其他组件并在下方进行components:{xx}组件使用声明
  */
  ```

+ 模块解析webpack

  ```js
  /*
   // 模块引入解析 和modules,plugin同级
    别名配置优先级高
    resolve: {
    alias: {
    	'@':xxx //  可以给xxx起一个别名调用@就是调用xxx,这样可以减少代码编写，加快打包
    }
      extensions: ['.js', '.vue']  // 碰到.js 和.vue的文件夹可以不带后缀进行解析
    }
  */
  ```

+ es7+语法兼容

  ```js
  /*
   使用 @babel/polyfill 包， npm i @babel/polyfill 
   引入: 可以直接在index.js中直接 import '@babel/polyfill'(不推荐手机用) ,虽可以解决兼容问题，但此引入方法占用内存较大，有大量不需要用的包
   webpack配置(推荐使用)：  
   presets:[
   	['@babel/preset-env',{
   		useBuiltIns:'usage',
   		'corejs':2 // 处理一些新语法
   	}]	
   ]
  */
  ```

  


+ 数据查找规则

  ```js
  /*
  1.  在组件开发中，用到的对象会先在组件模板中找数据 。组件对象的功能和vm一致，组件对象相当于是一个小的vm对象，管理的是他的组件，在Index中是大的vue对象，管理的是整个模板 
  2. 一个组件和另一个组件通讯时(props传参)，实际上是把自己组件对象上的一个属性或方法，挂载到另一个组件对象上，另一个组件在使用时，必须对其进行接收声明 及 props:['xx','xxx'] ，在组件中使用的vue对象下的数据，必须省略this
  
  3. 数据在哪个组件，那个组件就提供修改数据的函数
  */
  ```

  

+ Arrary.reduce方法

  ```js
  /*
   reduce方法用来做逻辑累加，arr.reduce((previousItem,item)=>(),0) ,第一个是累加函数，第一个参数previousItem是累加值，没有累加值则使用第二个参数给定的值，第二个参数item是数据的下一项，如 arr = [1,2,3]  arr.reduce(preItem,item=>{return preItem+item},0) 结果为 6
  */
  ```

  

+ deepWatch

  ```js
  /*
   deepWatch用在复杂数据类型中，当其改变时调用
  watch:{
  	todos:{
  		deep:true,   // 深层监视
  		handler: saveTodo   // 调用外部引入插件，函数底层进行传值
  	}
  }
  */



+ vue组件通讯基本原则

  ```js
  /*
   父组件传过来的data数据(和React State类似) 只能读取不能对其值进行修改，所以想要修改其父组件的data时，可以传递一个函数，然后在其他组件中进行调用，来修改，这样就符合原则，即自己的data用自己的function修改
  */
  ```

  

## 1.7 组件间通讯

+ 常用方法

  props

  vue的自定义事件

  消息订阅与发布(PubSub)

  slot

  vuex



自定义事件

```js
/*
 绑定自定义事件
 this.$on('事件名',函数)   || this.$once('事件名',函数) 此事件只触发一次      
 
 触发自定义事件
 this.$emit('事件名'[,形参...]) 传入形参数需和事件函数的形参数一致，否则会出现undefined ,或形参使用...运算符
 
 解绑自定义事件
 this.$off('事件名') 解绑事件，这种方式一次只能解绑一次 
 this.$off(['事件名','事件名']) 解绑多个事件
 this.$off() 解绑所有自定义事件
 
 应用
 	在事件总线中，用于绑定和触发自定义事件
 	在一个标签或子组件上声明一个自定义事件(v-on:xxx="函数")，在经过某种手段使用  this.$emit('事件名',形参) 触发
 	自定义组件实现子组件向父组件通讯(在该组件身上绑定自定义事件，然后操作函数写在父组件上)，当子组件触发某种行为时，带动$emit触发该自定义事件，从而完成子向父通讯
 	props实现父向子组件通讯，父组件可以在自福建上传递非关键词属性，子组件可以通过props
 	
 	
 注意：
 	自定义事件和系统自带事件一样都是放在底层的，所以打印组件时看不到
 	自定义事件只能通过this.$emit('事件名')方式触发
 	谁触发的自定义事件这个this就指向谁(仅限 this.$on('xx',function(){}写法)
this.$on('xxx',函数) 写发this不受影响 ，因为写在method里的方法this指向本组件(vue内部改变了this指向) ，使用this.$on('xx',()=>{})写法,this指向向外找，如果是在声明钩子中this指向该组件实例
	在一个组件中(<Stu @click="demo"/>)绑定原生事件，以@click="函数"方式绑定，vue会解读为自定义事件，仍然需要this.$emit('click')触发 。 以@click.native="函数"方式绑定，才可以成功给组件绑定原生click事件(给该组件的外层包裹元素绑定)，当点击该组件所在区域时，就会触发该事件
*/
```





+ 绑定事件方法

  ```js
  /*
  1. props传递
   <Mid :todo="todo" /> // 把 todo 传递给Mid Mid需要 props 声明后使用    {props:{todo: Object}}  // Mid组件内 Props声明
   
  2. 分发定义事件
   <Mid @todo="todo2"></Mid> //  把todo分发给Mid组件
   this.$emit('addTodo2'，'返回参数') //  Mid组件中，接收事件，如果该分发的内容需要接收数据，则可以在第二个参数中进行返回
   
  3.绑定自定义事件监听(最好和mounted配合)
  <Mid ref="Mid" />  // 把该组件收集到本组件的vue下
   this.$refs.Mid.$on('addTodo2',this.addTodo) // 给Mid绑定一个addTodo2属性，该属性值为this.addTodo  即{addTodo2:this.addTodo}
   
  4. 解绑自定义事件监听(通常卸载beforeDestroy)
  beforeDestroy(){
  	this.$refs.header.$off('addTodo2') // 事件解绑
  }
  */
  ```







+ Vue组件对象和Vue的关系(全局事件总线)

  ```js
  /*
   1.组件对象不是Vue的实例对象，他是VueComponent的实例，组件对象的原型对象是一个vm对象(Vue实例对象) ，这个实例对象每个组件都有且不互通，在在其上的是Vue显示原型，Vue显示原型下的值所有组件互通 this.__proto__.__proto__.a = xxx
   
   2.每个组件本质上为 ComponentVue他的显示原型是Vue的实例对象(也就是vm，vm实例之间属性和方法不共享),其隐式原型才是Vue对象，Vue对象下的属性和方法全组件都可以使用
   
   3.组件属性值查找规则：现在自身上找，再到vm原型上查找，在到Vue上查找
   
   4. 给Vue原型对象添加一个属性(专业术语：全局事件总线(globalEvengtBus))
    Vue.prototype.$globalEventBus = new Vue() //index.js页面添加， 在Vue对象上挂载一个Vue实例，因为vue实例才有$on等操作，这样所有vue都能拿到数据.或在new Vue({})中直接绑定 
    new Vue({
    	beforeCreate(){ //在属实话变量之前就要完成绑定，避免污染
    		Vue.prototype.$globalEventBus = this   
    	或   this.__proto__.$globalEventBus = this   
    	}
    })
    
    this.$globalEventBus.$on('data',this.data.todo) // 给事件总线绑定data事件
    this.$globalEventBus.$emit('data',this.index) // 在需要用到data数据的地方调用，并且传入参数
    
    注意： 
    （1）全局事件总线的$on多用于绑定一个函数，然后$emit把对应的函数执行并在第二个形参上传入值并调用原函数
    （2）数据总线可以用于绑定需要传递的数据，如 this.__proto__.__proto__.$a=1
    
  */
  ```





+ slot

  ```js
  /*
   1. 基本使用
     默认插槽
   	slot标签为vue内置标签，在父组件使用子组件时，可以在该子组件中插入html标签及内容，这些标签默认会被slot标签接收，当该子组件使用slot标签时，显示的就是传入的标签
  如在 App 父组件中： <Cat><h1>我是cat</h1></Cat>  //在app组件中给，Cat子组件声明默认插槽      Cat 组件中 <div><slot>我时内容</slot></div> //显示 我是cat 当没有定义插槽时，slot标签会输出他所包含的内容，如果给定了slot内容，则会显示传入的内容
     
     具名插槽
       指在定义插槽时，就给该标签指定对应的插槽，在子组件使用时，需以 <slot name='xx'></slot>使用 
       如 <template slot="aa">  //template标签也会为其他的包裹抱歉，如div，不过template标签不会被渲染只做包裹元素，div会被渲染到页面
       	<h1>我是templ</h1>
       	<h2>我是t</h2>
       </template>
       // template标签使用插槽写法还可写为 <templte v-slot:xxx></template>,只有template能这么写，其余标签只能slot="xx"
     
     作用域插槽
       在子组件中使用slot标签可以以 <slot :value="value" :result="result"></slot> 把自己组件的属性或方法传递给定义该插槽的父组件，方便父组件完成操作 。 父组件接收时，必须使用template包裹指定元素，且在其中添加 scope="value" 或slot-scope="value"接收传入的属性或方法 如 <templte scope="bus"></template> ,这个value实际上是一个对象，可以使用解构赋值解构自己需要的数据 <templte scope="{value,result}"></template> ，或使用bus.value bus.result即可获得传入的数据，同时bus下可以接收多个数据
       
  注意
  	在定义插槽时，子组件不能自闭合，只有<Ca></Ca>方式的组件才能指定插槽
  	默认插槽只能有一个，所有在组件中没有给定插槽名的标签都会被默认插槽接收
  	 vue解析插槽时,是把对应的插槽类别放到对应的指定插槽中在进行整合渲染，如果对应插槽内容有包裹元素，也会把包裹元素一起放到指定插槽中
  	作用域插槽传递子组件自身数据时，父组件进行接收，且会放到一个专门的对象里，父组件使用 scope="x"本质上就是把所有的属性和方法放到了该x下，所以也可使用结构赋值进行处理
  */
  ```

  




+ slot

  slot为vue内置的标签 ， 如果slot标签其行内没有任何属性则其为默认插槽，默认插槽只能写一个，如果有多个默认插槽则需要给其设置name属性，给定name属性的插槽成为命名插槽

  插槽用于传入不同的标签结构

  插槽的使用

  ```js
  /*
   1.slot值的传入是在父组件中(如 app)进行并解析后传入给子组件的,被传入的slot标签应被子组件包裹,slot写在标签行内如 
   <Footer>
   <button slot="btn">我是slot</button>  // 我是name为btn的 slot标签内容(简介写法) 该子组件使用此内容是因使用 <slot name="btn"/> 方式
   <template slot="temp">
   	<p>我是原始p</p>  // 此slot写法为初始slot写法，用法和第一种一致 ，在使用时 。子组件书写 <slot name="temp"/> 即可使用该内容
   </template>
   <div>我是div</div> // 我是默认插槽，只允许有一个  该子组件使用此内容是因使用 <slot /> 方式
   </Footer>
   
   2. slot内可以书写其他标签，在父组件没有传递slot时，则会使用slot内书写的标签
  */
  ```

  

+ PubSub

  ```js
  /*
   PubSub是一个 三方包，需要使用 npm i pubsub-js
   和React中的用法一致
   PubSub.publish('暗号'，'数据') ， 数据可以为任意变量类型，接收方使用 PubSUb.subsribe('暗号',(msg,data)=>{}) 接收，暗号需和发出的暗号保持一致，第二个参数是一个函数，第一个形参msg为暗号，第二个形参data为传递的数据
   pubsub.unsubscribe('xx') 取消监听
   如   let pub = PubSub.publish('data',666)
   	  PubSub.unsubscribe(pub)  // 取消监听
  */
  ```
  
  

## 1.8 vue-ajax

+ vue库

  vue-resource(vue插件。非官方库，vue1.x使用广泛)

  axios,通用的ajax请求库，官方推荐，vue2.x使用广泛

  

  

  

+ vue-resource使用

  ```js
  /*
   vue-resource本质上一个vue专属包，手机用是必须使用Vue.use(vue-resource)进行解包，这时会往Vue上添加一个$http属性，用来发请求
    $http的使用
    this.$http.get('https://xxx.com')  //使用this.$http.请求方式('地址')即可发请求，不能携带参数
    	.then()  // 请求成功执行.then的第一个函数，请求失败执行.then的第二个函数
    	.catch() // 失败也可执行.catch函数
  */
  ```



+ vue设置代理解决跨域问题

  ```js
  /*
   跨域问题
   指违背了同源策略，即协议名，主机名，端口号必须一致 ，当其上一个不满足时，响应信息就会被浏览器拦截(能发出数据到服务器，服务器也返回了数据，但是被浏览器拦截了)
   
   解决跨域
   	后端设置cors
   	jsonp ，使用script标签的src不受同源限制来解决 ，只能解决get问题
   	配置代理服务器 ，代理服务器默认和前端地址保持一致，在使用时，代理服务器直接指向目标服务器的地址即可,当前端地址访问当前服务器数据时，代理服务器会进行一个转接
   	
  
   配置vue代理服务器
   
    方式一
   	在vue.config.js中配置  devServer:{
   		proxy: 'http://localhost:5000' //不屑请求点缀，默认转发这个地址
   	}
   	在app.vue中配置
   	axios.get('http://localhost:8080/student').then().catch()
  	即可获取数据，代理服务器默认和前端服务器地址保持一致，在vue.config.js中配置的是转接的服务器，即当我访问http://localhost:8080/student时，实际上是在访问http://localhost:5000/student
  	
    方式二
       在vue.config.js中配置  devServer:{
   		proxy:{
   		 // 只有包含请求前缀/req的才会被代理服务器转发，注意转发过去的地址仍有/req这会导致转发服务器匹配不到资源，因此proxy验证完毕后，需要把/req处理掉
   		 // /req路径前缀可以为任何值
   			'/req':{  
   				target:'http://localhost:5000',
   				pathRewrite:{'^/req':''} , // 匹配所有/req重写为''这样就处理了proxy请求前缀，可以正常的请求转发
   				ws:true, // 用于支持websocket
   				changeOrigin: false // 是否改变代理服务器地址(默认值为true)，是一个布尔值，如果为true表示随着请求域名的变化而变化，false表示源地址不变化             如 8000服务器查找数据，代理服务器帮忙转接到5000服务器，此时代理服务器的地址改变为5000，这样有效果规避了该5000服务器的检测，如果为false，可能会被该服务器拦截
   			},
                '/c': {
                  target: 'http://localhost:5001',
                  ws: true,
                  pathRewrite: { '^/c': '' },
                  changeOrigin: true,
                }
   		}
   	}
   	在app.vue中配置
   	axios.get('http://localhost:8080/student').then().catch()
  	
  	
  注意
  	前端服务器根目录为public
      前端服务器发起请求时，如果该服务器配有对应的资源，则会使用自己的资源，而不会去找代理服务器帮忙查找，如果没有匹配资源，则去找同源的代理服务器帮忙查找(去它所指向的域名查找)，如果找到返回资源，如果没有则报错。资源查找本质上是查找路径上的匹配资源
      代理服务器本质上就是一个同源的请求转接者，有效的规避了同源策略，而拿到另一台服务器的数据
      代理服务器只能配置一个(方式一)
      想配置多个代理直接要求匹配一个唯一路径前缀即可(为了获得数据，在proxy匹配成功后，需要自己删除前缀)
      代理服务器更改配置后，需要重新vue服务器
      
  代理服务器工作流程
  	在前端服务器获取本地资源时(在public目录下获取)，如果public中没有匹配对应的资源，则会去找代理服务器帮忙转接查询请求资源(代理服务器须在vue.config.js中配置)，如果匹配成功返回数据。直接转接的代理服务器只能配置一个，这样不利于向多服务器发起请求，所以我们要给每个代理匹配一个前缀路径，当前端服务器使用该前缀查找资源时，对应代理才会匹配，否则不会进行资源查找。在代理服务器匹配成功后，需重写匹配前缀为空，否则转发的路径和目标服务器路径不匹配(匹配前缀只是用来匹配代理的，所以必须处理)
  */
  ```

  

assets文件夹用于公共存放样式，public文件夹也可存放一些用到的第三方库样式

## 1.9 vuex

nanoid可以生成唯一的id

vuex专门在Vue中实现集中式状态(数据) 管理的一个Vue插件，对vue应用中多个组建的共享状态进行集中式管理，适用任意组件间通讯



 Vuex工作原理图

![image-20210917151233885](C:\Users\xx\AppData\Roaming\Typora\typora-user-images\image-20210917151233885.png)

、图中需要一个store去管理 Actions,Mutations,state

vue在执行代码时，会先去执行引入(import )模块，然后在执行自己编写的代码，不管顺序如何

+ Vuex使用

  ```js
  /*
  1.
   下载 Vuex   npm i vuex
   引入且使用 Vuex Vue.use(Vuex)
   在 main.js中使用可以设置配置项store，并能成功绑定到vue实例上(不使用Vuex则store绑定不上)
   
   2. 在src文件夹下创建store文件夹，在其下创建index.js
   在index.js中写入    
   	import Vuex from 'vuex'
   	const actions = {} //准备action用于影响组件中的动作
   	const mutations ={} // 准备mutations用于操作数据(state)
   	const state={} //装备state用于存储数据
   	export default  new Vuex.Store({  //创建管理actions,mutations,state并向外暴露
   		actions,mutations,state
   	})
   3. 把配置完的store交给vue管理
     new Vue({   // 默认用到的组件或库已全部引入
     	el:'#app',
     	render: h=>h(App),
     	store
     })
     
   注意：
   	使用插件必须在定义之前执行，但由于vue引入模块优先执行，所以Vue.use('Vuex')应该放到store下的index.js中执行
  */
  ```

+ $store使用

  ```js
  /*
   1.$store下有dispatch,和commit方法
   	dispatch作用：联系actions做程序逻辑处理。然后commit转发给mutations
   	commit作用：联系mutations修改$store.state状态
   	
   2. actions组成(dispatch里的匹配值建议小写，函数同样)
   	actions对象里的函数使用$store.dispatch('匹配值'，参数) 才能触发,dispatch里的匹配值用于匹配actions里写的函数 ，参数用于传递给Value(第二个形参),actions函数有两个环境形参，第一个为context(包含dispatch,commit,state等)为一个浓缩版的$store，第二个值为一个value形参用于接收传递的数据，在这个函数中可以做一些修饰操作(如添加定时器，做逻辑判断等)，然后使用commit方法提交给mutations
   	
   3. mutations组成(commit里的匹配值建议大写,函数同样)
   	mutations对象里的函数使用$store.commit('匹配值'，参数)才能触发,commit里的匹配值用于匹配mutations里写的函数 ，actions函数有两个环境形参，第一个为state为$store.state下所有的状态(data)，第二个值为一个value形参用于接收传递的数据，在这个函数中可以做一些state的改变(state改变后会重新渲染页面)
   	
   4. state
   	state对象用于存储公共的状态，以键值对方式
   	
  注意：
  	如果没有使用定时器判断等加工代码可直接使用$store.commit('匹配值',value) 把数据传递给mutation处理
  	
  5. getters配置项
  	用来加工state里的数据并返回一个新的数据，使用$store.getters.value 获取加工后的数据 
  	如 const getters ={
  		bigSum(state){ // getters下的第一个参数bigSum,使用$store.getters.bigSum获取该函数加工值
  			return state.sum*10  
  		}
  	}
  */
  ```



+ mapState&mapGetters

  ```js
  /*
   mapState是vuex下的一个模块， import {mapState} from 'vuex' 引入
  mapState可以快速获取vuex中state里的数据
  1. 对象写法
   //mapState({he:'sum',xue:'study'})结果是有对象包裹的键值对函数需要使用...扩展，he为函数名(key)，sum为state,拿到的结果是以return this.$store.xxx形式返回，在使用时直接插入这个函数名{{he}}即可
  	computed:{...mapState({he:'sum',xue:'study'})} // key为computed下的函数名，value为查找的状态
  	
  2. 数组写法
  // mapState数组写法是把函数名和查询值都写成一样的，在读取时以{{sum}}读取，查找状态时以'sum'查找
  	computed:{mapState(['sum','study'])}
  
  mapGetters
   引入   import {mapGetters} from 'vuex'
   mapGetters和mapState用法一致，有对象和数组写法，用来读取getters里的数据
  
  注意
  	mapState函数返回的是一个对象形式因此需要使用...运算符扩展
  	使用mapState生成的computed属性在devTools中以binding形式显示
  	使用对象写法时{key:value} key表示实际的函数，value表示在state中查找的值，必须是一个字符串
  	使用数组写法时 ['value']，则同时指定了函数名和查找名，相对简洁
  */
  ```

  



+ mapMutations

  ```js
  /*
  1. mapMutations
   mapMutations可以快速调用commit方法把结构提交到mutations中
   
   对象写法（推荐）
   // key为函数名，value为查找mutations里的函数名
   格式 methods:{ ...mapMutations({increase:'ADD',decrease:'MINUS'})}  
   
   数组写法(不推荐)
    // 函数名和查找的数据一致
   格式 methods:{...mapMutations({'ADD','MINUS'})} 
   
  
   	
  2. mapActions
  	借助mapActions生成对应的方法，方法中会调用dispatch联系actions
   对象写法
   // key为函数名，value为查找actions里的函数名
   格式 methods:{ ...mapMutations({increase:'add',decrease:'minus'})}  
   
  数组写法
    // 函数名和查找的数据一致
   格式 methods:{...mapMutations({'add','minus'})} 
   
    注意
   	使用mapActions和mapMutations时，每个函数默认要接收一个形参，如果没有传递则默认去event环境变量，所以通常都是在声明时传入形参
  */
  ```

  

+ vuex模块化编码

  ```js
  /*
   1. 在做业务时，vuex收集的state一定会有很多，这时就需要把他们进行分类，哪个逻辑用到了哪个actions和mutations,state都要进行分类
   如   const cat = {namespaced:true, actions:{},mutations:{},state:{}}
   	 const dog = {namespaced:true,actions:{},mutations:{},state:{}) 
   	 此时各自分类有了各自功能，然后集成到store中
  如	new vuex.Store({
  	modules:{  //模块会被放到state下 $store.state.cat.xx
  		cat,dog
  	}
  当使用时，可以以 computed:{...mapState({cat:'cat',dog:'dog'})} 引入，然后使用 cat.xx 方法拿到里面的state .也可以给每个模块块指定 namespaced:true(默认false),这样就可以使用...mapState('模块名',{xxx:'xxx'})的方式获得模块里的内部数据 。 mapActions，mapMutations同理
  
  模块下的getters属性会存放到$store.getters下，以 '模块名/getters函数名'方式存储，以$store.getters['模块名/getters函数名'] 获取
  
  注意 
  	在使用模块区分功能且给每个模块配置了namespaced：true后,mapState等方法的第一个参数可以接收指定模块名，然后第二个参数可以拿到指定模块名下的下一级state数据
  	在模块化分类后，actions和mutations里的state和context拿到的当前模块的state
  	使用模块分类后，$store下commit，dispatch提交格式应为 this.$store.dispatch('模块名/actions函数名',value) ，commit同理  
  	使用模块分类后，获取某个模块下的state可以以 this.$store.state.'模块名'.value
  	每个模块可以单独开一个js文件，然后整合到store下
  	在使用模块分类后。每有一级模块包裹，$store下就会出现/进行分割
  	在模块内部使用commit,state等会在自身作用域查找
  */
  ```

  

## 2.0 vue-router

import xx from './router' 如果至指定了文件夹，没有指定文件，默认找当前目录下的index.js

+ 判断路由包裹情况

  二级路由依附一级路由，三级路由依附二级路由....(这种衣服关系被称为嵌套路由，且嵌套路由在页面中仍然呈现，如果没有呈现，则该路由为一级路由)

+ name属性

  ```js
  /*
   1. 各组件或标签name属性作用
   	组件的name属性，用于在<keep-active include="组件name"> 标签下的include进行缓存选定
   	路由中的name属性，可以给当前包含路由添加一个name属性(route可以查看)，查找时可使用 :to="{name:'val'}"进行快速选择路由，这和path:'/a/b'选择路由方式达到一样效果，不过以name指定可以更简介明了，route下还可以多一个name属性。路由查找规则：查找到指定的path或name属性后，返回对应的route下的component组件到<router-view>里
   	<transition/>标签里的name属性 更具不同name所对应的值可以指定不同个动画样式，如果没有name属性则默认动画样式只为 v-enter-active v-leaver-active
   	<slot>标签里的name属性，用来匹配父传子的标签 ，不使用name属性只能拿到没有被slot属性管理的标签元素(父组件slot管理示例：<div slot="manage">我</div>)，子组件使用<slot name="manage"><slot> 则可以收集父组件中被 <div slot="manage"></div>管理的标签元素(包括被包裹的标签元素)并呈现到子组件上
  */
  ```

  

+ SPA

  ````js
  /*
   单页面web应用(Single page web application)
   整个应用只有一个完整的页面，点击页面导航链接不会刷新页面，只会做页面局部更新(数据需要通过ajax请求获取)
  */ 
  ````

   

+ vue路由基本操作

  ```js
  /*
  1.基本操作
   下载vue路由 npm i vue-router
   Vue.use(VueRouter) // 使用后会多出很多内置标签
   把Router配置到vue实例   
   new vue({ //默认所有模块均已正确引入
   	el:'#app',
   	render:h=>h(App),
   	router:router,  //  引入路由配置
   	store:store   // 引入全局状态
   	
   })
   
   配置路由文件
   	创建一个文件加router。在其下创建一个index ，在其中 引入 VueRouter 和用到的组件 
   	配置路由：
   		const router = new VueRouter({
   			routes:[  //添加路由规则
   				{
   					path:'/about',
   					component:About
   				},
   				{
   					path:'/home',
   					component:home
   				}
   			]
   		})
   
   内置标签
       <router-link active-class="选中样式" to="路由页"></router-link> // to=""指跳转的路由 active-class="" 指被选中时触发的样式
       <router-view></router-view>  // 被选中route里的内容展示区域
  
  工作过程 	
  	下载并引入VueRouter后，在new vue中可配置router到全局，所有组件都有routet-lin和router-view标签。配置router文件(默认引入)  router new VueRouter({}),把所有的路径文件夹引入 以 {component:AB，path:'/ho'} 方式，router-link的to可以默认跳转到路径，这时router会在全局进行监听，当发生跳转时，router立刻去匹配它实例里的路径，匹配成功则启用对应的component，component会被渲染到 <router-view/>标签中
  	
  注意
  	组件分一般组件，和路由组件，一般组件为自己引入的组件，路由组件为经过router操作引入的组件。components中放一般组件，pages中放路由组件
  	切换路由时，原显示路由会被销毁
  	路由配置完后vue实例下会多出$route和$router,$route每个组件有各自拥有，存放的是当前路由的信息。$router为共同的存放的是history,match,option(里面是所有路由)等属性或方法
  	$route里存放的是query,params,路径等信息
  */
  ```
  
+ 多级路由

  ```js
  /*
   配置路由：
   		const router = new VueRouter({
   			routes:[  //添加路由规则
   				{
   					path:'/about',
   					component:About,
   					children:[
   						{
   							path:'news',
   							components:News,
   						},
   						{
   							path:'Message',
   							components:Message
   						}
   					]
   				},
   				{
   					path:'/home',
   					component:home
   				}
   			]
   		})
   注意
   	多级路由写在其上级路由的里面以children形式包裹，且多级路由的path不用加/，vue在遍历多级路由时，遍历到childre属性会自动加上/
   	router-link匹配时 to需写成 to="about/news"
   	router-link 和router-view同步出现 ，匹配到路由后在改组件内的router-view中展示
   	多级路由其子路由被选中，其上级路由默认也是被选中状态
   	
  工作原理
  	点击router-link，跳转至to指定的后缀域，路由带着这个后缀域去路由中和path进行匹配,匹配成功返回component指定的组件，然后该组件中使用router-view展示返回的组件内容
  */
  ```

  

+ 路由传参

  ```js
  /*
   1. query传参 使用 localhost:4000/#/about/news?id=666title=你好啊!  传参，?为分割先 传入的数据会以对象的形式方法$router的query下(以对象形式)
   
   2. to对象写法
   	<router-link :to="{
   		path:'/home/messgae',  // url路由显示 + 匹配路由
   		query:{  //传入的query参数
   			id: 1,
   			title: 'title'
   		}
   	}"></router-link>
   
   3.params
   	在配置路由时写成 /home/news/details/:id/:msg，则vue路由只会匹配到/home/news/details,其后的两个路由的值将会作为value放到id和msg下 如 params:{id:xxx,msg:xxx}
   	params使用实例
   		<router-link :to="{
   			name:'xiangQing',
   			params:{  //params中的key需和路由规则中的key一致
   				id:12,  
   				title: 'title'
   			}
   		}">
   		</router-link>
   	路由规则中写法
   		children:[
   			{
   				name:'xiangQing',
   				path:'details/:id/:title'.
   				components:Detail
   			}
   		]
   		
   4. 路由props传参
   	路由中的props可以给当前包含块中的组件进行传参，该组件可以使用 props:['val']方式接收
   	路由传参的方式
   		对象传写法路由组件中)：props:{a:1,b:'hello'} //组件接收使用a和b时显示为1和hello,使用props:['a','b']接收
   		布尔值写法：props:true值为布尔值，如果为真，就把该路由收到的params参数以props对象形式传递给该组件，如接收到的为id:1，title:'t',组件接收就写为props:['id','title'],当使用id和title时显示的是1和t
   		函数写法：返回值是一个对象，该函数有一个环境形参$route在该形参中可以拿到该路由的所有配置项，可以通过他来返回params和query参数.如 props(route){return{id:route.query.id,title:route.query.title}}
   		
   注意 	
   	使用params时 to只能用name方式跳转，不能使用path
   	当路由props传递params参数时，使用布尔值传递则query参数无法传递传递
  */



+ 命名路由

  ```js
  /*
   可以给路由起名字，使用 <router-link :to={name:'guanyu'}></router-link> 可以跳转至对应的name相同的路由，url路径变为 给name中的path路径
   
   注意
   	<router-link to="xxx"></router-link>  to="xx"方式是默认匹配一个路径(path)， to有一个配置对象 写成:to="{path:'xxx',name:'xxx',query:{},params:{}}" path为匹配的路径，name为指定的路由，query为携带的query参数
   	当想利用name进行跳转对应路由时，只能写成:to='{name:'xx'}'形式
  */
  ```

  

+ replace属性

  ```js
  /*
   replace为route-link标签下的一个属性用来替换掉当前栈顶的一条信息，不写replace默认为push，即每点击一次路由跳转就会向栈下压一层路由，同时指针上移指向最新一层，如果我下次点击的路由是replace属性则替换掉当前栈顶的路由
   <router-link replace></router-link>或<router-link :replace="true"></router-link>
  */
  ```



+ 编程式路由导航

  ```js
  /*
   利用$router原型下的back()和forward()方法来完成页面前进和后退
   利用$router原型下的push()和replace()方法完成推栈和替换栈
   $router.push({name:'xxx'(或path:'/a/v')}) 完成推栈和页面前进
    $router.replace({name:'xxx'(或path:'/a/v')}) 完成替换栈顶和页面前进
    $router.back() 方法完成页面一次回退(指针往下移一格)
    $router.forward() 方法完成页面一次前进(指针往上前进一格)
    $router.go(number) 需传入一个数字，如果是正数则完成number次页面前进，如果是负数则完成number次页面后退
    
    注意
    	每次点击前端路由实际都是向栈顶压一层路由页面(默认push)，当执行到含有replace属性的路由跳转时，则替换(覆盖)掉当前栈顶的一层路由页面  如 往下推 1 2 3 .推的第4下是replace属性的路由则替换掉3 栈结构为 1 2 4，当点击回退时回退到2
  */
  ```

  

+ 缓存路由组件

  ```js
  /*
   使用 <keep-active/> 标签包裹的<router-view/>标签返回组件在做切换时不会被销毁，而是把数据缓存到vue下，<keep-active> 有一个属性 include可以选定要缓存的路由组件名，此时只有被指定需要缓存的组件才会被缓存，通常用在保存临时数据等场合。单纯的显示内容和功能不需要<keep-active>包裹
   	如 <keep-active include="News"> // 此时只有News组件在做切换时不会被销毁
   		<router-view></router-view>
   		</keep-active>
  
  注意
   	<keep-active/>中include属性需要填写要缓存组件的组件名(组件名需要在该组件export default{}内以name形式指定)
   	哪个组件的数据需要缓存就给它的上层引用组件的<router-view>包裹<keep-active>
   	如果想要缓存多个组件 需写为 :include="['News','Message']"
   	要对多级路由进行缓存则要把它上级路由都要设置缓存，否则就会产生缓存链中断
  */
  ```

  

+ 路由组件独享声明钩子

  ```js
  /*
   有 actived(){} 其他路由切换到当前组件时触发  ，通常开启定时器，和数据请求
   deactived(){} 切换路由到其他路由触发 ， 通常关闭定时器和中断数据请求
   应用：当想做数据缓存又想在页面切换时中断请求和定时器，可以使用这些钩子
  */
  ```

  

+ 路由守卫

  ```js
  /*
   作用：只有符合对应的要求才能查看对应的前端路由
   1. 前置路由守卫
   VueRouter实例下有一个方法beforeEach((to,from,next)=>{}) 在初始化和每次切换路由时自动调用，形参to是要去的目标路由，from是从那里进入的那个路由,next为放行函数，如果不放行不会跳转路由 。
   应用：可以对页面切换做判断，有权限则可以查看，没权限则不能看
   
   2、 后置路由守卫
   VueRouter实例下有一个方法afterEach((to,from)=>{}) ,形参to是要去的目标路由，from是从那里进入的那个路由
   应用： 通常用来动态修改文件标题显示
   
   3. 独享路由守卫
   在需要配置独享路由守卫的路由包含块下，配置beforeEnter(to,from,next){}函数，和全局守卫逻辑一致，不过只是对单个路由进行限制 。都西昂路由守卫只有前置没有后置
   
   4. 组件内路由守卫
   在路由组件中进行配置beforeRouterEnter(to,from,next){}和beforeRouteLeave(to,from,next)
    beforeRouteEnter在通过路由规则进入该组件时被调用，beforeRouteLeave在通过路由规则离开该组件时被调用(即从同级路由跳转到零一个同级路由时调用)
   
   注意
   	to和from是一个route对象可以通过路径判断和变量设置来判断是否可以放行
   	设置路由时，只能加入指定的key value属性，如name,path,query等，自定义配置需要放到route下的meta里，如 routes:[{meta:{isAuthority: true},path:'/a'}]
  */
  ```
  
  

+ history模式与hash模式

  ```js
  /*
  1. hash模式
   路由地址里的 localhost:8080/#/ 这个#表示hash,hash值后的路径不会随着http请求发给服务器 如    localhost:8080/students/#/wsdaw/wdaw  发给后端的请求只有localhost:8080/students不包括#后的路由域
   
   2. history模式
   在路由实例中通过 mode:'history'来更改路由模式 ，如new VueRouter({mode:'history'||mode:'hash'})，向服务器发起请求时全部路径都会发给服务器
   
   3. hash模式和history对比
       hash比history兼容性较好 
       在部署到服务器上后，可以使用hash模式(方便)，因为hash模式在页面刷新时不会带着前端路由路径去找服务器请求资源，而history模式会，如果后端没有设置匹配的资源则会报错 。如果一定要使用history模式，那必须由后端人员进行配置解决。
  如在node中可以使用connect-history库，npm i connect-history-api-fallback,然后在使用静态资源之前使用该中间件 
  const express = require('express')
  const history = require('connect-history-api-fallback')
  const app = express()
  app.use(history())
  app.use(express.static(__dirname+'/static))
  */
  ```

  

## 2.1 Vue UI 

+ 移动端宠用ui库

  Vant 

  Cube UI

  Mint UI

+ PC端常用UI库

  Element UI

  IView UI

UI库使用参考官网即可

+ Element UI

  ```js
  /*
  1. 粗暴使用 (产生文件体积过大，包含全部css)
   npm i element-ui 下载ui库
  import ElementUI from 'element-ui'  // 引入elementUI库
  import 'element-ui.lib.theme-chalk/index.css'  // 引入css样式
  Vue.use(ElementUI)  // 即可直接使用elementUI里的全部样式
  
  2. 局部(按需)使用(用到那个部分引入那个部分，包含引用到的css)
   	npm install babel-plugin-component -D   
   	然后，将 .babelrc 修改为：
      {
        "presets": [["@babel/preset-env", { "modules": false }]],
        "plugins": [
          [
            "component",
            {
              "libraryName": "element-ui",
              "styleLibraryName": "theme-chalk"
            }
          ]
        ]
      }
   	按需引入配置完成后 可以以 import {Button,Row} from 'element-ui' 等引入组件(相关样式会自动配置)，然后使用 Vue.component('name-xxx',Button)注册组件到全局(component第一个参数是给该组件指定一个标签名，在其余组件中使用该标签名即可引入该Button组件)
  */
  ```




## 2.2 vue源码分析

源码分析不能光看，需要自己去理解感悟

+ 数据代理

  ```js
  /*
   好处：没有数据代理只能使用 vm._data.xx方式获取data里的值，使用数据代理后只需要使用vm.xx即可获取data里的值
   实现：
   	通过Object.defineProperty(vm,key,{})给vm添加与data对象属性对应的属性
   	所有添加的属性都有get/set方法
   	在get/set方法中去操作data中的属性
  */
  ```




+ 模板解析

  ```js
  /*
   模板解析的关键对象为 compile对象
   模板解析基本流程： 将el所有子节点取出，添加到一个新建的文档fragment对象里，然后对fragment中的所有层次子节点递归进行编译解析处理 ，一般解析对象为 插值语法，事件指令，一般指令
   	插值语法
   		首先根据正则匹配{{}}里的值，然后找到在data或computed中的值进行对应匹配，再将属性值以文本节点textContent方式渲染到节点里
   	事件指令解析
   		首先从指令名中取出事件名，根据指令的值从methods中匹配对应的事件处理函数，在给当前元素节点绑定事件名及回调函数的dom监听事件，最后，指令解析完毕后，移除此指令属性
   	一般指令解析
   		得到指令表达式和值，从data中根据表达式得到对应的值，然后以指令方式渲染到节点，最后移除元素指令属性(渲染指令有 v-text v-html v-class)
  */
  ```



+ 数据劫持&数据绑定

  ```js
  /*
   数据绑定
   	一旦更新了data中的某个属性的数据，界面上的属性也会一起更新
   数据劫持
   	数据劫持是vue中实现数据绑定的一种技术，通过defineProperty()来监视data中所有属性数据的变化，一旦变化就去更新界面
   	
   四个重要对象
   	Observer
   		用来=对data所有属性数据进行劫持的构造函数
   		给data中所有属性重新定义属性描述(get,set)
   		为data中每个属性创建对应的dep对象
   	Dep(Depend)
   		data中的每个属性都对应一个dep对象
   		创建时机：
   			在初始化define data中各个属性时创建对应的dep对象
   			在data中某个属性值被设置为新的对象时
   		对象结构
   			{
   				id, // 每个dep都有一个唯一id
   				subs  // 包含1n个队医watcher数据(subscribes)
   			}
   		subs属性说明
   			当一个watcher被创建时，内部会将当前watcher对象添加到队医的dep对象的subs中，当data属性值发生改变时，所有subs中的watcher都会被收到更新的通知，从而最终更新队医的界面
   	compile
   		用来解析模板页面对象的搞糟函数，利用compile对象解析模板页面，没解析一个表达式都会创建一个对应的watcher，并建立watcher和dep关系
   		compile与watcher是一对多关系
   	watcher
   		在初始化编译模板时创建，模板中每个非事件指令或表达式都对应一个watcher对象，用来监视当前表达式数据的变化
   		对象的组成
   			{
   				vm, //vm对象
   				exp, // 对应指令表达式
   				cb, // 当表达式所对应的数据发生改变的回调函数
   				value, // 表达式值
   				depIds, //表达式中各级属性所对应的dep对象集合
   			}
   	总结
   		dep和watcher是多对多关系，一个data中的属性对应一个dep，一个dep中可能包含多个watcher
   		模板中一个非事件表达式对应一个watcher,一个watcher可能博阿寒多个dep
   		数据绑定使用2个核心技术 ，defineProperty(),订阅与发布
  */

![image-20211007110312597](C:\Users\xx\AppData\Roaming\Typora\typora-user-images\image-20211007110312597.png)



+ 双向数据绑定

  ```js
  /*
   双向数据绑定是建立在单向数据绑定的基础上的，在解析v-model指令时，给当前元素添加input监听，当input的value发生改变时，将最新的值赋值给当前表达式所对应的data属性
  */
  ```





## 2.3 项目实战

## 看到10 2.06

工程化项目：可以一键打包运行的项目

css预编译器:less,sass,stylus

![image-20211007125151935](C:\Users\xx\AppData\Roaming\Typora\typora-user-images\image-20211007125151935.png)

![image-20211008103227689](C:\Users\xx\AppData\Roaming\Typora\typora-user-images\image-20211008103227689.png)





+ vue常用三方库

  vue-router  开发单页应用

  axios/vue-resource  向后端发起请求

  vuex 集成状态管理

  better-scroll/vue-scroller/swiper 实现页面滑动(轮播)

  mint-ui 组件库

  vue-lazyload 实现图片懒加载

  mockjs 模拟后台数据接口





+ 移动端适配

  ```js
  /*
   用到两个包  lib-flexible postcss-px2rem，这样我们就能按照图里的像素以px方式编写，不需要自己转换
   	下载  npm i lib-flexible postcss-px2rem -D    
   	适配配置
   	
   	
   	
   	
   	
   注意
  	一个package.json文件中的main：xx指定该文件的入口js
  */
  ```



+ 解决0.3秒延迟

  ```js
  /*
   使用fastclick库即可，并在script中输入
  <script type='application/javascript' src='/path/to/fastclick.js'></script>
  if ('addEventListener' in document) {
  	document.addEventListener('DOMContentLoaded', function() {
  		FastClick.attach(document.body);
  	}, false);
  }
  */
  ```



+ 样式书写小技巧

  如果样式有问题可以在页面中以删除标签元素的方式进行调试，然后切换大小屏幕，即可完成屏幕适应

  点击下方的Filter可以看到css中所有默认样式



+ stylus语法

  ```js
  /*
   stylus是css编译语言，编译后的代码最终会转变为普通css输出
   最新版本的stylus和stylus-loader有兼容性问题，建议下载指定版本
   	npm i stylus@0.54.5 stylus-loader@3.0.2 -D
   
  1. 基本语法
  	(1) 省略了选择器后的{} 和 样式后的: 以及结尾的; , 央视嵌套，用缩进表示包裹层级
  		如 普通写法 a {            	stylus写法    a
  					width: 100px;   			  width 100px                    
  				}
  	(2) 定义变量 
  		用来存储当前页面的主题色 ， 如 $color = '#ace'
  		
  	(3) 定义混合
  		如 btnSty(w,h,bg)其他样式直接引入该混合即可使用该样式如.btn1
  			width w                                          btnSty(10px,20px,red)
  			height h
  			background bg
  			
  	(4) 函数
  		在函数中可以对数值进行计算，而混合指向单纯的样式复用
  	
  	(5) 父级引用
  		使用 & 表达当前标签元素的上一级父级
  		
  	(6) 导入文件
  		使用 @import "x"
  */
  ```
  
  

# Vue(3.x)

+ 创建vue3脚手架

  ```js
  /*
  第一种方式：vue create xxx ， 选择vue3
  
  第二种方式：使用vite创建 vite在开发环境中无需打包即可启动，轻量快速，按需编译
  	创建工程： npm init vite-app <project-name>
  	进入工程目录：cd <project-name>
  	安装依赖：npm install
  	运行：npm run dev
  */
  ```

  

  + vue3对比vue2脚手架的变化

    ```js
    /*
     import {createApp} from 'vue' // 引入的不在是Vue构造函数，而是一个工厂函数
     import App from './App.vue'
     createApp(App).mount('#app')
    */

  

  

  
