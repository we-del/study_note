#   React

+ 特别注意

  ```java
  /*
   1. 在js中要注意this指向问题，this一般指向其调用者，但js中箭头函数this指向和外层this指向一致，函数的返回值无异常(返回给调用者)， 因此在特殊使用我们可以使用call,apply,bind 去改变函数this的指向
   2. 在引入资源时，该资源会被先执行一遍(已经引入的资源在引入时不会在执行，因为其已经加载过)
   3. 在js事件调用时，如果需要接收参数来完成功能，就需要使用静态代理模式来解决问题(因为js底层在调用时，接收的需要是一个函数地址，且只会传入一个环境变量参数，因此需要使用静态代理模式帮助完成参数传递)
   	 如：a.onclick = (e)=>{ fun(uuid,e)} // 当点击事件触发后，js底层调用此箭头函数(代理函数)，然后由箭头函数(代理函数)去调用另一个真正需要执行的函数(实现函数传递) 
   4. react在调用了一个函数没有传递正确的参数时，会阻塞程序运行，且不会提示错误
  */
  ```
  
  

报错的通常原因，在一个undefined中读取属性，变量未定义，语法错误，没打括号等

## 技巧

+ 解决rfeact报错问题

  ```java
  /*
   18.0 后 react不支持 ReactDOM.render()方法需要使用下列方法
   import React from 'react';
  import ReactDOM from 'react-dom';
  import App from './App';
   
  import { createRoot } from 'react-dom/client';
  const container = document.getElementById('root');
  const root = createRoot(container);
  root.render(<App />);
   
   
  // 被注释的是之前ReactDOM.render的代码
  // ReactDOM.render(
  //   <React.StrictMode>
  //     <App />
  //   </React.StrictMode>,
  //   document.getElementById('root')
  // );
   
  */
  ```

  

+ date

  **说明： 技巧，前言部分为 2022/4/2日添加，旨在认真复习react和vue，之前做的笔记仅供参考，此后新增笔记才是本体**

+ 解决jsx文件无法快速生成html标签

  ```java
  /*
   在WebStorm中打开File–>Setting–>Languages & Frameworks–>JavaScript–>Libraries，点击右边的Download按钮，选择TypeScript community stubs，下载react和react-native。
  */
  ```

  

+ 组件实例对象创建到页面渲染过程

  ```java
  /*
  注意
  	js的类也存在 封装，继承和多态
  
  
  实例对象创建过程
   		js底层会去调用该组件类的构造器 , 然后再其中隐式调用super()关键字去调用其父类的构造器(如果父类中使用了constructor就需要自己手动调用super()),一直往复如此，直到加载到顶级父类，然后从顶级父类类到一级父类,...,到当前子类，执行static修饰的语句，并放入一个地址空间中，该空间和子类存在映射关系 ，此时类加载完毕，然后从从顶级父类类到一级父类,...,到当前子类，执行属性，方法和构造器，执行完后将其属性和方法压入(堆结构)该实例对象的地址内，往复如此直到把当前子类压入该地址内时，此次实例对象就创建成功了，该地址空间内，从最底部到最顶部依次为 顶级父类的属性和方法 ，一级父类的属性和方法,...,当前父类的属性和方法。当该实例对象使用属性和方法时优先从顶部开始查找，如未找到，则查找指针下移去查找此上一级父类的属性和方法，直到找到为止。此外，每次方法的调用会从当前子类实例对象开始查找，也就是该空间的最顶层开始查找。
  
  当编写完组件类后，引入该组件实际上是创建了一个该组件类的实例对象，然后react底层会进行一次实例对象的参数补充，它会将props接收的属性，载入到该实例对象的props对象下，将ref属性映射的虚拟dom或组件 载入到指定属性上，同时进行一次页面渲染(初始化，和更新时渲染)，此时组件实例对象一次工作完毕
  
   react 生命周期函数得调用(间接render()重绘页面)方式常见有:
   	初始化时
   	componentDidMount()生命周期函数调用导致其他函数调用setState()时
   	事件触发，导致其他函数调用setState()时
  */
  ```
  
  



+ 集合

  ```java
  /*
  ==================================================
   // 关于组件
   1. 在组件拆分时，把可能在多个页面用到的部分拆分成一个组件(js的模块)，以便在多个页面进行引入，大大增加了其可维护性。如果一个功能在其他页面不可能被引用则不需要拆分
   2. 每引用一个组件就是创建了该组件类的实例对象，然后执行初始化生命周期函数
   3. 组件分受控组件和非受控组件,使用state来控制重绘的组件就是受控组件，没有使用state的就是非受控组件
   4. react是组件化开发的，也就是说react构建的页面也是基于对象来开发的，通过某个组件实例对象的state改变来完成页面局部更新
   5. 一个组件在其他组件被引用时，被引用组件被称为子组件，主动引用其他组件的组件被称为父组件
   6. 如果一个组件的内容过于庞大，可以对该组件的局部功能进行拆分(变为该组件的一个子组件)，然后再引入到该组件中计科(增加了可维护性，和复用性)
   ==================================================
   // 关于react diff算法
   1. react使用虚拟diff算法来增加重绘重排效率，且该算法通过每个属性不同的key来完成识别
   
    ==================================================
   // 类
   1. js 在生成了一个类的实例对象后，通过对类方法进行调用或手动在该实例对象下挂载属性，仍然可以在该实例对象地址中追加属性(java不行)
    ==================================================
   // react脚手架
   1. react脚手架是facebook公司搭建的一个webpack脚手架，可以高效的运行react环境，react脚手架具有模块化，组件化，工程化特点
   2. 脚手架整体的架构为：react+webpack+es6+eslint 
   ==================================================
   // 其他
   1. 因为js中函数也可以当作参数传递,因此为了保持该函数的this指向不变,可以使用 apply,call或bind 来完成调用
   2. import引入其他资源，和java中引入其他类相似，都是把目标的一个映射引入到本文件中
   3. React.Component 在 react 包中 ，ReactDOM.render() 在 react-dom 包中
   4. props 属性可实现 父类向子类通信，ref属性可实现 子类向父类通信
   5. 在一个组件实例对象生成完毕后，其地址内底层已经隐式挂载了react底层的生命周期函数，当它们调用时，是从该地址顶层开始查找，(因此我们可以重写这些生命周期函数，来完成生命周期函数的过程封装),如果没找到则 寻址指针下移 最终会移动到该地址的底层去调用react底层封装的生命周期函数，找到后调用，寻址指针重新回到该地址顶部，准备下一次属性或方法调用
   6. react在事件调用时如果想携带参数，可以写为 onClick={(e) => this.removeUserContent(uuid, e)} ,该语句表示，当这个按钮点击时，传入一个函数到react底层，然后react调用传入了一个环境变量，此时通过此调用来完成另一个方法调用
   7. 函数也可以返回虚拟dom
  */
  ```

+ 注意

  ```java
  /*
  ==================================================
  // 关于虚拟dom
   1. 虚拟dom存储在内存中
   2. jsx中 分 js语句和虚拟dom标签,js语句在虚拟dom标签外可以正常解析,在虚拟dom标签内使用需要加{}，且{}内只能写js表达式(或虚拟dom标签，如果该虚拟标签内还需要解析js表达式则要嵌套{})，(有值在本行得语句就是js表达式，包括变量，有返回值的函数等)
   3.虚拟dom标签和dom标签语法，结构和功能大致相同，虚拟dom也可以通过css来改变样式，标签包裹的内容默认当字符串处理，标签行内的内容默认当属性处理，唯一不同点是虚拟dom可以使用js表达式而真实dom不行，如果在虚拟dom标签中的某个位置需要使用js语句，则需要使用{}包裹(在被包裹的内容中只能写js表达式，在虚拟dom内的注释也需要使用{}包裹)
   4. 虚拟DOM绑定事件和原生绑定事件基本一致，不过虚拟DOM绑定事件采用小驼峰命名，且通常写在行内
   5.为了简化开发，使用js渲染的虚拟dom如果超过5行则可以单独开一个属性方法，然后由改属性方法会返回虚拟dom(属性指向一个函数，react无法自定方法，因为react底层干煸了方法的this指向)
  ==================================================
  // 关于组件
   1. 当引入一个组件时(如 <He/>)，react底层会帮助生成一个该组件类的一个实例对象，并进行生命周期函数初始化调用（最终会去调用render方法(返回虚拟dom)），我们只用写该组件标签即可
   2. 当我们创建了一个组件类后，react底层将我们自己定义的方法的调用者更改成了undefined(重写父类已有的方法时，调用者正常,即该组件实例对象),因此 我们需要将其修改指向并挂载到该实例对象的属性上，这样就能正常解析使用
   	如  this.xx = this.xx().bind(this)  //此代码因写在constructor 内
  ==================================================
  // 关于state
   1. setState只会改变当前组件实例对象的状态，因此只会对此组件的局部进行重绘(diffs,即单个组件状态改变，之后导致页面局部更新)
   2.使用 this.setState()方法能重新渲染页面，底层使用了观察者模式？(state)属性只能对单个表达式的指向产生修改 所以不能重新渲染页面
   3. 每次引用组件就是创建了该组件的实例对象，,react底层调用的就是该实例对象的 render方法，然后把虚拟dom作为返回值返回到该行
   4. 在 webstorm中 不要选择 Change setState() signature 选项 ，否则state属性无法使用
  
   ==================================================
  // 关于props
   1. 在引用一个组件(实例对象)时，可以在该组件标签中 添加 行内属性，此时这些行内属性会被收集并放到该组件类(实例对象)的props下(以对象形式)
   2. 在实际使用中，通常把props需要传入的数据 放入一个对象中，然后再传入时使用 {...obj} 即可完成展开，实现props属性传入，此方式简化了props传递(react+babel会转换 ...obj 帮助该对象展开(react+babel独有) )
  
   ==================================================
  // 关于ref
  1. ref属性可以获取当前组件类中虚拟dom的一个dom节点或使用的组件，并把该节点或节点维护到该组件类的一个属性下
  	将遗弃的写法  
  		<div ref="divNode">123</div> // 把该虚拟dom节点维护到 该组件类的ref属性对象下，key为divNode，value 为该虚拟dom节点
  	函数写法 
  		<div ref={node => this.divNode = node}>123</div> // 将该虚拟dom节点维护到组件类的divNode属性下 ； 此方式是向node底层传入一个函数，然后其进行调用并将该虚拟dom节点放到该函数的第一个形参上
  	专人专用写法(每一个ref实例只能存放一个虚拟dom节点)
  		myRef = React.createRef();  // 创建一个可以存储虚拟dom节点的一个属性
  		<div ref={this.myRef}>123</div> // 把该虚拟dom节点存放到myRef属性下
  
   ==================================================
  // 关于生命周期函数
  1. 生命周期函数为react底层定义的特有函数，它们由react统一来完成调用，我们可以通过对生命周期函数进行重写来完成自己封装生命周期钩子的过程(即react底层会做判断，在对应的生命周期函数调用中，如果我们重写了该生命周期函数则react会帮我们调用，如果某个生命周期钩子我们没有重写，react会依去底层调用其自己的钩子函数)
  2. 每个组件实例对象继承了这一套生命周期系统，组件间的生命周期过程(函数)互相隔离，即不会影响彼此(当生成A组件实例对象时，是调用A组件实例对象上的初始系列方法)
  3. 当初始化时，按顺序调用 componentWillMount() -> render() -> componentDidMount() 方法
    当更新状态等操作时，按顺序调用为
    componentWillReceiveProps() -> shouldComponentUpdate()->componentWillUpdate() -> render() -> 	   componentDidupdate()
  4. 每个组件类都隐式继承了这些生命周期函数
    
   ==================================================
  // 关于diff算法 (百度查看源码)
  	1. 创建虚拟DOM树是 使用了文档碎片技术
  	2. 每一个虚拟dom都会被react赋予一个唯一的不冲突的key值(不包括js表达式生成的虚拟dom标签，js生成的标签需要自定义key属性)，然后react通过该key值给每一个虚拟dom标签生成对应一个文档碎片，最后组成一颗虚拟dom树，并进行页面初始化渲染，当状态改变时，会去检查虚拟dom树结构和每一个文档碎片的前后数据，前后一致的文档碎片不做处理，前后不一致的文档碎片和虚拟dom树内容，则会进行内容替换或更新，并重新渲染页面，即局部更新
  	3. 在进行diff算法时，只会进行文档碎片的内容比较，而非标签比较，在替换时，也是替换标签内有差异的部分
  	
   ==================================================
  // 关于react脚手架
  	1. 使用 npm i -g create-react-app 全局创建脚手架(全局安装后，该命令在此主机上就可以使用)
  	2. 使用 create-react-app 项目名，会在该路径下创建一个react脚手架项目
  	3. 在react脚手架中，css和图片资源当作 "模块" 引入，且这些资源默认为 默认暴露
  	4. 在idnex.js页面渲染时,可以直接获取public下的index.html里的标签(React内部完成链接,文件夹不能更改)
  	5. 成型的第三方库或者包需要放在public目录下(如 bootstrap.css)，由index.html引入
  	
   ==================================================
  // 其他
  	1. render返回的虚拟dom最终会由react底层渲染到界面上(期间还会进行diff算法差异比较)
  	2. 再引入一个组件类时，如果没有指定组件名，则react会默认引入该组件下的index.jsx 文件
      3. 再引入一个组件类时，如果是jsx或js文件,则后缀可以不写
  */
  ```

+ 规范

  ```java
  /*
   ==================================================
  // 关于虚拟dom
   1. 为了使多级虚拟dom标签为一个整体，通常使用()进行包裹
   2. 在react中 虚拟dom标签的 class 属性 必须写为 className
   3. 虚拟dom标签写样式时，必须使用{{}}包裹，且样式必须写为键值对格式(对象形式)
   4. 函数也可以以返回值得方式返回虚拟dom，利用这个特性可以向类中render()方法的return中载入虚拟dom
   ==================================================
  // 关于组件
   1. 组件的状态改变只能在该组件中进行改变，如果其他组件想改变此组件的状态，可以向外暴露一个方法，其他组件只用调用该方法即可(迪米特法则)
   2. 组件必须大写字母开头，且文件名必须和组件名一致(借鉴java)
   
  ==================================================
  // 关于state
   1. state中的复杂数据类型的数据不要直接指向该地址并且修改该地址的数据,而应该先深拷贝(使用JSON下的stringify和parse方法)一个state地址里的数据，然后在对这个新地址进行改变，最后使用 setState()维护新数据 ， 如果不这样做可能导致状态改变但页面不更新(因为有些时候state里的值变化了但是由于地址没有改变可能导致页面不更新)
   2. 在一个类中需要使用的state最好在一开始统一声明(，后面向该对象下挂载属性也可以，但是对于属性来说不直观)
  ==================================================
  // 关于jsx
   1. jsx只允许有一个根标签
   2. jsx 就是 js+xml ，即使用js和xml的语法规范，所有的虚拟DOM标签必须闭合
   
  ==================================================
  // 关于react脚手架
   1. react脚手架中，js交互直接写在对应的模块类的方法中 ； 在引入一个资源时必须见名知意，在引入一个组件类时，必须和该类名同名
   2. react脚手架中,public目录是服务器的根目录
   3. 每有一个组件类就会随即给该组件类配置一个文件夹(一般为该组件类名的小写形式)，该文件夹下放入该组件类使用的资源(图片，样式等)
   4. 组件类一般使用jsx文件(即该文件专门用于存储组件类)，App.js,index.js一般为包装文件，即对这些组件进行总和并做处理
   5. 在react中，不要使用a标签 ,且使用window下的confirm方法时，需要写为 window.confirm()
  ==================================================
  // 其他
   1. 标签的key值需选择唯一不冲突的值(减少页面怪异行为)，此时在状态改变时有效的减少页面重绘重排，大大提高了渲染效率
   2. 因尽可能的少使用ref属性
   3. SPA ,为 single page Application
   4. react模式(结构)和java非常类似
   5. 生命周期函数setState()可以更新状态的同时，开启一轮更新生命周期函数的调用，因此state通常被认为是用于页面渲染的属性
   6. 关注点分离原则(软件工程中得一个理念)，即把状态和结果区分开，在返回状态时，就知道这次操作得结果
   7. 在react组件间通信中，能使用props和ref完成的通信则尽量使用它们(迪米特法则，不要将组件的数据过多的暴露给外部)，实在无法完成可以使用 消息订阅与发布 和 redux
  */
  ```
  



+ 生命周期钩子函数(旧)

  ```java
  /*
  注意
  	此为react生命周期函数调用顺序，react在每次调用生命周期函数时，会去检查该组件实例对象是否重写该生命周期函数，如果重写则进行调用。如果某个生命周期钩子我们没有重写，react会依据情况去底层调用其自己的钩子函数(往复如此，直到初始化或更新的函数顺序调用执行完毕)
  	componentDidMount() 生命周期函数 一般用于开启 定时器，发起请求，等初始化操作
  	componentUnmount()生命周期钩子在一个组件实例对象将要被从页面卸载时,进行调用,通常在该函数中做首尾工作
  	只有调用render()方法才会进行页面重绘
  	每个组件实例对象都有自己的生命周期过程(函数)，它们之间互相不影响
  	componentDidMount()和componentUnmount()是两个常用的生命周期函数
  	当我们使用this.setState()更新状态时，react底层会从 shouldComponentUpdate()开始进行调用(,此方法返回值如果为true表示按顺序调用下一个生命周期函数，为false表示终止更新)，我们可以对shouldComponentUPdate()进行重写来完成延时更新等功能(如延时更新等)
  	调用this.forceUpdate()方法也可以完成页面重绘，该方法接收一个函数作为形参，起强制更新作用
  	当父组件向子组件传递props参数时，componentWillReceiveProps() 生命周期函数开始顺序调用
  
  组件实例对象挂载过程
   	当一个组件实例对象被创建(先遵循js对象创建原则)后，react底层会顺序去调用该组件实例对象的 componentWillMount() -> render() -> componentDidMount() 方法 完成初始化显示 。
   	
  组件实例对象更新过程
   	当一个组件做了状态更改操作时，如果调用的是 setState() ,则react会去调用 shouldComponentUpdate() -> componentWillUpdate() -> render() 方法 ;如果调用的是 forceUpdate() 则 react会按顺序调用 componentWillUpdate() -> render()
  */
  ```
  
  ![image-20220406195601900](D:\typora_import_images\typora-user-images\image-20220406195601900.png)



+ 生命周期钩子函数(新)

  ```java
  /*
  新增钩子函数
   static getDerivedStateFromProps(props,state)   // 此钩子需要使用static关键字装饰
     此生命周期函数起一个拦截作用可以在函数中对state的值进行更改，根据此钩子的返回值react会做判断，如果为     null则不会对state发生更改，如果返回一个对象，则表示更新state里的数据，如果如果返回对象的key在state里存   在，说明做覆盖操作，如果不存在说明做添加操作
     应用： 可以获得父组件向该组件传递的属性，然后维护到该组件的状态中
     使用
     	static getDerivedStateFromProps(props, state) {
              console.log(props, state);
              return {contentList: [], data: 0};
          }
    
    getSnapshotBeforeUpdate(prop,state)
    	此生命周期函数需要和componentDidUPdate配合使用,该函数返回一个结果，并作为参数出现在componentDidUpdate第三个参数上，通常用于界面临时信息(使用较少)
    	
  组件实例对象初始化过程(新钩子函数)
   getDerivedStateFromProps(props,state) -> render() -> componentDidMount
   
  组件实例对象更新过程
   	getDerivedStateFromProps(props,state) -> shouldComponentUPdate()(使用force(fn)强制更新时不会触   发此方法) -> render() -> getSnapshotBeforeUpdate() -> componentDidUpdate()
   	
  新，旧生命周期函数对比
   	新生命周期函数中废弃了 componentWillMount(),componentWillReceiveProps,componentWillUpdate()钩子   函数，新增了 getDerivedStateFromProps()，getSnapshotBeforeUpdate()钩子函数
   	getSnapshotBeforeUpdate 介于 componentDidUpdate和render之间
       
  */
  ```

  ![image-20220407172339059](D:\typora_import_images\typora-user-images\image-20220407172339059.png)





+ 虚拟dom与diff算法

  ```java
  /*
   当初始化一个组件实例对象时(引入组件)，页面渲染顺序为：创建虚拟DOM树 -> 真实DOM树 -> 绘制界面显示
   当对组件实例对象的state属性进行更改时，页面渲染顺序为: setState()更新状态(导致页面重绘) ->重新创建虚拟DOM树 -> 新/旧 dom标签对比 (通过对key的对比)-> 更新差异位置的虚拟DOM树 -> 重绘界面
  */
  ```

  ![image-20220407202046650](D:\typora_import_images\typora-user-images\image-20220407202046650.png)



+ fetch

  ```java
  /*
   前言
   	fetch是 h5 新增得一个js发起异步请求的一个语法，它遵循了关注点分离原则,即把状态和结果区分开，当返回状态时就知道请求得结果
   	
   语法
   	 sendRequestByFetch = () => {
          let q = "v";
          let s = "stars";
          let url = `https://api.github.com/search/repositoriess?q=${q}&sort=${s}`;
          fetch(url) // 向url地址发送fetch请求(异步请求)，并返回一个 fulfill状态的promise实例(存储着请求的结果)
  
              // 得到该请求的结果，如果接收请求的地址不存在，则立刻抛出一个rejected状态的promise实例(其存储着错误信息)
              .then(response => {
                  console.log(response);
                  // 如果接收请求的地址存在则解析其结果并抛出一个fulfill状态的promise实例(存储着响应结果)
                  return response.json();
              })
              // 拿到该请求的结果。如果没有返回数据(未匹配到路由)则立刻抛出一个 rejected状态的promise实例(存储着错误信息)
              // 如果有返回数据，则继续执行该函数
              .then(data => {
                  console.log(data);
                  this.changeStateByFetch(data);
  
              })
              .catch((reason) => {
                  console.log(reason);
                  let {responded} = this.state;
                  responded = true;
                  this.setState({error: true, responded})
              });
      }
   
   注意
   	fetch向指定位置发送请求后，会返回一个 fulfill状态的promise实例对象,其中的value为此次请求的状态;当我们使用then方法拿到该结果后,如果地址匹配失败则立刻抛出一个状态为rejected的promise实例(值为报错信息),如果匹配地址成功需要手动返回一个结果(response.json(),js底层会自动将这个结果包装成一个fulfill状态的promise)
   	fetch发送的请求，如果请求的地址不存在则直接抛出一个 rejected状态的promise实例对象(该状态中的值为错误信息);如果存在则抛出一个 fulfill状态的promise实例对象(它的值是本次请求结果)，当我们使用then方法调用时，js底层会对该请求进行解析(检查是否匹配到路由和是否由结果)，如果该路由有返回数据则继续执行本then方法的第一个函数，如果该路由没有返回数据(说明路由匹配失败)，会立刻抛出一个 rejected状态的 promise,里面存储报错信息
  */
  ```
  
  

+ 组件间通信

  ```java
  /*
   1. props 可以实现 父向子通信 
   2. ref 可以实现 子向父通信
   3. 
  */
  ```

  



## 前言

IDE 集成开发环境 WebStorm，将所有需要的环境都配置好了

Visual Studio Code: 渐进式开发工具，只支持一个壳，额外功能需安装插件

React文件引入原则，含有js,jsx后缀的可只写文件名，引入Index文件可只引入文件夹，react内部会查找index进行引入

在return里只能写语句不能做赋值操作，所以赋值操作要在render里return外进行，然后再把值写在return里

+ vscode使用

  ```js
  /*
   ctrl+shift+p打开控制面板
   键盘快捷方式可以设置快捷键
   用户代码片段可以自定义代码($1标识让光标停留在此处)
   alt+左回退到上一步 alt+右回退到下一步
   ctrl+p 快速打开文件
   ctrl+g 快速切换行
  */
  ```
  
  

声明式：不亲自操作DOM,只声明数据回来去哪里

命令式：亲自操作DOM

模块话： js

组件化：页面上一个功能点所有代码资源集合

+ React特点

  Declarative(声明式编程)

  Component-based(组件化编码)

  Learn Once,write Anywhere(支持客户端与服务端渲染)

  **高效（DOMdiff算法，编码人员不需操作DOM），React会在内存中虚拟开辟一个DOM当操作全部完成后，一次性返回结果，极大减少了页面重绘重排**

  单向数据流

  

  

## 1.0 react 入门

+ React基本使用

  ```js
  /*
  react 资源 
  <script src="https://cdn.bootcdn.net/ajax/libs/react/18.1.0-next-aa05e7315-20220331/umd/react.development.js"></script>
  
  再开始编写React代码时需要引入三个库 react源生库，reactDOM库，babel翻译库
  	<script type="text/babel"> // 使用react写下的是jax语法，必须用babel进行翻译，type类型不能丢
      let vDOM = <h1>Hello World</h1> //创建虚拟dom,写下的内容不能用引号包裹，否则按字符串处理
      ReactDOM.render(vDOM,document.querySelector('#test')) //借助react把dom渲染到页面上
      </script>
      
  */
  ```

+ 使用js创建虚拟dom

  ```js
  /*
  注意
  	<script type="text/javascript"></script> 比 <script type="text/babel"></script>查询优先级高，且<script type="text/javascript"></script> 先执行
  
   1.使用js语法创建虚拟DOM
     let myID = 'atzhangsan'
      let myData = 'ewZHANGSAN'
      //创建一个虚拟dom标签，该api接收三个参数，1 为标签名，2 为 该标签的属性 ， 3 为 内容
      let vDOM = React.createElement('h2',{class:myID.toLowerCase()},myData.toLowerCase())
      // 渲染虚拟DOM ，render 接收两个形参 ， 1 为插入的内容 ， 2 为插入的dom标签
      ReactDOM.render(vDOM,document.querySelector('#test1')) // 元素创建太繁琐，若有多层嵌套代码冗杂
   
   
   2.使用Jsx语法创建虚拟DOM(创建的元素只能有一个根包裹标签)
    //使用React创建的虚拟DOM更加直观，javascript内部引用参数需用{}括起来，整体可以用()包起来，提示为一个语句。再引用class需用className方式(因为js已经有class类概念)
     <script type="text/babel">
      let vDOM1 = (
        <div>
          <h3 id={myID.toUpperCase()}>
            <span className="red">
              {myData.toUpperCase()}
            </span>
          </h3>
          <h1>哈喽</h1>
        </div>
      )
      ReactDOM.render(vDOM1, document.querySelector('#test2')) 
    </script> 
    
   3.虚拟DOM本质
    是js种的一般Object对象，虚拟DOM比较小和真实DOM相比少了很多API
   */
  ```

+ jsx语法

  ```js
  /*
  1.常规概念
   全称 javascriptXML, react定义的一种类似于XML的JS扩展语法：XML+JS,本质是React.createElement(component,props,...childrens)方法的语法糖，用来简化创建react虚拟DOM元素对象。
   标签名任意：HTML标签或其他标签
   标签属性任意：HTML标签属性或其他
   
  2. 基本语法规则
   （1）遇到<开头的代码，以标签的语法解析：html同名标签转换为html同名元素，其他标签需特别解析
   （2）遇到{开头的代码，以js语法解析：标签中的js表达式必须用{}包含
   （3）babel.js作用，浏览器不能直接解析jsx代码，需要借助babel转义为纯js代码才能执行，只要用了jsx，都要加上type="text/babel",声明需要babel处理
   （4）js语句和js表达式
   	表达式：一个表达式会产生一个值，它可以放任何需要一个值的地方，如3+b,arr.forEach()都有返回值，即所有可以用变量接收的语句都可以算表达式
    (5)只有(<>)里包裹的元素才需要加{}(把改元素包装称react对象)再分解
  */
  ```



+ 模块与组件

  ```js
  /*
   模块
   	向外提供特定功能的js程序，一般是js文件，作用：复用js,简化js编写
   组件
   	用来实现特定界面功能效果的代码和资源的集合(html/css/js/image),作用复用编码，简化项目编码，提高运行效率
   模块话
   	当应用js都以模块编码写的，这个应用就是模块化
   组件化
   	当应用以多组件的方式实现，这个应用就是一个组件化应用
  */
  ```

  

+ 组件使用

  ```js
  /*
  1. 组件创建有 工厂函数，类创建两种方法，都需要用<Conponent/>方式引入，它们的命名都需要首字母大写。在类中主要是使用类里面的render方法去得到渲染返回结果
  2.组件嵌套返回，直接再后面接其他组件的<Conponent/>即可，需要注意的是要添加一层外包裹元素
  3. 在</>中想写number和js表达式都需要用{}包裹 
  4. 简单组件无状态，复杂组件有状态
  5. 工厂函数的this指向undefined。类组件中，Render方法里的this指向组件类
  6. 表单元素也要以<input type="text"/> 结尾
  7. 以</>方式引用的类组件本质是在操作React.Component里的全局类属性，所以this指向一致，指向同一块类组件空间，使用自己的类组件的construcotr创建(new 对象方式)的不互通，指向new出来的新空间，且rop,ref,state属性不互通。
  8. 在类组件中，以<Con/>方式引用的类，自己后来额外添加的函数this指向为undefined(不包括继承函数重写),继承的内部函数和自己的constructor&&render函数this指向同一块内存，以new Con新增的类this指向自己独有的内存
  */
  
   <script type="text/babel">
      // 使用工厂函数定义组件，工厂函数必须大写开头
      //  jsx标签必须闭合(即再表便末尾谢上 /)
      // 嵌套其他工厂函数直接 < MyHa /> 引入即可
      function MyComponent() {
        return (
          <div>
            <h1>hello</h1>
            <MyHa />
          </div>
        )
      }
      function MyHa() {
        return <h1>哈哈</h1>
      }
      /*
       如果渲染标签时，首字母是小写的，系统会尝试着寻找html标签池中与之对应的标签渲染，未找到报错
       如果渲染标签时，首字母是大写的，尝试着寻找组件是否定义，定义则渲染
       */
      /*</>做了两件事,寻找MyComponent函数是否定义，若定义了,判断其是否为工厂函数，若是，则返回并渲染*/
      ReactDOM.render(<MyComponent />, document.querySelector('#example1'))
      console.log(new MyComponent())
      // 使用es6类组件
      class MyComponent2 extends React.Component {
        render() {
          return <h1>我是es6类组件(复杂的)</h1>
        }
      }
      console.log(new MyComponent2('zs'))
      // 使用类创建组件主要是借助该类里的render方法，需写上继承对象 React.Component
      ReactDOM.render(<MyComponent2 />, document.querySelector('#example2'))
    </script>
  ```

+ 组件三大属性

  ```js
  /*
   1.state
   (1)state是组件实例对象(只有class有权限才做state)最重要的属性，值是对象（可以包含多个key-value）
   (2)组件被称为 状态机，通过更新组件的state来更新对应页面显示
   (3)React支持再函数行内添加事件(和外部添加事件效果一致)
   (4)再React定义的类中，只有constructor，全局定义类变量和render里的this指向实例对象，其余的this指向undefnied， 想让其他函数this指向实例对象使用bind()函数
  (5) state不能直接修改和更新，需要使用this.setState()来设置状态
  （6）当状态改变时，才会重新渲染页面，不能再render里直接操作状态
  
   class Weather extends React.Component {
        constructor(a) {
          super(a)
          this.state = { isHot: false }
          this.demo = this.demo.bind(this) //改变this.dome函数this的指向
        }
        // demo函数必须写在class里，这样才能实时更改状态
        demo() {
          let { isHot } = this.state
          console.log(this)
          this.setState({ isHot: !isHot }) // 设置或更新状态
        }
        run() {
          console.log(this)
        }
        render() {
          return <h1 id="e" onClick={this.demo}>今天天气太{this.state.isHot ? '热' : '冷'}了</h1>
        }
      }
      ReactDOM.render(<Weather />, document.querySelector('#test1'))
   只要状态发生更改，render就会渲染1+n
    
  
  2.prop
  prop可以动态保存传入的参数(num,str,function,obj...)到一个该类的实例对象属性里
  	传参方式必须为 ReactDOM.render(<Person name="z3" age={18} sex="male" />, document.querySelector('#test1'))，其中 ame="z3" age={18} sex="male"是传递给prop类的参数，以键值对的形式存储。也可以在外部定义一个对象，把这些属性以键值对形式写入然后以 {...obj}方式进行写入到prop <Person {...obj} />，{}在react中是js语法的展开符，其内部配合babel帮我们进行了对象展开(仅在</>中有效)，在常规js语法中要使用{...obj}的方式展开对象，是es8新语法可以对一个对象进行展开
  	
  设置必须传入参数及默认传入参数
   // 限制传参以及设置默认值 15.5开始已弃用
   15.5前需在PropTypes前加React, React.PropTypes.string.isRequired,
   15.5后只需写 PropTypes.string.isRequired
  
    // 使用react想页面插入数据
      class PropsTest extends React.Component {
  
          constructor() {
              super();
              console.log(this);
              this.state = {isHot: false};
          }
  
          changeState = () => {
              console.log("我被调用了");
              this.setState({isHot: !this.state.isHot});
          }
  
          render() {
              return (
                  <div>
                      <div onClick={this.changeState} className="weather">今天天气非常{this.state.isHot ? "热" : "冷"}</div>
                      <div className="list">
                          <ul>
                              {
                                  Object.keys(this.props).map((item, index) => {
                                  return <li key={index}>{item + " -> " + this.props[item]}</li>
                              })}
                          </ul>
                      </div>
                  </div>
              )
          }
      }
  
      // 限制prop接收参数
      PropsTest.propTypes = {
          name:PropTypes.string.isRequired, 
          age:PropTypes.number,
          sex:PropTypes.string
     }
  
      // 在不传入参数时,prop的默认类型
      PropsTest.defaultProps = {
          age :13,
          sex: "未知"
      }
        
   3.refs
   refs为类组件里的一个属性，里面存储的是键值对信息，可以用三种方法定义
    （1）可能弃用refs写法 
    在refs属性里添加一个键值对对象 使用 this.refs.input2获取input表单元素
    <input type="text" ref="input2" onBlur={this.blurEnd} />
    获取值：this.refs.input2.value
    （2）回调refs写法
     在组件类属性里添加一个input属性，value为input,使用this.input获取表单元素
     <input type="text" ref={input => this.input = input} /> 
     获取值：this.input.value
    （3）createRef写法
    调用React.createRef即创建了一个ref容器，该容器专人专用(只保存一个ref),再次重写会把之前的覆盖(所以用此方法写的容器需要声明n个React.createReft)
    myRef = React.createRef()
    <input type="text" ref={this.myRef} />
    获取值：this.myRef.current.value
  */
  ```

  

+ 组件化编程流程

  ```js
  /*
  React原则，状态在那个类里，就在哪个类里改变
   1.拆分组件：拆分界面，抽取组件
   2.实现静态组件：使用组件实现静态页面效果
   3.实现动态组件
   	动态显示初始化数据
   		数据类型
   		数据名称
   		保存那个组件
   		交互
   4.onSubmit在表单提交的时候触发(有些时候需要住阻止默认行为)，onchange在内容发生改变时触发
  */
  ```

+ 受控组件，非受控组件

  ```js
  /*
   数据是往state里存的标识受控组件
   数据不往state里存的标识非受控组件
  */
  ```

+ 生命周期函数(旧)

  ```js
  /*
   1. 在jax里写style标签需要用两个{{}}，第一个标识js表达式，第二个标识为一个对象，样式和样式之间用逗号间隔
   jax中 style={{ opacity: this.state.opacity, color: "#ace" }}
   js中 style="opacity: .8;width:500px;height:300px"
   
   2. 生命周期函数也叫生命周期钩子
   有三个过程：挂载，更新，卸载组件
   工作过程：初始化render,组件将要挂载，render完成渲染，挂载完成，组件将要更新，组件更新完毕
   
   3.生命周期函数(旧)
   组件挂载(在render第一时间渲染完毕时自动触发)
   componentDidMount() { // 一般在此函数发送异步ajax请求，定时器等
          setInterval(() => {
            if (this.currentNum <= 0) {
              this.currentNum = .9;
            }
            this.currentNum -= this.changeNum
            console.log(this.currentNum)
            this.setState({
              opacity: this.currentNum
            })
  
          }, 200)
        }
    
  组件更新（在render更新时，自动触发此函数）
  componentDidUpdate(){
  
  }
  
  组件卸载（在组件将被销毁时自动触发）
   componentWillUnmount() { // 组件将被卸载时自动触发，一般做一些收尾工作，如关闭定时器等
          console.log(5)
        }
  death = () => { // 执行此函数标识删除组件，继而触发componnetWillUnmount
          ReactDOM.unmountComponentAtNode(document.querySelector('#test'))
        }
  
  
  其他生命周期函数
  constructor(){} // 在每次引用或生成实例对象时自动执行
  componentWillReceiveProps(){  // 此函数在第二次(包含第二次)以后获得的参数时，自动触发
  
  }
  componentWillMount() {} // 组件将要挂载完成时更新(少用)
  shouldComponentUpdate(){} // 每次更新组件时执行，询问是否更新，返回true&false
  componentWillUpdate(){} // 组件将要更新时触发
  this.forceUpdate() // 强制更新
  4.总结
   【初始化】
    触发条件： ReactDOM.render(<Component/>)
    constructor()
    componentWillMount()
    render()
    componentDidMount()
    【更新】
    触发条件： this.setState({})
    componentWillUpdate()
    render()
    componentDIdUpdate()
    【卸载】
    触发条件： ReactDOM.unmountComponentAtNode()
    componentWillUnmount() // 收尾工作 ，如关闭定时器，只执行一次
  */
  ```
  
  ![image-20220406195601900](D:\typora_import_images\typora-user-images\image-20220406195601900.png)旧

+ 声明周期函数(新)

  ```js
  /*
   1. 即将废弃的钩子函数
   componentWillMount()
   componentWillReceiveProps()
   componentWillUpdate()
   
   2. 新增钩子函数
   （1）static(在类中需要加) getDerivedStateFromProps(props,state){} props,state都是解构后的对象 ,取代了componentWillMount和componentWillUpdate,getDerivedFromProps，返回值必须是一个对象或null,返回的对象会挂载在状态中，如果状态有重名则覆盖之前的，没有重名就添加一组新的键值对
   
   （2）getSnapshotBeforeUpdate(props,state){},需要返回参数或者null，这个返回参数会传递给componentDidUpdate(props,state,data) 中的data参数，可以把getSnapshotBeforeUpdate(props,state){}和axio.interceptors.response.use()类似，都是做一个中间处理
  */
  ```

  ![image-20210809100451985](C:\Users\zbcz\AppData\Roaming\Typora\typora-user-images\image-20210809100451985.png)新





+ 虚拟DOM Diff算法

  ```js
  /*
   初始化显示界面
   	创建虚拟DOM数(是js一般对象，用到文档碎片优化)，真实DOM树，绘制界面显示，setState()更新状态，重新创建虚拟DOM树，新/旧树比较差异，更新差异对应真实DOM，局部重绘
  */
  ```

  ![image-20210809111721560](C:\Users\zbcz\AppData\Roaming\Typora\typora-user-images\image-20210809111721560.png)

+ key标识

  ```js
  /*
   作用：key是虚拟DOM的标识符，当状态数据发生变化时，React会生成新的虚拟DOM节点，然后React会把新的DOM节点和旧的DOM节点作Diff比较
   若旧的虚拟DOM中如果找到了与新的虚拟DOM相同的key，则会对比它们的差异，若没变化，则直接使用原来的真实DOM,不刷新页面的真实DOM，若有变化，则先更新虚拟DOM，随后刷新页面的真实DOM
   若旧的虚拟DOM中没找到虚拟DOM，则渲染到真实DOM页面
   若把key的值设置为一个可能发生改变的值(如下标)，那会产生没必要的真实DOM更新，因为key的值一直在和原来的值想改变，会一直触发局部重绘重排，再者，一些标签相同，可标签里的输入内容不同的标签不会被刷新，因为Diff对比差异是对比标签本身和字符串内容的差异，并非查找到输入的内容，所以会产生错位(如input:text　标签)
   
   总结: key的值一定要设置为一个唯一身份标识符,避免信息错位和无意义重绘重排
  */
  ```

  

## 1.1.1 React脚手架

refs属性可以给标签和组件添加，添加后可以调用该标签或组件的属性和方法
为了避免setState冲突，一个函数最好只设置一个this.setState（如果只能设置多个最多设置两个，如果报错则改为一个）

+ 下载react脚手架库

  ```js
  /*
   创建项目并启动
   安装脚手架:npm i create-react-app -g
   创建项目: create-react-app hello-react(创建脚手架的名字)
   启动: 切换到下载项目目录 使用 npm start开启项目
  */
  ```

+ React结构&标签讲解

  ```js
  /*
  public 文件夹下的内容
  	index.html 下的内容
           <link rel="manifect" href="xx.json"> // 用于给网站加壳,使它能称为app
           <meta name="theme-color" content="#000"> // 更改安卓手机地址栏颜色
           <meta name="description" content="Web site created using create-react-app"/> // 项目上线		时的网站描述
           <noscript>you should agree this React.js runing in the app</noscript> //不能执行script代	   码时会执行这个标签
           <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" /> // 设置在IOS系统中,被收藏	    的应用图标
           
      manifest.json , 此文件为页面打包成app时的配置，写的不全，使用HBuilder打包即可
   	robots.txt ，此文件规定了爬虫允许获取的内容
   
   src 文件夹下的内容
       App.test.js ,用于项目测试使用
       reportWebVitals.js — 页面性能分析文件（需要web-vitals库的支持）
       setupTests.js — 组件单元测试的文件（需要jest-dom库的支持）
       
   注意
  	在idnex.js页面渲染时,可以直接获取public下的index.html里的标签(React内部完成链接,文件夹不能更改)
  	如果文件夹或文件改过名可能导致编译出错,这时需要重启react服务器
  
  在组件类命名时最好与模块名引入模块值一致，符合开发规范，虽然组件中的类名值可有可无，最终还是以引入为准，但一致时会有代码提示
  
  组件的js文件一般写为jax，主js文件一般以App.js命名，入口文件以index.js命名（.jax和.js可以省略，不建议省）
  
  在初级阶段可以先搭建静态页面，然后把内容粘贴到React App.js中，注意<input/>等单回路标签要闭合 。 React不希望a标签有网址(后期解决) 可以先用#1锚点占位
  
  所有成型的第三方库编写的代码可以放到public文件夹,引入给index.html即可(如 bootstrap)。当使用bootstrap字体时，会调用对应的字体文件，如果没找到才会报错。如果按css模块引入会立刻扫描这个文件，看到字体路径不合理会立即报错
  
  书写的静态页面复制到React文件中，需要把class写的类该为 className,input标签要闭合
  <input type="text"/>
  
  写项目时，要多写注释，在css样式时需要些注释表明 ...开始 ...结束
  
  在划分模块时要把这一部分的html和css都带过去，利于模块话拆分和开发
  
  key是内部参数 不属于 props
  
  
  */
  ```
  



+ uuid

  ```js
  /*
   可以创建一个完全无冲突的唯一id
   安装： npm i uuid
   引入： let uuid = require('uuid')  import引入可能报错
   使用
   1、uuid.v1(); -->基于时间戳生成  （time-based）
  
  2、uuid.v4(); -->随机生成  (random)
  */
  ```

+ state一些细节&&常用细节

  ```js
  /*
  0. 把看到的交互视作一种状态，并单独开一个对象控制
  
  1.state 不能直接更新修改和更新数据(在相同地址中)
   state在更新状态的时候才会重新渲染页面，所有在使用结构赋值这种方法获得状态时，可能react不承认更换了状态导致不会重新刷新页面，因为使用结构赋值更新的状态实际上还是指向同一块地址，在这个地址上进行修改，react有时候不认可这种写法，
   如 let {test} = this.sate   这种的地址值一致(复杂数据类型地址传递，简单数据类型值传递，因此简单数据类型可以使用此方法改变状态)。所以复杂数据类型在获得state状态值时要使用...运算符展开数据在进行传题(值传递) 如 let test = [...this.state.comments]
   
   总结：react的状态改变是根据地址值和原有值都改变的情况下才渲染的，只有对象和数据可以使用扩展运算符(使用扩展运算符返回的是一个新的地址)
   
   2.再触发一个事件又想传递参数时
   再触发一个事件(所有事件都有环境变量)又想传递参数时，可给事件指定一个匿名函数，在匿名函数中调用其他函数并传入参数即可 如 onClick=()=>{this.delete(a)}
   
   3.window.confirm在React中不能省略window
  */
  ```



## 1.1.2 react ajax

React本身只关注界面，并不包含发送ajax请求，前端应用需要通过ajax请求和后台交互需要用到ajax库

+ 常用ajax请求库

  jQuery:比较重，如果需要例外引入不建议使用

  axios:轻量级，建议用

  fetch：原生函数，老版本浏览器不支持

  原生ajax请求

  fly



+ 常见问题

  ```js
  /*
   1. 如果使用 axios 页面请求失败时，使用err.message拿到错误信息(字符串类型)状态，直接获取err会导致程序异常(err是一个Object对象)
   
   2. stackoverflow 常见代码错误解决平台
   
   3. 在component文件夹中区分各个模块后，可以把各个模块都用index.jsx命名，这样就只用指定到index的上级目录就可以访问index.jsx文件(React会自动引入index.jsx文件)
   比如 component/show/index.jsx 引入时可以简写import Show from './component/show',React会自动去查找index.jsx文件
  */
  ```

  

+ fetch

  ```js
  /*
   0. 关注者分离，把每个模块做的事情划分开，比如fetch就采用了这个原则，第一个.then判断是否链接成功，第二个.then获取返回数据(依附于第一个.then return JSon数据)，若失败则传递给Catch打印错误信息
   
  1.在使用fetch时不能直接返回json格式数据，因为其只会判断域名是否符合(匹配则返回json文件执行第二个.then)，而不会判断路由是否匹配。,所以需要自己做判断，不能盲目返回xx.json().
   
   2. 在fetch判断第一个.then时可以通过状态码等来判断
   （1）域名路由都正确，状态码为200,ok为true，
   （2）域名正确路由不正确，状态码404,ok为false,
   （3）域名不正确，直接执行catch语句
  */
  ```

  

+ 组件间通讯

  ```js
  /*
   1.消息订阅与发布
   要使用PubSubJS工具库，npm安装 npm i pubsub-js 
   安装后配合componentDidMount()钩子函数设置交流密文完成持续监听，当有其他模块传递正确密文时，立刻把信息传入
   接收模块（默认pubsub模块引入）：
    componentDidmount(){
    	PubSub.subscribe('list',(meg,data)=>console.log(meg,data))
    }
   发布模块(默认pubsub模块引入):
    PubSub.publish('list',{a:1,b:2}) // 接收模块会接收{a:1,b:2},并存储data里
    
    2. 所有通讯方式总结
     （1）通过props传递
     	   Ⅰ 共用的数据放在父组件上，特有的数据放在自己上
     	   Ⅱ 通过props可传递一般属性和函数属性，只能一层一层传递
     	   Ⅲ 一般属性->父组件传递数据给子组件->子组件读取数据
     	   Ⅳ 函数属性->子组件传递数据给父组件->子组件调用函数
     （2）使用消息订阅与发布机制
         Ⅰ传递消息便捷，可跨等级传递数据，适用于任意组件通讯
         Ⅱ多用可能导致报文出现雷同产生冲突，
         Ⅲ 不能集中式管理
         
     （3）
  */
  ```

  

## 1.1.3 react  router

+ 前端路由

  ```js
  /*
   安装包 npm i react-router-dom ,即可安装web路由
   react-router-dom时react的一个插件库，专门用来实现一个SPA应用，基于react项目基本都会用到此库
   1.SPA
   	(1) 单页Web应用
   	(2) 中各应用只有一个完整页面
   	(3) 点击页面的链接不会刷新页面，本身不会想服务器发起请求
   	(4) 当点击路由链接时，只会做页面的局部更新
   	(5) 数据需要通过ajax请求获取，并在前端异步展现
   	
   2. 使用react-router-dom包
    (1) 引入 import { Link, BrowserRouter } from 'react-router-dom'
    
    (2) 包裹路由<BrowserRouter><Link to='./about.html'>xxx</Link></BrowserRouter>可同时包裹多个Link路由 to写明相对路径路由，当点击到此Link标签时，自动切换到to引入的路由下,同时地址栏会变为/xxx(to指定的路由)
    
    (3) <Link>跳转的路由不能增加高亮效果，需使用<NavLink>才能添加高亮，需要写activeClassName才能添加样式，否则默认添加active样式。样式可能添加不上此时需要!important来提高权重，只有当前路由的样式会发生改变
    
    (4) <Route path="/xxx" component={Xxx}/> 用来协助<NavLink>完成对应页面的渲染，当url路径为/a时，只会匹配<Route path='/a' conponent={A}> 路径，其余路径不匹配，必须用<Browser>包裹，和if判断相似，Route可以看作为多组if判断
    
    (5) 可直接在index.js主引入模块中写下包裹<Browser><App/></Browser>，避免使用多组Browser导致代码冗杂
    
    (6) <Switch></Switch>可以帮助route加快效率，在第一时间匹配到路由后，不会继续往下查找。不引用Switch则路由会一直挨个匹配，导致效率降低,一般最频繁访问的路由就要越往前放
    
    (7) 自己定义且以<Com/>方式引入的组件为一般组件，如果组件依靠路由渲染为路由组件。通常一般组件放到Component里，路由组件放在pages文件夹里
    
    (8) 当路由写为<Route path="/" component={Com}>,标识匹配所有路由，在前端路由中 / 采用模糊匹配方法即，只要我写的path内容能完美匹配上你url上的一部分就算匹配成功(后端路由为精准匹配) 如  path为 /he  我的url路径为 /he/hello 也会被/he 匹配上，同理，当 path 为 / 时，标识所有路由等能匹配 因为url中一定包含 / ,为了解决这个问题需要把模糊匹配转换为精准匹配 在Route中添加属性 exact <Route path="/" component={Com} exact>此路由就会精准匹配
    
    (9) 在引入Route匹配二级路由时，会丢失public文件夹里引入的文件(引入路径发生改变)，因为写./是按照当前文件夹来的，直接变为 /xxx/则是按服务器根路径路径来的
     解决方法1 复制它引入其他link时的神秘代码 %PUBLIC_URL% 用他替换掉 ./xxx中的.
     解决方法2 直接删除./xxx中的. 变为 /xxx/ 则可以完美匹配
     解决方法3 使用HashRouter，不使用BrowserRouter
     
   (10) <Redirect/>重定向，写在所有路由最后面，当所有标签都没匹配上时，它会进行匹配，快跳转到你指定的路由点
   
   (11) 一般react精准匹配用的较少 大多把多级路由(难匹配)写在前方，低级路由(易匹配)写在后方
   
   (12) 当在切换路由时，当前路由显示，前一个路由会被卸载
   
   (13) HashRouter 会以/#/ 作为开头标识是前端路由，是h5提出的概念
   
   (14) 前端路由切换会留下历史记录(符合堆结构，先进后出，后进先出)，每次点击一个前端路由都会留下Push操作(历史记录)，使用replace可以一直替换当前堆底的前端路由(只会留下一次历史记录，点击回退是回退到上一个非前端路由控制页面)，解决push前端路由过多问题
   
   (15) 路由组件组件的this.props自带一些环境参数，如history,location,match等  ，    （1）history中常用的属性和方法有 goBack,goForward,push,replace
     （2）location中常用的有 pathname, match中常用的有 params
     （3）params可以获取路由的/xx/xx里的参数并以对象形式存入params
  如 <Link to={`/home/msg/detail/eq`}> < Route path="/home/msg/detail/:id" component={Detail} /> ，:id接收/eq里的数据以 {id:eq}保存
     （4） React环境变量history主要是靠history库实现
   
    (16) withRouter 属性，是一个高阶组件，可以把一个普通组件包装成ui组件，使用ui组件中的history,match等属性 
    	使用： Header = withRouter(Header)
  
   
    应用： 页面中出现选项卡切换效果时使用，
    （1）找到html中 选项卡 的位置 Link 带有高亮效果Navlink ,编写路由链接
    （2）找到html中 切换内容 的位置 把展现不同的内容，单独抽离成组件
    
    总结：前端路由采用了单页面多组件的风格，react-router-dom包中常用插件为 Link,BrowserRouter,NavLink,Route，Switch,HashRouter,exact,Redirect,activeClassName
  */
  ```

  

+ 组件间传参小细节

  ```js
  /*
   1.  自封装button组件按钮
   当给组件传参时可以以 <Com>点击</Com> 方式传递，默认会被接收到 this.props.children下 ，在另一个组件中可以使用  {...this.pros} 展开，children指向的值会自动输出到值上去 
   如 <Button>点击</Button>（默认Button被引入）  (Button组件写法) <button {...this.props}></button>
   
   2.  普通组件用 <He> 路由组件用{Hel} 普通图片和变量用{we}
  */
  ```



+ mock.js，用于模拟数据库信息



## 1.1.4 antd

antd组件本质为把所有的api排列组合形成对应效果

Input组件相同name值会一起控制

给一个函数设置多个 this.setState可能导致程序异常，最好只设置一个

antd组件的显示与隐藏只用操作其对应的关键状态即可

再状态里的数组要使用 [...this.state.xx]方式扩展才能使用数组的属性和方法，否则只能使用字符串的

+ 表单提交&antd按需引入

  ```js
  /*
   1. 安装antd: npm i antd , 再App.css引入 import 'antd/dist/antd.css'
   
   2. 按需引入(用到什么引入什么，要不然antd会全部引入)
   	(1) 下载 @craco/craco , npm i @craco/craco
   	(2) 配置 package.json
   		"scripts": {
             "start": "craco start",
             "build": "craco build",
             "test": "craco test",
          }
       (3) 再根目录创建 craco.config.js 修改默认antd配置
       (4) 自定义主题色
       	引入 less-loader 和 less(帮助编译less)一级craco-less                  安装 npm i less less-loader craco-less 。修改 craco.config.js
       	const CracoLessPlugin = require('craco-less');
          module.exports = {
            plugins: [
              {
                plugin: CracoLessPlugin,
                options: {
                  lessLoaderOptions: {
                    lessOptions: {
                      modifyVars: { '@primary-color': '#1DA57A' },
                      javascriptEnabled: true,
                    },
                  },
                },
              },
            ],
          };
   
   3. 表单验证规则
    	rule有两种适配方案，1按照已给API自定义限制，2按照以给API给定挟制
    	(1) 按照API给定限制
    	rules={[
            { required: true, message: 'Please input your username!',},
            {max:14,message:'输入的位数必须小于14位'},
            {max:4,message:'输入的位数必须大于4位'},
            {pattern:/^\w+$/,message:'输入的账号或密码必须由数字字母或下划线组成'}
          ]}
       
      (2) 按照API自定义限制
      	需要传三个参数，返回的为Promise对象，rule标识该表单配置，value标识表单值，str标识自己传入的标量
      verifyForm = (rule, value, str) => {
          if (value == null) return Promise.reject(new Error(str + '不能为空'))
          else if (value.length < 4) return Promise.reject(new Error(str + '长度要大于等于4位'))
          else if (value.length > 12) return Promise.reject(new Error(str + '长度要小于等于12位'))
          else if (!/^\w+$/.test(value)) return Promise.reject(new Error(str + '需由数字，字母或下划线组成'))
          else return Promise.resolve()
        }
        
  4. Form提交验证
  	(1) Form下有环境API,这时可以把他存到ref里，然后当按钮点击时，调用此ref下的Form下的validator方法,返回的是一个promise对象，可以用 async await拆包
  	 formRef = React.createRef()  // 默认被class包裹
        formCommit = async () => {
          try {
            const values = await this.formRef.current.validateFields();
            console.log('Success:', values);
            // message是antd模块下的一个组件，可以美观的在屏幕上弹出错误信息 message('错误信息'[,显示秒数])
            message.error('输入正确，即将进入..')
          } catch (errorInfo) {
            console.log('Failed:', errorInfo);
             message.error('输入格式不匹配，请重新输入')
          }
        }
        <Form ref={this.formRef}></Form>
        
  5. 清空form表单输入框(默认被class包裹)    
  ref={(form) => this.form = form}
  cancelModal = () => {
      console.log('取消了')
      this.setState({
        visible: false
      })
      this.form.resetFields()
    }
    
  
  6. Form组件使用注意点
  	(1) 被设置了name属性的Form.Item控件，表单空间会自动添加value,onChange,数据也会统一交给Form表单处理。这意味着不用使用onChange来监听数据变化，不能使用value和defaltValue设置初始值，此时不能使用setState而要使用form.setFieldValue来动态改变表单值
  */
  ```

  

+ 高阶函数&高阶组件

  ```js
  /*
   高阶函数：如果一个函数接收一个函数作为参数，则此函数为高阶函数
   高阶组件：如果一个组件接收一个组件，并把这个加工组件返回出去就为高阶组件
   组件的本质是函数
  */
  ```




+ antd layout使用

  ```js
  /*
   1.在引入antd布局时，可能出现元素不展开，在器外侧元素(Layout)上添加height:100%即可
  */
  ```

+ antd 使用menu导航菜单

  ```js
  /*
  1. 使用menu导航菜单
   menu导航菜单一般和路由使用再一起，再包裹时，路由应再<Item>组件内包裹
   如 <Item><Link to="/home">首页</Link></Item>
   
   2. icon使用map遍历问题，遍历不出来效果，须在config中把icon直接写为<Username/>(不能使用字符串，{<{}>}嵌套会报错)格式在遍历时即可
  */
  ```




+ table&&card

  ```js
  /*
   找到响应组件引入即可
   1.table 参数
   	columns={columns}  // 设置列头规则
      dataSource={data}  // 获取对应得列资源
      size="default"  // 设置尺寸
      bordered={true}  // 设置边框
      pagination={{ 
      pageSize: PAGE_SIZE， // 设置一个表格显示几行
      total: TOTAL, // 设置以共多少条数据
      current: NUMBER, // 设置当前默认选择第几列
      }} 
      rowKey="_id"    // 设置key值，会去找当前行列下得_id属性内部把它设置位改元素得key值,如果不使用rowKey参数，则需要在获取列表时用map挨个遍历给它们添加key关键字
          result.data.list.map(item => {
            return item.key = item._id
          })
   	loading = {}  // 设置是否加载，是一个布尔值 
   	(1) agination :可以改变一页显示多少个元素 如 pagination={{pageSize:5}}
   	
   	(2) rowKey: 可以代替react key值(接收唯一标识符，再后端传递得数据没有key选项得情况下)，可接收字符串，函数
   2. columns 参数 
   	title:标识列头id
   	dataIndex:标识所管元素
   	key: 唯一标识符
   	className:可以为当前列位设置类名
   	render:(item)=>{return<Button onClick={()=>{this.cl(item)}}/>} 此项用来生成对应行的元素，其中环境形参item(可以为任意值)可以拿到该列dataIndex的所有值,例如再请求后端拿到返回参数时，dataIndex可以写为 dataIndex:xxx格式(与后端返回key相匹配)render形参即可保存其数据，在必要时,配合事件可以完成对应逻辑和参数携带。若没有dataIndex属性，则该形参拿到是本行的所有数据
   3. columns和dataSource
   (1)columns用于设置该列样式及标识所管列，title属性设置列头名称，dataIndex用于收集dataSource中对应得资源，把它们整合到一列，如 dataIndex:'name',意为收集dataSource中所有得name都这一行
   (2)dataSource以数组嵌套对象格式展示出可选得属性
   如  {
      key: '1',
      name: 'John Brown',
    },
    {
      key: '2',
      name: 'Jim Green',
    },
    
   4. 如果想要给Item内的表单设置初始值需再Item下添加 initialValue=xxx属性
   如  <Item
                name="name"
                initialValue={this.state.itemNameInTable}
                rules={[
                  // rule和value都为环境变量，rule保存的是该标签的信息，value保存的是数值
                  { required: true, message: '信息不能为空' }
  
                ]}
              >
                <Input className="site-form-item-icon user-icon" placeholder="请输入信息" />
  
  5. 当一个Input被Form包裹时，想要给Input添加默认表单显示，只能给Form添加使用initialValue设置初始值，想要这个值动态变化需要使用form.setFieldsValue
    如 setTimeout(() => { 
            this.form.setFieldsValue({
              itemName: this.itemName // 指定一个键值对，键用来指向对应的数据，然后把这个键作为Item name值
            })
             // 需要给setFieldsValue设置一个定时器，把它放到任务队列里，避免因它先给值，而产生报错
          }, 0);
      <Item name="itemName"> .... </Item>
      
    例 1 
        setTimeout(() => {
            this.form.setFieldsValue({
              name: this.state.name, // Item name值：状态值
              desc: this.state.desc,
              price: this.state.price,
              categoryId: this.state.categoryId
            })
          }, 0);
     
     
  6. 当调用后端修改状态接口数据成功后，页面往往不会自动刷新(需更改状态，或重新获取列表)，当重新获取列表时，如数据量过大则不太适用，可使用改变状态方法，重新对比校对数据，再利用React Diff校验 达到完美加载效果
   修改状态代码
    productList = [...this.state.productList]
   if(status === 0) {
   	productList.map(item=>{
   		if(item.id === _id){
   			return item.status = status
   		}
   	})
   }
  */
  ```



+ 分页

  ```js
  /*
   及页面底端显示得当前页数，通常为 1 2 3 ... 最后一个，分，真分页 和假分页
   
   再antd周搜索分页即可查看分页组件
   
   分页分为前端分页，和后端分页
   前端分页： 一次性返回所有数据，由前端人员进行数据的切割，整理，划分页数，当数据量足够大时，会产生页面卡顿或浏览器崩溃
   后端分页： 返回的是一部分数据，需请求时指明每页显示多少条，再那一页，交由后端进行数据切割，后台需要明确，每页显示多少条，同时后台返回全部数据个数给前端
  */
  ```
  



+ 表格布局流程

  ```js
  /*
   一般会使用Card组件进行包裹，然后再里面使用Input和Button，Table等
  */
  ```




+ antd 插件 onchange事件细节

  ```js
  /*
   1.在 Select中 onChange的第一个形参就是表单里的值，也就是defaultValue值
   	如 <Select defaultValue="Description" onChange={v=>console.log(v)}/>  此形参v拿到的就是defaultValue的值
   2.在 Input中 onChange的第一个形参就是环境变量，使用event.target.value拿到值，该值为表单里实时输入的内容
  */
  ```
  



+ React解决htmlDOM数据

  ```js
  /*
   有时候我们向后端发起请求获取数据时，可能会返回一些html标签，这些标签不会被React解析，且原样输出，如果想让这些标签生效，需要加上dangerouslySetInnerHTML属性  
   如 <span dangerouslySetInnerHTML={{__html:this.state.desc}}></span>
  */
  ```




+ antd栅格系统

  ```js
  /*
   antd中把栅格系统分成了24份
  */
  ```



+ upload组件

  ```js
  /*
   用于上传图片等文件
   1. state中的参数
    state = {
      previewVisible: true, // 是否展示预览图
      previewImage: '', // 要预览图片的地址
      previewTitle: '', // 域栏图片的id
      fileList: [
        {
          uid: '-1', // 唯一标识符
          name: 'image.png', // 图片名
          status: 'error', // 当前图片的状态 uploading done remove
          url: 'https://zos.alipayobjects.com/rmsportal/jkjgkEfvpUPVyRjUImniVslZfWPnJuuZ.png', //图片地址
        }
   2. Upload携带参数
   <Upload
            // action 接收图片的服务器地址
            action={`${BASE_URL}/manage/img/upload`}
            method="post" // 请求方式，默认Post
            name="image" // 发给服务器的key
            listType="picture-card" // 展示图片方式
            fileList={fileList} // 图片列表，一个数组里面包含多个图片对象
            onPreview={this.handlePreview} // 点击预览按钮的回调
            onChange={this.handleChange} // 图片状态改变回调，里面包含了图片信息，和图片状态码，有 {file(里会返回是否上传成功，和图片地址等),fileList,event}
          >
            // 设置图片最大图片数量 
            {fileList.length >= 4 ? null : uploadButton}
          </Upload>
          
      // 上传，完成，删除都会触发onChange
      handleChange = ({ fileList, file, event }) => { 
  
          console.log(file)
          if (file.status === 'done') { // 图片上传完成
            console.log(fileList)
            fileList[fileList.length - 1].url = file.response.data.url // url该为服务器地址
            fileList[fileList.length - 1].name = file.response.data.name // name和服务器存储名关联
          }
          this.setState({ fileList })
        };
  3. 使用注意
  	在上传图片时需要把服务器返回状态的地址维护到本状态中，这样才能和服务器数据进行关联，否则前端做的操作，服务器不会发生修改
  */
  ```
  



+ 富文本编辑器

  ```js
  /*
   wysiwyg 库，专门为react定制
   1.安装包  npm i react-draft-wysiwyg draftjs-to-html html-to-draftjs draft-js
   2.打开网址 https://jpuri.github.io/react-draft-wysiwyg/#/demo ，复制自己需要的富文本
   3. 引入样式(系统不会自动引入)
    import 'react-draft-wysiwyg/dist/react-draft-wysiwyg.css'
    
   4. 将html语义化标签文本转换为正常文本(数据回写)
   	setRichText = (html) => {
      console.log(html)
      const contentBlock = htmlToDraft(html);
      if (contentBlock) {
        const contentState = ContentState.createFromBlockArray(contentBlock.contentBlocks);
        const editorState = EditorState.createWithContent(contentState);
        this.setState({ editorState })
      }
    }
  */
  ```




+ Model组件

  ```js
  /*
   可以设置点击出现对话框效果，通过设置visible={}状态方式
  */
  ```

+ tree组件

  ```js
  /*
   ztree和easyUi为jqeury树插件
   tree为antd组件
   
   1. 标签分析
    state = {
      expandedKeys: ['0-0-0', '0-0-1'], // 配置默认展开的节点
      autoExpandparent: true, // 设置是否自动展开父节点
      checkedKeys: ['0-0-0'], // 默认选中的元素
    };
    
     <Tree
              checkable  // 增加复选框
              onExpand={this.onExpand} // 展开时触发
              expandedKeys={this.state.expandedKeys} // 运行时，展开的树
              autoExpandParent={this.state.autoExpandParent} // 自动展开父节点
              onCheck={this.onCheck} // 当选择复选框后触发,自带环境形参，第一个为当前选中的元素
              checkedKeys={this.state.checkedKeys}
              onSelect={this.onSelect} // 当点击文本时触发，自带环境形参，第一个为当前点击的元素
              selectedKeys={this.state.selectedKeys} // 选中的key
              treeData={treeData}
              defaultExpandAll = {true} // 展开所有列表
            />
  */
  ```

  

## 1.1.5 react redux

redux用于收集一些公共操作的状态，私有状态内部修改即可

this.setState是异步任务，无法在同一个函数中设置和获取状态值(搭配定时器或设置全局属性可以获取想要的状态值)

+ redux集中式状态管理工具

  ```js
  /*
   1. redux是一个独立抓门用于做状态管理的JS库，它可以用再react,angular,vue等项目中，但基本配合react  . 作用：集中式管理(读/写) react应用中多个组件共享状态
   2. redux应用场景
   	总体原则：能不用则不用，如果不用比较吃力才考虑使用(出现在复杂交互中)
   	某个组件的状态，需要共享
   	某个组件需要再仍和地方都可以拿到
   	一个组件需要改变全局状态
   	一个组件需要改变立一个组件的状态
  */
  ```

  ![image-20210814150530032](C:\Users\zbcz\AppData\Roaming\Typora\typora-user-images\image-20210814150530032.png)

+ redux三个核心概念

  ```js
  /*
   1. action
   	标识要执行行为的对象，包含2个方面的属性，type:xxx,值为字符串，唯一必要属性。xxx:xxx 数值属性，值类型任意，可选属性
  */
  ```

  

+ redux使用

  ```js
  /*
   1. 安装redux  npm i redux ,创建redux文件夹，在里面创建reducer.js,和store.js，store文件夹需引入 redux下的createStore方法，并和reducer创建关联
   2. 核心store文件与index对话书写为 <App store={Store}>
   3. 再reducer.js中需要做状态初始化，同时reducer是一个函数接收两个参数，第一个参数为状态值，第二个参数为操作行为(dispatch)，再此函数中做判断推荐使用switch
  */
  ```

+ redux使用注意事项

  ```js
  /*
    1. 再reducer.js中不能修改传递过来的参数
    2. 当store状态改变时，不会重新渲染页面，因为其没有渲染权限，需要利用他的方法store.subscribe(()=>{<App store={store}>,xxx})借助ReactDOM.render渲染
    3. store一般只和index.js对话
    4. createStore下的dispatch方法用于传递action
    5. createStore下的getState用于获得改变状态值(伪)
    6. createStore下的subscribe方法再每次dispatch后都会触发，再index.js页面中使用可以配合页面渲染，redux并没有渲染页面的权限
    7. reducer(proviousState = initState, action) ，proviousState和action都由store传入，proviousState标识传入状态(内部由getState方法获取)，action由我们调用dispatch传入再有store转接
    8. reducer必须给定一个初始状态，因为getState()一开始获取不到值，当他第一次执行reducer函数后既获取了状态值
    9. actionCreators 和params_include配置js文件夹出现是为了模块化管理状态，使结构变得清晰
    
   store运行流程： 首先redux.createStore创建的实例对象给index.js引入，然后把store对象传递给app.js,app.js使用其下的dispatch()变量传递值给store，store内部处理把此值保存,并当作reducer函数中的action形参的值传入，reducer函数中的proviousState是由其下的getState()推动，由于程序刚运行没有状态导入，所以要再其上设置一个初始状态，当一个流程完毕后，可以获取到状态值，其也失效，store本质上没有页面渲染能力，当他的状态改变时，需其下的subscribe方法和index.js中的ReactDOM.rende配合实现页面再次渲染
  一般为了app页面显示参数整洁明了，故使用ActionCreator.js加工{type:xx,data:xx}对象后返回，因其中可能涉及到过多的变量所以要专门给ActionCreator.js配置专门的js文件 
   */
  ```



+ react-redux

  ```js
  /*
   1.React-Redux将所有组件分为两类
   	(1)UI组件
   		只负责UI呈现，不带任何业务逻辑(if,setTimeout等)，通过props接收数据，不适用任何Redux的API,一般保存再components文件下
   	(2)容器组件
   		负责管理数据和业务逻辑，不负责UI呈现
   		
   2. react-redux使用
   	(1) react-redux的出现更加规范了组件的使用和组件的分别，使每个组件都可以改变Redux的状态，而不经过App Prop传递
   	(2) 组件分Ui和容器后，在容器中引入 import { connect } from 'react-redux' 以及 import Counter from '../components/counter'和之前封装好的actionCreator  import { createAdditionAction, createSubtractAction } from '../redux/actionCreators'
   	(3) 使用 解构出来的 connect()函数完成组件封装，connect((state函数状态),(操作状态的方法))(绑定的ui组件)  
   	export default connect(state => ({ count: state }), {
        addition: createAdditionAction,
        subtract: createSubtractAction
      })(Counter)
      其中createAdditionAction函数(我们已封装)会返回一个{type:xx,data:xx}对象，当这个属性在Counter ui组件中被调用时，会自动转换并调用dispatch函数传递给store,sotre在转交给reducer处理 ，进而改变状态
  */
  ```
  



+ react-redux使用注意点

  ```js
  /*
   1. index.js也可引入 react-redux 使用解构出来的 Provider把store对象传递给App , ReactDOM.render(<Provider store={store} ><App /></Provider>, document.querySelector('#root')) 此总方法可以将store传递给全局react,当其他组件需要connect创建时，第一时间完成渲染，完美代替了 subscribe方法
   
   2. 在app.js引入中只引入他对应的容器组件即可，因为容器组件包含了ui组件，这样可以达到继承渲染效果
   
   3.connect传递规则为 connect(函数，对象{函数})(ui组件)，这些函数下的属性绑定在ui组件下的props对象下，第一个函数专门用于获得当前状态，第二个函数用于改变函数状态，在使用中ui组件直接调用方法传递数据即可，此函数内部会自动dispatch给store，进行状态校对
   	export default connect(state => ({ count: state }), {
        addition: createAdditionAction,
        subtract: createSubtractAction
      })(Counter)
  
  工作流程：首先，在我们封装好store后，把他引入到index.js中，然后下载 react-redux,把它也引入到index.js中，解构出 Provider属性 包裹在 App上，并给Provider 传递一个props,store={store},Provider会把store进行分发，和实时监听状态变化，当状态改变时，他会立刻重新渲染页面。第二，我们进行ui组件和容器组件封装，把ui组件和react-redux引入到容器组件中，对react-redux进行解构，得到connect方法，在其中我们可以给改ui组件下的props添加state(受store管理)，和操作reducer的方法，在ui界面像操作状态时，直接对props下的方法进行调用即可
  
  4. 在没有react-redux之前，store只能和index对话，并把store传递给App，使App能操作dispatch,getState等，不能跨阶段和阶级传递，而react-redux则解决了此问题，谁需要操作store状态，直接使用connect方法连接即可
  
  5. action操作代码启停流程，reducer负责改变状态
  
  6. 为了避免文件过多，通常会把ui和容器写在一个文件里
  
  7. Provider是顶级组件
  */
  ```

  

+ redux异步编程

  ```js
  /*
   用于解决异步任务(如定时器)不返回对象问题
    1.安装：  npm i redux-thunk
    2.使用
  	(1)在store.js中 写入
  	import { applyMiddleware, createStore, Middleware } from 'redux'
      import reducer from './reducer' 
      import thunk from 'redux-thunk'
      export default createStore(reducer, applyMiddleware(thunk))
      (2)在改容器组件中写下对应的actionCreator中的函数
      (2)在actionCreator中给对象的调用语句写成以下形式即可解决异步问题
      export const createAsyncAddAction = (value, delay) => {
            return (dispatch) => {
              setTimeout(() => {
                dispatch(createAdditionAction(value))
              }, delay)
            }
          }
  */
  ```

  

+ redux开发者工具

  ```js
  /*
   1.谷歌商店下载 redux DevTools ，
   2.下载后需安装 npm i redux-devtools-extension 
   3. 在store.js引入 
    import { createStore } from 'redux'
  import reducers from './reducers'
  import { composeWithDevTools } from 'redux-devtools-extension'
  
  export default createStore(reducers, composeWithDevTools())
  */
  ```

  

+ 多Ui多容器组件开发

  ```js
  /*
   1. 一般一个项目一个store有n个reducers和actions，它们的文件夹都位于redux下，为了统一管理需要把所有的reducers收集到本文件夹的index.js里
   	import counterReducer from './counter_reducer'
   	import personReducer from './person_reducer'
   	import {combineReducers} from 'redux'
   export defualt	combineReducers({  // 把它们各自reducers状态收集到combineReducers对象里
   		count: counterReducer,
   		person: personReducer,
   	}) 
   	这样书写后，获取响应reducers的状态就要用 state.xxx 方式来获取 如state.count
  */
  ```

+ 多Ui多容器组件开发注意点

  ```js
  /*
   1. 多组件开发以当前文件夹的index为收集点，传递给store.js，做组件分发
   2. connect中，第一个函数的环境形参可以拿到当前全部组件的状态点，并以对象的形式存储，获取一个值时要以 state.xx方法 如 { count: state.count, person: state.person }
  */
  ```

  

+ 高阶函数和纯函数

  ```js
  /*
  1.纯函数
       一类特别的函数：只要是同样的输入(实参)，必定得到同样的输出(返回) 
       如function dome(a){console.log(a)} dome(a)调用三次
       纯函数必须遵守以下一些约定
       	(1) 不得改写参数数据
       	(2) 不会产生任何副作用，列入网络请求，输入和输出
       	(3) 不能调用Date.now()或Math.random()等不纯的方法
    	 redux的reducer函数必须是一个纯函数
   
   2.高阶函数
   	形参接收的是函数，返回的也是函数，常见的高阶函数有 定时器，Promise,forEach等
  */
  ```

  

+ postman使用

  ```js
  /*
   1. 发post请求时，参数需要由body携带，还需选择 x-www-form-urlencoded
   2. 身份识别主要是看其携带的token是否准确
   3. 当在测试接口时，如需要登录权限，可在登录后复制登录的token，然后在测试其他需要token的接口时，把刚刚复制的token放到HEader中 以 Authorization:xxx(token) 形式携带即可验证身份
  */
  ```



+ 跨域问题

  ```js
  /*
   1.跨域本质是服务器收到了请求且返回了数据，但被https拦截了
   2.常见解决跨域问题的方法为 jsonp ,cors,和创造一个代理,这个代理可以接收数据且不会被拦截，同时还可以把数据带给用户
   3. 代理搭建， 将 "proxy":"http://localhost:4000"添加到package.json所有配置，然后连接此代理即可，这个代理和我们是同一地址，所以没有跨域问题。代理服务器本质为转发请求
   如 axios.post('https://localhost:3000/login', values)
   
   4. 代理为React私有功能，只有React可以使用
   
   5. 当向服务器发起请求时，如果是对象的形式 如 {name: tom, pwd: 123}
  那么服务器需使用 app.use(express.json()) 解析 ， 如果是name=tom&pwd=123的形式，则可以按传统post请求处理 
  
   6. 如果后端不允许传递json数据，则可以使用querySting包(react已安装)，它可以把对象转换为字符串
   
   7. 项目在开发环境中时，可以不指定地址，默认为自己本机地址，但在生产环境中必须指明地址
   
   注意事项： 在传递参数时，先与后端协商，看后端的跨域是否写了Cors,或json等
  */
  ```
  
  

+ 使用axios拦截器返回promise成功回调利用async await接收

  ```js
  /*
  1. 在书写axios请求时，可以使用拦截器代替.then和.catch避免主文件代码过多，请求拦截器可以加工请求数据，而响应拦截器可以对数据进行判断，如果数据判断成功直接返回第一个函数的值，在主函数使用async await即可获取数据，如果失败则返回第二个函数的值，为了终端Promise链，通常返回Promise.rejct('xxx')抛出错误，而await 接收不到失败数据为undefined，从而施加判断条件
   
   主文件关键代码(async关键字写在包裹函数上)
  let result = await reqLogin(values)
        let { status } = result
        if (status === 0) message.success('连接成功')
        else if (status === 1) message.warning('账号或密码错误')
        else message.warning('网络错误') //await获取不到状态码执行此句
        
   拦截器关键代码 
  export const loginAxios = axios.create({
    timeout: 2000
  })
  loginAxios.interceptors.request.use((config) => {
    if (config.method.toUpperCase() === 'POST' && config.data instanceof Object) {
      config.data = qs.stringify(config.data)
    }
    return config
  })
  
  loginAxios.interceptors.response.use(
    // 借用了promise思想，成功(找到服务器) 执行第一个函数，失败(没找到)执行第二个函数
    response => response.data,
    error => error // 返回一个await能给解析的值(也就是resolve值，没有中断await链)
  */
  ```

  

+ NProgress

  ```js
  /*
   可以展示一个加载进度条(欺骗性)
   下载包  npm i nprogress 
   引入 import NProgress from 'nprogress'
  	 import 'nprogress/nprogress.css'
  	 NProgress.start() 开始展示进度条
  	 NProgress.doen() 关闭进度条
  */
  ```

  

+ 没登陆跳转登陆页面

  ```js
  /*
   1. render()必须写return ,最好和<Redirect/>搭配使用
  */
  ```

  

+ 装饰器语法

  ```js
  /*
   @dome class MyClass{}  // 把class作为实参传递给dome函数
   function dome(target){
    	target.a=1
   }
  */
  ```



+ 常见状态码

  ```js
  /*
   401 没有权限访问(在没登录的情况下查看页面)
   404 没找到对应页面
  */
  ```

  

+ token

  ```js
  /*
   1.身份的唯一标识符，在注册成功时会生成一个唯一的token，在每次登录时和查看其它页面时都需要携带此token
   2. token永远不会通过url参数携带，而是以公共请求头添加，公共给请求头需要一个自定义头和token才能完成认证，这种方式比较安全
  */
  ```

  ![image-20210818092316961](C:\Users\zbcz\AppData\Roaming\Typora\typora-user-images\image-20210818092316961.png)

+ 容器组件，普通组件，路由组件，ui组件

  ```js
  /*
   在只使用前端路由时，组件分普通组件(component文件夹下)和路由组件(page文件夹下)
   在使用前端路由基础上使用redux时，则只分为ui组件(component下)和容器组件(container下)
  */
  ```

  

+ 校验身份是否失效

  ```js
  /*
   if (error.response.status === 401) { // 状态码为401标识没权限
        message.error('身份验证失败请重新登录')
        // 清除token
        store.dispatch(deleteLoginInfoAction())
      }
  */
  ```

  

+ 全屏显示库

  ```js
  /*
   安装： npm i screenfull
   使用 screenfull.toggle()方法即可完成全屏切换
   
   关键代码
   import screenfull from 'screenfull'
   state = {
      isFullScreen: false
    }
    fullScreen = () => {
      screenfull.toggle()
  
    }
    componentDidMount() {
      screenfull.on('change', () => {
        let { isFullScreen } = this.state
        this.setState({
          isFullScreen: !isFullScreen
        })
      })
    }
   
   全屏显示会有bug,当用户第一次按下f11时，全屏功能会失效
  */
  ```

  

+ 优雅的交互框(antd提供)

  ```js
  /*
   可代替原生的confirm或alert,原生的太不友好，且样式无法修改
   1. 进入antd ->组件->反馈
  */
  ```

  

+ localstorage

  ```js
  /*
   localstorage存储复杂数据类型数据时，需要JSON.stringify()转换为字符串存储，在获取对应复杂数据类型数据时，需要使用JSON.parse()解析为识别类型，否则可能出现重大程序故障
  */
  ```

  

+ 获取当前时间库

  ```js
  /*
   npm i dayjs
   
   dayjs(value) 如果有value则按value时间来，如果没有value就按当前时间来
   
   获得当前时间 
   state = {
   	date: dayjs().format('YYYY年 MM月DD日 HH:mm:ss')
   }
  */
  ```

  

+ 获取实时天气接口

  ```js
  /*
   https://tianqiapi.com/api.php
  */
  ```

+ 访问其他服务器跨域问题

  ```js
  /*
   使用axios发起请求时，若后端没设置跨域选项，则会出现跨域问题，这时可以使用jsonp库解决
   安装 npm i jsonp
  */
  ```

  

+ 公共资源

  ```js
  /*
   把公共资源放到public目录下， 由于组件不能跨出src引入文件资源，所以再src下要创建一个static目录，src内的公共资源引入static,public同级的公共资源引入publicz
  */
  ```

  

+ 再一串数组对象中查找值得最好方法

  ```js
  /*
  1.利用全局类属性保存key值，这样笔避免了forEach递归传递错误值问题，forEach无法退出，会一直遍历完数组里得所有值
  以下代码默认被class包裹
    title = null
    getTitle = (arr, value) => {
      // 出现点击bug的原因，每次点击都是把全部的config拿去给forEach遍历
      //  title = this.getTitle(item.children, value) 可以拿到值，但只能拿到最后一个子导航的值，因为会一直遍历完,title值在变化
      //所以只能使用find查找
      arr.find(item => {
        console.log('forEach  ')
        if (item.children instanceof Array) {
          this.getTitle(item.children, value)
        } else {
          if (item.key === value) {
            this.title = item.title
          }
        }
      })
    }
  */
  ```

  

+ 当使用了connect连接状态时，第一个参数不能为空需要返回一个对象

  ```js
  /*
   export default connect(
    state => ({}), // state不能为空，可以返回一个空对象
    { saveProduct: saveProductMessage }
  )(Product)
  */
  ```

  

+ connect工作流程

  ```js
  /*
   connect用来给指定组件添加操作store公共状态的属性，第一个参数用来设置获取公共状态，为 state=> {return {xxx:state.xxx}} 或 state=> ({xxx:state.xxx})（括号表示一个整体，括号内是一个对象，箭头函数只有一条语句时可以省略{}且会返回这一个对象），第二个参数用来操作公共状态，一般为 {xxx: (actions)} actions为自己书写的一个js库，里面封装的返回值为 {type:xxx,data:xxx} 返回后connect自动会做dispatch操作，转交给store,store在分配给对象的reducer处理，在reducer中第一个参数为获取到的全局状态(store.getState()第一次需要自己设置初始值，因为没有设置全局状态)，第二个参数为接收到的action值，配合action里的type和data以及switch完成状态更新，处理完后暴露此reducer给store(reducer封装一个Index.js用于汇总)，自己保留一份状态，然后返回给store一个状态，在connect取值时，state存储的就是大的状态，state.xxx 就是获取对应reducer里的状态
  */
  ```



+ 在查找中find和filter的区别

  ```js
  /*
   find 返回的是对象
   filter 返回的是数组
  
  
  */
  ```

  

## 1.1.6 逻辑技巧

+ 权限技巧

  ```js
  /*
   在遍历菜单时做一个判断，看其是否拥有此权限如果有则为true，没有为false
  */
  ```

  

+ 数据可视化

  ```js
  /*
   可以快速高效的生成一个指定图表
   https://echarts.apache.org/examples/zh/index.html
  */
  ```

  

